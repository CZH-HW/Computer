[TOC]

# Kafka 基本框架
Kafka是分布式的流处理平台。它最初由LinkedIn(领英)公司发布，使用Scala语言编写，与2010年12月份开源，成为Apache的顶级项目。Kafka是一个高吞吐量的、持久性的、分布式发布订阅消息系统。它主要用于处理活跃的数据(登录、浏览、点击、分享、喜欢等用户行为产生的数据)。
三大特点：
1. 高吞吐量（生产消费）
   可以满足每秒百万级别消息的生产和消费。
2. 持久性（中间存储）
   有一套完善的消息存储机制，确保数据的高效安全的持久化。
3. 分布式（整体健壮性）
   基于分布式的扩展和容错机制，Kafka的数据都会复制到几台服务器上。当某一台故障失效时，生产者和消费者转而使用其它的机器。


![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_1.png)
![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_2.png)

主要由Producer、Consumer、Broker、Topic、Partition、

### Broker
![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_3.png)

Broker 是 Kafka 实例，每个服务器上有一个或多个 Kafka 的实例
可以简单地认为一台 Kafka 服务器就是一个 broker，一个 Kafka 集群由多个 Broker 组成。
每个 Kafka 集群内的 Broker 都有一个不重复的编号，Broker-0、Broker-1 等…… 


Broker 接收来自生产者的消息，为消息设置偏移量，并提交消息到磁盘保存。
Broker 为消费者提供服务，对读取分区的请求做出响应，返回已经提交到磁盘的消息。
Broker 是集群 (Cluster) 的组成部分。每一个集群都会选举出一个 Broker 作为集群控制器 (Controller)，集群控制器负责管理工作，包括将分区分配给 Broker 和监控 Broker。
在集群中，一个分区 (Partition) 从属一个 Broker，该 Broker 被称为分区的首领 (Leader)。一个分区可以分配给多个 Brokers，这个时候会发生分区复制。这种复制机制为分区提供了消息冗余，如果有一个 Broker 失效，其他 Broker 可以接管领导权。


![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_8.png)



### Topic与Partition

- Topic：消息的主题，可以理解为消息的分类。在每个 Broker 上都可以创建多个 Topic；
为了实现扩展性，一个非常大的 Topic 也可以分布到多个 Broker 上。
<br>

- Partition：Topic 的分区，每个 Topic 可以有多个 Partition，每个 Partition 由一个一个消息组成。
同一个 Topic 在不同的 Partition 的数据是不重复的，Partition 的表现形式就是一个一个的文件夹！

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_4.png)


#### Topic（逻辑层面）

如下图所示：
创建 Topic1 和 Topic2 两个 Topic，分别有 13 个和 19 个 Partition 分区，则整个集群上会相应会生成共 13+19=32 个文件夹（Topic1 和 Topic2 replication-factor 均为 1），如下图所示。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_topic_1.png)


#### Partition offset
Partition 中的每条消息都被标记了一个 sequential id，也就是 offset，并按顺序存储在 partition 中。

offset的顺序性不跨Partition，由于一个 Topic 包含多个 Partition，因此无法在整个 Topic 范围内保证消息的顺序性，但可以保证消息在单个 Partition 内的顺序性。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_partition_2.png)

这种组织和处理策略提供了如下好处：
1. 消费者可以根据需求，灵活指定 offset 消费。
<br>
2. 保证了消息不变性，为并发消费提供了线程安全的保证。每个 Consumer 都保留自己的 offset ，互相之间不干扰，不存在线程安全问题。
<br>
3. 消息访问的并行高效性。每个 topic 中的消息被组织成多个 partition，partition 均匀分配到集群 server 中。生产、消费消息的时候，会被路由到指定 partition，减少竞争，增加了程序的并行能力。
<br>
4. 增加消息系统的可伸缩性。每个 topic 中保留的消息可能非常庞大，通过 partition 将消息切分成多个子消息，并通过负责均衡策略将 partition 分配到不同 server。这样当机器负载满的时候，通过扩容可以将消息重新均匀分配。
<br>
5. 保证消息可靠性。消息消费完成之后不会删除，可以通过重置 offset 重新消费，保证了消息不会丢失。
<br>
6. 灵活的持久化策略。可以通过指定时间段（如最近一天）来保存消息，节省 broker 存储空间。
<br>
7. 备份高可用性。消息以 partition 为单位分配到多个 server，并以 partition 为单位进行备份。备份策略为：1个leader 和N个 followers，leader 接受读写请求，followers 被动复制 leader。leader 和 followers 会在集群中打散，保证 partition 高可用。
<br>


#### Partition 是如何存储数据的？

从上面的图可以知道每个 Partition 其实都会对应一个日志目录：`[topicName]-[partitionid]/`。

**日志是一种存储抽象，只能追加按照时间完全有序的记录序列，是一种有持久性保证和强有序的语义消息系统。日志是分布式一致性的底层实现基石。**

在此目录下会对应多个日志分段(Log Segment)，每个 Segment 文件又包含 `.index`文件（索引文件）、`.log`文件（数据文件）、`.timeindex`文件（早期版本中没有）三个文件。

Segment 文件命名规则：
1. 每个名字有 20 个字符，不够用 0 填充。
2. 每个名字从 0 开始命名，下一个 Segment 文件的名字就是上一个 Segment 文件中最后一条消息的 offset 值。

如下图所示：
![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_partition_3.png)

`.log`文件就是实际存储消息的地方，而`.index`文件和`.timeindex`文件为索引文件，用于检索消息。如下图所示：

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_partition_4.png)

- 索引文件存储大量元数据，索引文件里元数据指向相应数据文件里 message 的物理偏移地址。
<br>
- 索引文件不是对每条消息都做记录，它是每隔一些消息记录一次，避免占用太多内存。
<br>
- 以索引文件里元数据`3,497`为例，依次在数据文件里表示第 3 个 message (在 partiton 中表示第 368772 个 message)、以及该消息的物理偏移地址为 497。

**注意：注意索引文件中物理偏移地址的不是消息的 offset，而是 message 在 segment 中的物理位置。**
<br>

日志文件的消息结构如下图所示：

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_partition_5.png)

主要包含消息体、消息大小、Offset、压缩类型……等等！
- Offset：Offset 是一个占 8byte 的有序 id 号，它可以唯一确定每条消息在 Parition 内的位置！
<br>
- 消息大小：消息大小占用 4byte，用于描述消息的大小。
<br>
- 消息体：消息体存放的是实际的消息数据（被压缩过），占用的空间根据具体的消息而不一样。


#### Partition 消息存储策略

对于传统的Message Queue而言，一般会删除已经被消费的消息。

而Kafka集群会保留所有的消息，无论其被消费与否。当然，因为磁盘限制，不可能永久保留所有数据（实际上也没必要）。

因此Kafka提供两种策略删除旧数据：一是基于时间，二是基于Partition文件大小。
- 基于时间，默认配置是 168 小时（7 天）。
- 基于大小，默认配置是 1073741824。

需要注意的是，Kafka 读取特定消息的时间复杂度是 O(1)，即与数据大小无关
所以这里删除过期的文件并不会提高 Kafka 的性能！


#### Partition 中如何通过 offset 查找 message ？

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_partition_4.png)

例如在上图读取 offset = 368776 的 message，需要通过下面2个步骤查找。

1. 第一步二分查找 segment file：
`00000000000000000000.index`表示最开始的文件，起始偏移量（offset）为 0；
第二个文件`00000000000000368769.index`的消息量起始偏移量为 368770 = 368769 + 1；
同样，第三个文件`00000000000000737337.index`的起始偏移量为 737338=737337 + 1；
......
后续文件依次类推，以起始偏移量命名并排序这些文件，
只要根据 offset **二分查找**文件列表，就可以快速定位到具体文件。
例如当 offset = 368776 时定位到`00000000000000368769.index|log`
<br>

2. 第二步通过 segment file 查找 message：
当 offset=368776 时，去`00000000000000368769.index`文件中查找索引为 7（=368776-368769）或者最接近并小于 7 的元数据`6,1407`，此时就相应得到了这条索引所在的物理位置为 1407，然后再通过 00000000000000368769.log 顺序查找直到 offset=368776 为止。


由于 Kafka 消息数据太大，如果全部建立索引，会占用更多存储空间，所以 Kafka 选择了**稀疏索引**的方式。

稀疏索引占用的索引存储空间比较小，但是查找时间较长。

#### Partition 备份机制

- 每个 Partition 都有一台 server 作为 “leader”，零台或者多台 server 作为 follwers。
<br>
- leader server 处理一切对 partition （分区）的读写请求，而follwers只需被动的同步leader上的数据。当leader宕机了，followers 中的一台服务器会自动成为新的 leader。
<br>
- 每台 server 都会成为某些分区的 leader 和某些分区的 follower，因此集群的负载是平衡的。


### Producer

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_producer_1.png)

#### Producer 向 Broker 发送消息的基本流程：

1.确认发送消息所在的目标分区，Kafka Producer 提供了一个默认的分区器 Partitioner。

- 对于每条待发送的消息而言，如果该消息指定了 key，则 Partitioner 会根据 key 的哈希值(然后对分区数量取余)来选择目标分区，将具有相同Key的所有消息都路由到相同的分区中；
<br>
- 若该消息未指定 key，则 Partitioner 使用轮询的方式确认目标分区。
<br>    
- 另外 Producer 的 API 赋予了用户自行指定目标分区的权力，即用户可以在消息发送时跳过 Partitioner 直接指定到要发送到的分区。

2.确认分区后，Producer 要做的第二件事是寻找该分区对应的 leader，也就是该分区 leader 副本所在的Kafka broker。

- 每个分区都由若干个副本组成，其中一个副本充当 leader 角色，只有 leader 能响应 clients 发送的请求，剩下的副本中有一部分副本会与 leader 副本保持同步，即所谓的 ISR(In-Sync Replicas, 副本同步队列)。

- 因此，在发送消息时，producer可以有多种选择来实现消息发送，例如不等待任何副本的响应便返回成功，或者只是等待leader副本响应写入操作后再返回成功。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_producer_2.png)

代码层面流程（Java）：
producer首先使用一个线程(用户主线程，也就是用户启动producer的线程)将待发送的消息封装进一个ProducerRecord类实例，然后将其序列化之后发送给partitioner，再由后者确定了目标分区后一同发送到位于producer程序中的一块内存缓冲池中。而producer的另一个工作线程(I/O发送线程，也称Sender线程)则负责实时地从该缓冲区中提取准备就绪的消息封装进一个批次(batch),统一发送给对应的broker。

Kafka producer发送消息的主方法是send方法，producer在底层完全实现了异步化发送，并且通过Java提供的Future同时实现了同步发送和异步发送+回调(Callback)(默认异步)两种发送方式。最后producer程序结束时需要关闭producer。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_producer_3.png)

1、构建一个KafkaProducer对象，初始化一些用到的组件，比如缓存区，Sender线程等

      2、如果配置了拦截器，可用对发送的消息进行可定制化的拦截或更改

      3、对Key,value进行序列化

      4、根据传入的参数，为消息选择合适的分区，具体怎么选，后面分析

      5、将消息按照分区发送到RecordAccmulator暂存，消息按照每个分区进行汇总

      6、后台Sender线程被触发后从RecordAccmulator里面获取消息然后构建成ClientRequest，怎么构建后面分析

      7、将ClientRequest封装成NetWorkClient准备发送

      8、NetWorkClient将请求放入KafkaChannel准备发送，然后执行网络IO，最后发送到kafka server


#### Producer 如何保证消息的完整性 ？

问：Producer 在向 Kafka 写入消息的时候，怎么保证消息不丢失呢？

答：**通过 ACK 应答机制**（用来平衡吞吐量与可靠性）。在 Producer 向集群写入数据时设置`request.required.acks`参数来设置数据可靠性的级别：这个参数可设置的值为`0`、`1`、`all`：

- `0` 代表 Producer 往集群发送数据不需要等到集群的返回，不确保消息发送成功。这种情况下数据传输效率最高，但是数据可靠性确是最低的。
- `1` 代表 Producer 往集群发送数据只要 Leader 应答就可以发送下一条，只确保 Leader 发送成功，如果 Leader 宕机了，则会丢失数据。这是Kafka的默认配置。
- `all` 代表 Producer 往集群发送数据需要所有的 Follower 都完成从 Leader 的同步才会发送下一条，确保 Leader 发送成功和所有的副本都完成备份。安全性最高，但是效率最低。

最后要注意的是，如果往不存在的 Topic 写数据，能不能写入成功呢？

Kafka 会自动创建 Topic，分区和副本的数量根据默认配置都是`1`。


### Consumer

#### Consumer Group
之前我们知道传统的消息队列有两种模式：点对点模式（队列）、发布订阅模式

Kafka 通过 consumer group 将两种模式统一处理：

consumer group是kafka提供的可扩展且具有容错性的消费者机制。既然是一个组，那么组内必然可以有多个消费者或消费者实例(consumer instance)，它们共享一个公共的ID，即group ID。组内的所有消费者协调在一起来消费订阅主题(subscribed topics)的所有分区(partition)

系统将 consumer group 按名称分组，**将消息复制并分发给订阅了的分组，每个分组只有一个 consumer 能消费这条消息**。如下图：

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_7.png)


1）任意消费组内，每个partition有且仅有一个consumer，因此kafka保证每个partition的消息处理是无锁且有序的，因此kafka才有大吞吐量

2）任一partition会被每个消费组消费，因此当需要对同一份message处理两种逻辑，可以分为两个消费组

3）GroupId是topic下的一个子概念，不同topic的groupId没有任何关系，即使他们的值相同





两个极端情况：
当所有consumer的consumer group相同时，系统变成点对点模式
当每个consumer的consumer group都不相同时，系统变成发布订阅


注意
1、Consumer Groups 提供了topics和partitions的隔离， 如上图Consumer Group A中的consumer-C2挂掉，consumer-C1会接收P1,P2，即一个consumer Group中有其他consumer挂掉后能够重新平衡。如下图：






一般消息系统，consumer存在两种消费模型：

 push：优势在于消息实时性高。劣势在于没有考虑consumer消费能力和饱和情况，容易导致producer压垮consumer。
 pull：优势在可以控制消费速度和消费数量，保证consumer不会出现饱和。劣势在于当没有数据，会出现空轮询，消耗cpu。
kafka采用pull，并采用可配置化参数保证当存在数据并且数据量达到一定量的时候，consumer端才进行pull操作，否则一直处于block状态。kakfa采用整数值consumer position来记录单个分区的消费状态，并且单个分区单个消息只能被consumer group内的一个consumer消费，维护简单开销小。消费完成，broker收到确认，position指向下次消费的offset。由于消息不会删除，在完成消费，position更新之后，consumer依然可以重置offset重新消费历史消息。




