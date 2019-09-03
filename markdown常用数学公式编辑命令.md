# Markdown-常用数学公式编辑命令

## 1. 基本语法

#### 1.1 行内公式
- 正文(inline)中的LaTeX公式用`$...$`定义
- 语句为`$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$`
- 行内公式显示为：$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$

#### 1.2 行间公式
- 单独显示(display)的LaTeX公式用`$$...$$`定义，此时公式居中并放大显示
- 语句为`$$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$$`
- 行间公式显示为：$$\sum_{i=0}^N\int_{a}^{b}g(t,i)\text{d}t$$


## 2. 希腊字母
|显示|命令|显示|命令|
|:----:|:----:|:----:|:----:|
|$\alpha$|`\alpha`|$\beta$|`\beta`|
|$\gamma$|`\gamma`|$\delta$|`\delta`|
|$\epsilon$|`\epsilon`|$\zeta$|`\zeta`|
|$\eta$|`\eta`|$\theta$|`\theta`|
|$\iota$|`\iota`|$\kappa$|`\kappa`|
|$\lambda$|`\lambda`|$\mu$|`\mu`|
|$\nu$|`\nu`|$\xi$|`\xi`|
|$\pi$|`\pi`|$\rho$|`\rho`|
|$\sigma$|`\sigma`|$\tau$|`\tau`|
|$\upsilon$|`\upsilon`|$\phi$|`\phi`|
|$\chi$|`\chi`|$\psi$|`\psi`|
|$\omega$|`\omega`|

- 若需要大写希腊字母，将命令首字母大写即可
`\Gamma`——>$\Gamma$
- 若需要斜体希腊字母，将命令前加上var前缀即可
`\varGamma`——>$\varGamma$


## 3. 字母修饰

#### 3.1 上下标
- 上标：`^`
- 下标：`_`

举例：`C_n^2`显示为$C_n^2$ 


#### 3.2 矢量
`\vec a`显示为：$\vec a$
`\overrightarrow{xy}`显示为：$\overrightarrow{xy}$

#### 3.3 字体
- Typewriter：`\mathtt{A}`显示为$\mathtt{A}$
- Blackboard Bold：`\mathbb{A}`显示为$\mathbb{A}$
- Sans Serif：`\mathsf{A}`显示为$\mathsf{A}$

#### 3.4 分组
- 使用{}将具有相同等级的内容扩入其中，成组处理
- 举例: `10^{10}`呈现为$10^{10}$，而 `10^10`显示为$10^10$

#### 3.5 括号和分隔符
- `()、[]和|`表示符号本身
- 当要显示大号的括号或分隔符时，要用`\left`和`\right`命令。 使括号大小和邻近的公式相适应，适应于所有括号。
&emsp;&emsp;`(\frac{x}{y})`显示为$(\frac{x}{y})$
&emsp;&emsp;`\left(\frac{x}{y}\right)`显示为$\left(\frac{x}{y}\right)$

- 一些特殊的括号：

|命令|说明|输入|显示|
|:----:|:----:|:----:|:----:|
|`\langle \rangle`|尖括号|`\langle a+b \rangle`|$\langle a+b \rangle$|
|`\lceil \rceil`|上方括号|`\lceil a+b \rceil`|$\lceil a+b \rceil$|
|`\lfloor \rfloor`|下方括号	|`\lfloor a+b \rfloor`|$\lfloor a+b \rfloor$|
|`\lbrace \rbrace`|大括号|`\lbrace a+b \rbrace`|$\lbrace a+b \rbrace$|
|`\overline`|连线符号|`\overline{a+b+c+d}`|$\overline{a+b+c+d}$|
|`\underline`|下划线|`\underline{a+b+c+d}`|$\underline{a+b+c+d}$|
|`\overbrace`|上大括号|`\overbrace{a+\underbrace{b+c}_{1.0}+d}^{2.0}`|$\overbrace{a+\underbrace{b+c}_{1.0}+d}^{2.0}$|
|`\underbrace`|下大括号|`\underbrace{a+d}_3`|$\underbrace{a+d}_3$|


#### 3.6 空格
- Latex语法本身会忽略空格的存在。
- 小空格: `a\ b` 显示为 $a\ b$
- 大空格: `a \quad b` 显示为 $a \quad b$

## 4. 数学公式
####4.1 初等运算
|命令|说明|输入|显示|
|----|----|----|-----|
|`=`|等于|`x=y`|$x=y$|
|`+`|加|`x+y`|$x+y$|
|`-`|减|`x−y`|$x-y$|
|`\ast`|乘|`x \ast y`|$x \ast y$|
|`\div`|除|`x \div y`|$x \div y$|
|`\pm`|加减|`x \pm y`|$x \pm y$|
|`\mp`|减加|`x \mp y`|$x \mp y$|
|`\times`|叉积|`x \times y`|$x \times y$|
|`\cdot`|点积(内积)|`x \cdot y`|$x \cdot y$|
|`^`|幂|`x^a`|$x^a$|
|`^`|指数|`a^x`|$a^x$|
|`\log`|对数|`\log_ax`|$\log_ax$|
|`\ln`|自然对数(e为底)|`\ln x`|$\ln x$|
|`\lg`|10为底对数|`\lg x`|$\lg x$|
|`\frac`|分式|`\frac{x}{y}`|$\frac{x}{y}$|
|`\sqrt`|二次开方根式|`\sqrt{x}`|$\sqrt{x}$|
|`\sqrt`|根式|`\sqrt[n]{x}`|$\sqrt[n]{x}$|
|`\sqrt`|常数二次开方根式|`\sqrt{a}`|$\sqrt{a}$|
|`\sqrt`|常数根式|`\sqrt[n]{a}`|$\sqrt[n]{a}$|
||多项式|`a_nx^n + \cdots + a_1x + a_0 \quad n \geq 0`|$a_nx^n + \cdots + a_1x + a_0 \quad n \geq 0$|

#### 4.2 比较运算
|命令|说明|输入|显示|
|----|----|----|----|
|`\leq`|小于等于|`x \leq y`|$x \leq y$|
|`\geq`|大于等于|`x \geq y`|$x \geq y$|
|`\nleq`|不小于等于|`x \nleq y`|$x \nleq y$|
|`\not \leq`|不小于等于|`x \not \leq y`|$x \not \leq y$|
|`\ngeq`|不大于等于|`x \ngeq y`|$x \ngeq y$|
|`\not \geq`|不大于等于|`x \not \geq y`|$x \not \geq y$|
|`\neq`|不等于|`x \neq y`|$x \neq y$|
|`\approx`|约等于|`x \approx y`|$x \approx y$|
|`\equiv`|恒等于|`x \equiv y`|$x \equiv y$|

#### 4.3 集合运算
|命令|说明|输入|显示|
|----|----|----|----|
|`\in`|属于|`x \in y`|$x \in y$|
|`\notin`|不属于|`x \notin y`|$x \notin y$|
|`\subset`|真子集|`x \subset y`|$x \subset y$|
|`\not \subset`|非子集|`x \not \subset y`|$x \not \subset y$|
|`\subseteq`|子集|`x \subseteq y`|$x \subseteq y$|
|`\supset`|真超集|`x \supset y`|$x \supset y$|
|`\supseteq`|超集|`x \supseteq y`|$x \supseteq y$|
|`\cup`|并集|`x \cup y`|$x \cup y$|
|`\cap`|交集|`x \cap y`|$x \cap y$|
|`\setminus`|差集|`x \setminus y`|$x \setminus y$|
|`\emptyset`|空集合|`\emptyset`|$\emptyset$|

#### 4.4 三角函数
|命令|说明|输入|显示|
|----|----|----|----|
|`\sin`|正弦|`\sin x`|$\sin x$|
|`\cos`|余弦|`\cos x`|$\cos x$|
|`\tan`|正切|`\tan x`|$\tan x$|
|`\cot`|余切|`\cot x`|$\cot x$|
|`\sec`|正割|`\sec x`|$\sec x$|
|`\csc`|余割|`\csc x`|$\csc x$|

#### 4.5 累加与累乘
|说明|输入|显示|
|----|----|----|
|累加求和|`\sum_{i=0}^{n}{a_i}`|$\sum_{i=0}^{n}{a_i}$|
|累乘求积|`\prod_{i=0}^{n}{a_i}`|$\prod_{i=0}^{n}{a_i}$|
|累计并集|`\bigcup_{i=0}^{n}{A_i}`|$\bigcup_{i=0}^{n}{A_i}$|
|累计交集|`\bigcap_{i=0}^{n}{A_i}`|$\bigcap_{i=0}^{n}{A_i}$|

#### 4.6 数学分析
|命令|说明|输入|显示|
|----|----|----|----|
|`\lim`|极限|`\lim_{0 \to \infty}`|$\lim_{0 \to \infty}$|
|`\Delta`|微变量|`\Delta x`|$\Delta x$|
|`\mathrm{d}`|微分算子|`\mathrm{d}{x}`|$\mathrm{d}{x}$|
|`\partial`|偏微分算子|`\partial{x}`|$\partial{x}$|
|`\int`|一重积分|`\int_{a}^{b}{f(x)}\mathrm{d}{x}`|$\int_{a}^{b}{f(x)}\mathrm{d}{x}$|
|`\iint`|二重积分|`\iint_{D}{f(x, y)}\mathrm{d}{\delta}`|$\iint_{D}{f(x, y)}\mathrm{d}{\delta}$|
|`\iiint`|三重积分|`\iiint`|$\iiint$|
|`\oint`|一重曲线积分|`\oint_{L}{P\mathrm{d}x+Q\mathrm{d}y}`|$\oint_{L}{P\mathrm{d}x+Q\mathrm{d}y}$|
|`\ooint`|二重曲线积分|||
|`\oooint`|三重曲线积分|||

## 矩阵
#### 5.1 基本语法
- 起始标记`\begin{matrix}`，结束标记`\end{matrix}`
- 每一行末尾标记`\\`，行间元素之间以`&`分隔

举例:
```markdown
$$
\begin{matrix}
1&0&0\\
0&1&0\\
0&0&1\\
\end{matrix}
$$
```
呈现为：
$$
\begin{matrix}
1&0&0\\
0&1&0\\
0&0&1\\
\end{matrix}
$$

#### 5.2 矩阵边框
- 在起始、结束标记处用下列词替换 matrix
- pmatrix ：小括号边框
- bmatrix ：中括号边框
- Bmatrix ：大括号边框
- vmatrix ：单竖线边框
- Vmatrix ：双竖线边框

举例:
```markdown
\begin{pmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{pmatrix} 
\begin{bmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{bmatrix} 
\begin{Bmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{Bmatrix} 
\begin{vmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{vmatrix} 
\begin{Vmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{Vmatrix}
```

呈现为：
$$
\begin{pmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{pmatrix} 
\begin{bmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{bmatrix} 
\begin{Bmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{Bmatrix} 
\begin{vmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{vmatrix} 
\begin{Vmatrix} 1&0&0 \\ 0&1&0 \\ 0&0&1 \\ \end{Vmatrix}
$$


#### 5.3 省略元素
- 横省略号：`\cdots` 
  输入：`1,2,\cdots,n`, 
  效果显示为：$1,2,\cdots,n$
- 竖省略号：`\vdots`
  输入：`\vdots`
  效果显示为 $\vdots$
- 斜省略号：`\ddots`
  输入：`\ddots`
  效果显示为 $\ddots$
- 底省略号: `\ldots`
  输入：`1,2,\ldots,n`
  效果显示为 $1,2,\ldots,n$

举例：
```markdown
$$
\begin{bmatrix}
{a_{11}}&{a_{12}}&{\cdots}&{a_{1n}}\\
{a_{21}}&{a_{22}}&{\cdots}&{a_{2n}}\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\
{a_{m1}}&{a_{m2}}&{\cdots}&{a_{mn}}\\
\end{bmatrix}
$$
```
呈现为：
$$
\begin{bmatrix}
{a_{11}}&{a_{12}}&{\cdots}&{a_{1n}}\\
{a_{21}}&{a_{22}}&{\cdots}&{a_{2n}}\\
{\vdots}&{\vdots}&{\ddots}&{\vdots}\\
{a_{m1}}&{a_{m2}}&{\cdots}&{a_{mn}}\\
\end{bmatrix}
$$

#### 5.4 阵列
- 需要array环境: 起始、结束处以`{array}`声明
- 对齐方式: 在`{array}`后以`{}`逐行统一声明
  左对齐：`l`  
  居中：`c`  
  右对齐：`r`
竖直线: 在声明对齐方式时，插入`|`建立竖直线
- 插入水平线：`\hline`

举例：
```markdown
$$
\begin{array}{c|lll}
{↓}&{a}&{b}&{c}\\
\hline
{R_1}&{c}&{b}&{a}\\
{R_2}&{b}&{c}&{c}\\
\end{array}
$$
```
呈现为：
$$
\begin{array}{c|lll}
{↓}&{a}&{b}&{c}\\
\hline
{R_1}&{c}&{b}&{a}\\
{R_2}&{b}&{c}&{c}\\
\end{array}
$$

#### 5.5 方程组
- 需要cases环境：起始、结束处以{cases}声明

举例:
```markdown
$$
\begin{cases}
a_1x+b_1y+c_1z=d_1\\
a_2x+b_2y+c_2z=d_2\\
a_3x+b_3y+c_3z=d_3\\
\end{cases}
$$
```
呈现为:
$$
\begin{cases}
a_1x+b_1y+c_1z=d_1\\
a_2x+b_2y+c_2z=d_2\\
a_3x+b_3y+c_3z=d_3\\
\end{cases}
$$



















