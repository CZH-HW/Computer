[TOC]

### Kafka 简介
Kafka是分布式的流处理平台。它最初由LinkedIn(领英)公司发布，使用Scala语言编写，与2010年12月份开源，成为Apache的顶级项目。Kafka是一个高吞吐量的、持久性的、分布式发布订阅消息系统。

三大特点：
1. 高吞吐量（生产消费）
   可以满足每秒百万级别消息的生产和消费。
<br>
2. 持久性（中间存储）
   可进行持久化操作，将消息持久化到到磁盘，以日志的形式存储，因此可用于批量消费，例如ETL，以及实时应用程序。 
   通过将数据持久化到硬盘以及 replication 防止数据丢失。
<br>
3. 分布式（整体健壮性）
   基于分布式的扩展和容错机制，Kafka 的数据都会复制到几台服务器上。当某一台故障失效时，生产者和消费者转而使用其它的机器。
<br>

### Kafka 拓扑结构
![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_1.png)

|名词|解释|
|----|----|
|Producer|消息的生成者|
|Consumer|消息的消费者|
|ConsumerGroup|消费者组，可以并行消费 Topic 中的 Partition 的消息|
|Broker|缓存代理，Kafka 集群中的一台或多台服务器统称 Broker|
|Topic|Kafka处理资源的消息源（feeds of messages）的不同分类|
|Partition|Topic 物理上的分组，一个 Topic 可以分为多个 Partition，每个 Partition 是一个有序的队列。Partition 中每条消息都会被分配一个有序的 Id（offset）|
|Message|消息，是通信的基本单位，每个 Producer 可以向一个 Topic（主题）发布一些消息|

<br>


### Broker
![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_3.png)

可以简单地认为一台 Kafka 服务器就是一个 Broker，一个 Kafka 集群由多个 Broker 组成。
每个 Kafka 集群内的 Broker 都有一个不重复的编号，Broker-0、Broker-1 等…… 


- Broker 接收来自生产者的消息，为消息设置偏移量，并提交消息到磁盘保存。
<br>
- Broker 为消费者提供服务，对读取分区的请求做出响应，返回已经提交到磁盘的消息。
<br>
- Broker 是集群 (Cluster) 的组成部分。每一个集群都会选举出一个 Broker 作为集群控制器 (Controller)，集群控制器负责管理工作，包括将分区分配给 Broker 和监控 Broker。
<br>
- 在集群中，一个 Partition 可以分配给多个 Brokers，这个时候会发生分区复制。这种复制机制为分区提供了消息冗余，如果有一个 Broker 失效，其他 Broker 可以接管领导权。


![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_8.png)



### Topic 与 Partition

- Topic：消息的主题，可以理解为消息的分类。在每个 Broker 上都可以创建多个 Topic；
为了实现扩展性，一个非常大的 Topic 也可以分布到多个 Broker 上。
<br>

- Partition：Topic 的分区，每个 Topic 可以有多个 Partition，每个 Partition 由一个一个消息组成。
同一个 Topic 在不同的 Partition 的数据是不重复的，Partition 的表现形式就是一个一个的文件夹！

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_4.png)


#### Topic

如下图所示：
创建 Topic1 和 Topic2 两个 Topic，分别有 13 个和 19 个 Partition 分区，则整个集群上会相应会生成共 13+19=32 个文件夹（Topic1 和 Topic2 replication-factor 均为 1），如下图所示。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_topic_1.png)


#### Partition offset
Partition 中的每条消息都被标记了一个 sequential id，也就是 offset，并按顺序存储在 partition 中。

offset 的顺序性不跨 Partition，由于一个 Topic 包含多个 Partition，因此无法在整个 Topic 范围内保证消息的顺序性，但可以保证消息在单个 Partition 内的顺序性。

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

- 每个 Partition 都有一台 server 作为 leader，零台或者多台 server 作为 follwers。
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
<br>
- 因此，在发送消息时，producer可以有多种选择来实现消息发送，例如不等待任何副本的响应便返回成功，或者只是等待leader副本响应写入操作后再返回成功。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_producer_2.png)

代码层面流程（Java）：

1. Producer 首先使用一个线程（用户主线程，也就是用户启动 Producer 的线程）将待发送的消息封装进一个 ProducerRecord 类实例，然后将其序列化之后发送给 Partitioner，再由后者确定了目标分区后一同发送到位于 Producer 程序中的一块内存缓冲池 RecordAccmulator 中。
<br>

2. Producer 的另一个工作线程（I/O 发送线程，也称 Sender 线程）则负责实时地从该缓冲区中提取准备就绪的消息封装进一个批次（batch），统一发送给对应的 Broker。
后台 Sender 线程被触发后从 RecordAccmulator 里面获取消息然后构建成 ClientRequest，将 ClientRequest 封装成 NetWorkClient 准备发送，NetWorkClient 将请求放入 KafkaChannel 准备发送，然后执行网络 IO，最后发送到 Kafka server。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_producer_3.png)


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

consumer group 是 kafka 提供的可扩展且具有容错性的消费者机制。既然是一个组，那么组内必然可以有多个消费者或消费者实例(consumer instance)，它们共享一个公共的 ID，即 group ID。组内的所有消费者协调在一起来消费订阅主题(subscribed topics)的所有分区(partition)，当然，**每个分区只能由同一个消费组内的一个consumer来消费**。

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_7.png)


两个极端情况：
当所有consumer的consumer group相同时，系统变成点对点模式
当每个consumer的consumer group都不相同时，系统变成发布订阅



#### 消费者位置（consumer position)

消费者在消费的过程中需要记录自己消费了多少数据，即消费位置信息。

很多消息引擎都把这部分信息保存在服务器端(Broker端)。这样做的好处当然是实现简单，但会有三个主要的问题：
1. Broker从此变成有状态的，会影响伸缩性；
2. 需要引入应答机制（acknowledgement）来确认消费成功。
3. 由于要保存很多 Consumer 的 offset 信息，必然引入复杂的数据结构，造成资源浪费。

而Kafka选择了不同的方式：每个 consumer group 保存自己的位移信息，那么只需要简单的一个整数表示位置就够了；同时可以引入 checkpoint 机制定期持久化，简化了应答机制的实现。


Kafka 默认是定期帮你自动提交位移的`enable.auto.commit = true`，你当然可以选择手动提交位移实现自己控制。
另外 kafka 会定期把 group 消费情况保存起来，做成一个 offset map，如下图所示：
<br>

![](https://github.com/CZH-HW/CloudImg/raw/master/Comp/kafka_consumer_1.png)

上图中表明了test-group这个组当前的消费情况。









