# Storage

> - Lecture07
> - Lecture09

存储器由一定数量的单元构成，每个单元可以被唯一标识，每个单元都有存储一个数值的能力。

地址：单元的唯一标识符（采用二进制）

地址空间：可唯一标识的单元总数

寻址能力：存储在每个单元中的信息的位数

大多数存储器是字节寻址的，而执行科学计算的计算机通常是 64 位寻址的。

## 引子

### 存储体系金字塔

![image-20210108205640069](Storage.assets\image-20210108205640069.png)

#### 三要素：容量、存取时间和价格

+ 存取时间越短，平均每位的花费就越大
+ 存储容量越大，平均每位的花费就越小
+ 存储容量越大，存取时间就越长

为了满足容量要求、性能要求，也使每位的价格低，采用了 memory hierarchy 的结构。随着层次的下降：

+ 每位价格下降
+ 容量增大
+ 存取时间变长
+ 处理器访问存储器的频率变少

### buffer

+ 提高效率；
+ 减小丢失和等待；

> 事实上，Cache可以看作Memory的Buffer，不同层级的Cache之间也可以相互看作是Buffer，这个概念可以很广
>
> RAM相关内容放在`Memory&Cache.md`中。



## 硬件

> 下面的“不能随机存取”指的常规情况下。利用特殊手段可以实现逻辑上“随机存取”，但是性能受存储实现原理一般也不怎么样。

### 磁存储技术

> - Lecture09

#### 磁盘

磁盘是一种由非磁性材料制成的称为衬底的圆盘,其上涂有一层磁性材料.

+ 衬底材料: 铝合金,铝,玻璃..
+ 玻璃衬底的优点:
  + 改善磁层表面的均匀性,增加磁盘的可靠性.
  + 显著减少整个表面的缺陷,有助于减少读写错误.
  + 支持更低的飞行高度.
  + 更好的刚度,降低了磁盘的动力需求.
  + 更好的耐冲击和耐磨损能力.

+ 分类:
  + 软盘（他真的是软的，所以很容易坏，rtw上课讲的故事）
  + 硬盘（HDD)



#### 机械硬盘

##### 磁盘物理结构

<img src="https://i0.hdslb.com/bfs/article/3e4cbe481f534268d23e2afca86fc7c67241ab98.png"/>

+ 采用可移动磁头,每面只有一个读-写头.磁头固定在支架上,支架可以伸缩,使磁头可以定位到任意磁道.
+ 任何时候,所有的磁头都定位在磁道上,所有的磁道都是同心圆.
+ 磁盘上相对位置相同的所有磁道的集合称为柱面.

<img src="https://i0.hdslb.com/bfs/article/fca4b3384798d7895a61507b0374a7cd767c1c06.png"/>

+ 盘上的数据组织呈现为一组同心圆环,圆环被称为**磁道**(track).
+ 数据以**扇区**(sectors)为单位传入或者传出磁盘.通常大小为 512 字节
+ 相邻此岛之间有**间隙**(gap),它可以防止或减少由于磁头未对准或磁域干扰引起的错误.为了防止对系统提出不合理的定位要求,相邻的扇区也留有间隙.

###### 恒定角速度(CAV)

+ 盘外沿的传输要比靠近盘中心快, 通过增加位之间的间隔,使磁盘以固定的速度扫描信息.

**优点**

  + 可以用磁道号和扇区号直接寻址.

**缺点**

+ 外围长磁道与内圈短磁道存储的数据一样多.
  <img src="https://i0.hdslb.com/bfs/article/4adceca11695a8e1f1a09320d9146fed93c63b56.png"/>

###### 多重区域记录

+ 盘面被分为多个区域,每个区域中各磁道的扇区数是一定的,远离中心的磁道能够容纳更多的扇区.
+ 优势: 提升了存储容量.
+ 劣势: 实现的电路更加复杂.

##### 读写机制

<img src="https://i0.hdslb.com/bfs/article/e05df25e7ef8c81a42d80706580c0351ca98ce24.png"/>

+ 读-写是通过一个叫做**磁头**的导电线圈进行的.
+ 读写期间,磁头静止不动,盘片高速旋转
+ 头的数量:
  + 单: 读写共用一个头.用于软盘, 老式硬盘.
  + 双: 单独的读,写头.用于现代硬盘.

###### 写机制

+ 脉冲电流送入写磁头.
+ 利用通电线圈周围产生磁场的性质,线圈中的电流在缝隙中感应出一个磁场.
+ 产生的磁化模式被记录在其下的磁盘表面
+ 逆转电流的方向将逆转记录介质上的磁化方向.

###### 读机制

+ 传统的读机制采用磁盘相对线圈运动产生磁场的效应.
+ 当代读机制采用一个单独的读磁头,由一个部分被屏蔽的磁阻(MR)传感器组成.当下方介质的磁化方向改变的时候,电阻会发生变化.
+ 让电流通过 MR 传感器,电阻的变化作为电压变化被检测出来.
+ MR 设计允许高频操作, 这等同于更高的存储密度和更快的操作速度.

###### 磁头机制

磁头必须产生或者感应到足够大小的电磁场,以便正确读写.

+ 较窄的磁头要求与盘面距离更近.
+ 更大的数据密度需要更窄的磁头和更窄的磁道.这将导致更大的出错率.
+ 温切斯特磁盘: (现在几乎所有盘都是这种了，是硬盘发展史上的革命)
  + 磁头是一个空气动力箔片,盘片静止时,他停靠在盘面上.
  + 旋转盘片产生的气压足以使滤波从表面上升起.

##### 格式化

<img src="https://i0.hdslb.com/bfs/article/8f96c47ff22976a116674aba9e61d84bd1fdb87a.png"/>

+ 磁道必须有一些起始点和辨识每个扇区的起点及终点的方法.
+ 格式: 附加一些仅被磁盘驱动器使用而不被用户存取的额外数据.
  + ID 域是唯一的一个标识或地址，用于定位具体的扇区.
  + 同步字节是一个特殊的位模式，用来定义区域的起始点.
  + 磁道号标识磁盘表面上的一个磁道.
  + 磁头号标识一个磁头，因为磁盘有多个面.
  + ID 和数据域各包含一个检错码（CRC).



#### 磁盘性能参数

<img src="https://i0.hdslb.com/bfs/article/dffc832d9baadeeeae29a68f40aed1b8868f484e.jpg">

##### 寻道时间

+ 磁头定位到该磁道所花的时间.
  + 初始启动的时间
  + 穿越磁道的时间

##### 旋转延迟

+ 等待相应扇区的起始处到达磁头的时间.
  + 平均为磁盘周期的一半.

##### 存取时间

+ **寻道时间**和**旋转延迟**总和

##### 传送时间

+ 数据传送到磁盘所需时间
  $$
  T=\frac {b}{rN}
  $$
  + $b$ 待传送字节数
  + $N$ 各磁道的字节数
  + $r$ 转速

##### 总平均存取时间

$$
T = T_S + \frac{1}{2r} + \frac{b}{rN}
$$

> 可以证明磁盘整理加快存取速度.

**性能实测（YDJSIR的4T 希捷酷狼，50MB的读写大小）**

![image-20210108221105071](Storage.assets\image-20210108221105071.png)

**随机存取效率低**

#### 磁盘调度算法

##### 先来先服务(FCFS)

+ 按 I/O 请求的顺序处理.

> 可能会出现部分请求长期未响应的情况

##### 最短寻道时间(SSTF)

+ 优先处理距离当前磁头位置的最短寻道时间的请求.

> 可能出现部分请求长期未响应的情况

##### SCAN

+ 在 0 和 N 之间移动磁头,优先处理当前移动方向上的请求,到达一端后反向.

##### C-SCAN

+ 将磁头从 N 移动到 0, 到达 0 后立即返回 N

##### LOOK

+ 建立在 SCAN 和 C-SCAN 的基础上, 磁头只移动到一个方向最远的请求.

#### 磁带

+ 顺序存储

+ 廉价

  ![image-20210113233640341](Storage.assets\image-20210113233640341.png)

+ 写方式

  + 并行写:多个磁道同时写 
  + 串行写:一个磁道返回来写另一个



**不能随机存取**

### 光存储技术-光盘

> - Lecture09

#### CD 和 CD-ROM

<img src="https://i0.hdslb.com/bfs/article/7fb8c174c901eee86e22e7ba055ac09d454ae5c6.jpg">

##### 生产

+ 用精密聚焦的高强度激光制造母盘.
+ 用母盘在聚碳酸酯上压印出复制品.
+ 在凹坑表面镀上高反射的材料.
+ 表面涂一层丙烯酸树脂防止灰尘或划伤.
+ 用丝网印刷术把标签印刷在丙烯酸上.

##### 读

+ 通过安装在光盘播放机或驱动器中的低功率激光从CD或CD-ROM中检索
+ 如果激光束落在表面有点粗糙的凹坑上，光就会散射，低强度的光就会反射回来
+ 如果激光束落在一个光滑的表面上，更高的强度会被反射回来
+ 磁盘包含一个单一的螺旋轨道，和所有扇区是相同的长度
+ 以变速旋转磁盘
+ 然后激光以恒定的线速度读取凹坑

##### CD和CD - ROM的区别

+ CD-ROM player更坚固，并有错误纠正设备，以确保数据正确传输

###### 优势

+ 光盘和存储在上面的信息可以廉价地大量复制
+ 光盘是可移动的

##### 缺点

+ 它是只读的，不能更新
+ 它的存取时间比磁盘长得多

#### CD-R

+ 包括染色层，用于改变反射率，并被高强度激光激活
+ 可以用适当强度激光写入一次
+ 生成的磁盘可以在CD-R或CD-ROM驱动器上读取

#### CD-RW

+ 使用一种在不同相位下反射率显著不同的材料, 可以被激光改变.
+ 会随着使用老化最终完全失去性能.

#### DVD

<img src = "https://i0.hdslb.com/bfs/article/d44806d7159cd21cf86706a28d258d64997d5484.png">

##### 与CD相比 

+ 位组装更加紧密.
+ DVD 采用双层结构
+ DVD-ROM可以是双面的, CD只能是单面的.

**以上均不能随机存取**

#### 蓝光

更短的波长，更大的数据容量，更好的耐久性（25G/50G/100G)

> 

### 半导体存储技术

> - Lecture07
>
> 注意到这章节内讲到的内容不止包含冯诺依曼架构包含的"内存"！本章涉及到的存储技术同样可能用作外部存储，如Flash芯片也用在我们的U盘和固态硬盘中。这里总结的内容对Cache和内存只介绍其物理原理！

**这里涉及的存储器类型都可以随机存取。**

#### Memory Cell - 位元 - 基本单元

<img src="Storage.assets\image-20210108210011903.png" alt="image-20210108210011903" style="zoom: 50%;" />

- 两个稳定/半稳定状态以代表0和1；
- 至少能写入一次去设置状态；
- 能够读取其状态

#### 分类

![image-20210108210450495](Storage.assets\image-20210108210450495.png)

#### ROM

非容失性

###### ROM

只能写一次不能改（既是优点也是缺点）

写入需要特殊设备甚至通常只能在工厂进行，大批量生产成本低

适合于只读场景（系统程序、子例程库等）

###### PROM

只能写一次不能改（既是优点也是缺点）

能用电子方式去烧了，但需要特殊设备

成本仍然较低，可以大批量生产

###### EPROM

###### EEPROM

> 如上表所示，价格越来越贵，写入越发容易。

#### Flash

> 是现在常用的USB和固态硬盘采用的技术

是EPROM和EEPROM在开销和性能上的平衡

- 擦除比EPROM快
- 密度能达到EPROM水平，比EEPROM高

Flash的速度可以很快很快，当然也可以比较慢。下面是YDJSIR的固态。

![image-20210108221409001](Storage.assets\image-20210108221409001.png)

## 软件

此处主要指的是磁盘（可以是机械，可以是固态）的RAID。Cache和RAM的高级组织不在此处展开。

### RAID

> - Lecture11

#### 基本思想

使用多个磁盘, 分散的 I/O 请求, 以至于单一的 I/O 请求可以被并行处理, 只要请求的数据分散在不同的磁盘上.

##### Redundant Array of Independent Disks

![image-20210113234127736](Storage.assets\image-20210113234127736.png)

#### 特点

1. RAID 是被视为**一块逻辑磁盘**的一组物理磁盘.
2. 数据交叉分布在物理磁盘上.
3. 冗余的磁盘可用于存储奇偶校验信息, 以保证再磁盘故障的情况下的数据可恢复性.

<img src="https://i0.hdslb.com/bfs/article/b1cdc3c889a42b58f7db4790cdaadd03a1480418.jpg" style="zoom: 80%;" />

> 图中所示为RAID 1

#### 商业化使用的RAID

| RAID 0                                                       | RAID 1                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 数据在可用的磁盘上**条带** (strip) 状排列, 如果数据跨物理磁盘, 则同时读写.<br/>不含冗余盘,因而并不是真正意义上的RAID方式。<br/>**用途**:+ 高速率数据传输+ 高速 I/O 请求<br/>![](https://i0.hdslb.com/bfs/article/5c640fb591dd2ac5f4fb02edd077ebdc49f6e700.jpg)<br/>与单个的大容量磁盘相比: ​	<br/>**优点**: 若两个不同的 I/O 请求等待不同的两块数据, <br/>如果这两块数据位于不同的物理磁盘, 就能加速.​	<br/>**缺点**: 若数据跨盘, 只要其中的一块硬盘坏了, 所有的都不能读取.<br/>RAID 0 一时爽，数据火葬场<br/> | 所有的数据都按 RAID 0 的方式存取, 只是每个数据都存两份 ( 镜像 ).<br/>**优点**: 恢复很简单, 可以加速读取 ( 若两块硬盘中同时有这个数据 ).<br/>**缺点**: 浪费, 性能会降低为较慢的盘.<br/>**用途**: 仅用于存储系统文件, 数据和其他高度关键文件的驱动器.<br/>与 RAID 0 相比:<br/>+ 若请求的大部分是读取, RAID 1 可以实现较高的 I/O 请求率, 性能几乎是 RAID 0 的两倍.<br/>+ 若大部分是写请求, 性能没有显著的优势.<br/>![](https://i0.hdslb.com/bfs/article/cebac2dff8b13ac60b2d07856d55b1d8daee2029.jpg) |

| RAID 3                                                       | RAID 5                                                       | RAID 6                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 使用**并行存取技术**.<br/>使用很小的条带<br/>**纠错机制**:<br/>+ 一个奇偶校验盘, 存有数据盘相同位置数据的奇偶校验码<br/>+ 若有一个磁盘失效, 可以由其他磁盘和校验盘恢复出这个数据, 如 $b_0$ 失效:<br/>$$b_0 = P(b)\ \oplus b_1 \oplus b_2 \oplus b_3$$<br/>![](https://i0.hdslb.com/bfs/article/c14185b2896bf704dbdffd8eb15818f8a4ab3f5d.jpg)<br/>性能:<br/>+ 高速率传输数据, 对大型传输改善尤为明显.<br/>+ 一次只能执行一个 I/O 请求 | 与 RAID 4 相类似, 但是没有专门的冗余盘, 而是分散到各个盘, 减少了 I/O 瓶颈.是最常用的RAID模式。![](https://i0.hdslb.com/bfs/article/baf3a2e585eaa2a9c04472f81049dc165ad591fc.jpg) | 两种纠错码相互印证<br/>![](https://i0.hdslb.com/bfs/article/329222880f592d912c0168da0ee8dd5d85a0c895.jpg) |

#### 未商业化的RAID模式

##### RAID 2 (未商业化)

使用**并行存取技术**(为一个客户服务):

+ 所有的成员磁盘参与 I/O 的运算,
+ 各自驱动器的轴是同步旋转的, 以便每个磁盘磁头的位置在同一时刻是相同的.

使用数据条带: 条带很小, 一个字节或者一个字.

**纠错机制**:

+ 每个磁盘相应位置计算校验码, 并存在多个校验盘的相应位置上.通常使用**汉明码**.
+ 读: 获取所请求的数据以及校验码.
+ 写: 必须访问所有的数据盘和校验盘.

**优点**: 

+ 高速率数据传输
+ 数据丢失可以恢复

缺点: 

+ I/O 响应慢,  同时只能处理一个 I/O
+ 成本过高, 因为磁盘可靠性已经得到了极大提高, 不容易出现大量错误

![](https://i0.hdslb.com/bfs/article/25f37c6b45723246e13639184a33eda269778c1a.jpg)



##### RAID 4(未商业化)

使用**独立访问技术**: 期望每个成员磁盘独立操作, 可以独立地并行完成 I/O 请求.

使用较大的数据条带

纠错机制: 每个数据盘上相应的条带上逐位计算奇偶校验带, 存储在奇偶校验盘的相应位置.

性能: 

+ 执行小规模 I/O 写请求时, RAID 4 遭受写损失

+ 每次写操作, 阵列管理软件不仅需要更新数据 , 还需要更新对应的奇偶校验码. 两读两写.
  $$
  P' = P \oplus b_0\oplus b_0'
  $$

+ 涉及大规模 I/O 写入, 仅根据新数据原数据和校验码就能得到新的校验码.

+ 冗余盘不能同时进行多个 I/O 操作, 成为写的瓶颈.

![](https://i0.hdslb.com/bfs/article/82cb8a6c55db2048f293916b87988b3e9e419f7e.jpg)

## 纠错

> - Lecture10

#### 基本思想

##### 方法

 添加一些位来存储附加信息以便校正或者发现错误

纠错不是万能的，只能在一定程度上保障数据的可靠性，如果真的有保证不出错的方式，我们直接就那样做一个存储器就好了。

##### 过程 

+ 读入:$M$ 位的数据 $D$ 通过函数 $f$ 产生 $K$ 位的校验码 $C$
+ 被读出:通过 $f$ 由$D'$ 生成 $C''$与 $C'$ 相比较
  + 无错误: 发送 $D'$
  + 有错误并可以纠正,发送 $D''$
  + 有错误且不能纠正, 报告

#### 奇偶校验法

##### 过程
$D=D_M...D_2D_1$

+ 奇校验: $D_M \oplus ...D_2 \oplus D_1 \oplus 1$
+ 偶校验: $$D_M \oplus ...D_2 \oplus D_1$$
+ 检查 $S=C' \oplus C''$
  + $S=1$ 错误的位数为奇数
  
  + $S=0$ 错误的位数为偶数或没有错误
  
    注意: 此处是指$C$与$D$合在一起是否出错

##### 优势

+ 廉价

##### 劣势

+ 无法确定出错的位置
+ 无法纠正错误

+ 适用于较短的 $D$，是汉明码的基础

### 汉明码

#### 基本思想

将数据的位分组, 每位都分到多个组且分到的组的情况不同, 每个组中奇偶校验产生一位校验码, 最后根据所有组的校验码可以定位到这位数据

#### **前提**

假设最多仅有一位出错

#### 具体过程:

+ 将 $M$ 位数据 $D$ 分成 $K$ 组
+ 每个组中产生一位~~奇~~偶校验码, 最终产生一位$K$ 位的校验码 $C'$
+ 由 $D'$ 产生 $C''$
+ 检查: 故障字 $SW=C'\oplus C''$, 长度为 $K$ 位

#### 校验码的长度

+ 要确保故障字的情况能够包含所有的情况在内.即$$2^K\geq 1+ M+K$$
  其中 $K$ 为校验码出错情况,$M$ 为数据出错情况, 1 未出错情况

#### 故障字分析:

如果是数据位出错,那么至少有两位校验码会出错, 即故障字至少有两位为1,可得下面规则
+ 全为0 : 没有错误
+ 只有一位为 1: 校验码 $C'$ 出错,无需纠正
+ 多位为 1: 数据位出错,需要纠正(对相应位置的数据取反)

#### 数据位划分

+ 以 8 位的数据 $D = D_8...D_2D_1$为例子，校验码$C_4C_3C_2C_1$

+ 关系如下
  
  |  12  |  11  |  10  |  9   |  8   |  7   |  6   |  5   |  4   |  3   |  2   |  1   |
  | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
  | 1100 | 1011 | 1010 | 1001 | 1000 | 0111 | 0110 | 0101 | 0100 | 0011 | 0010 | 0001 |
  |  D8  |  D7  |  D6  |  D5  | `C4` |  D4  |  D3  |  D2  | `C3` |  D1  | `C2` | `C1` |
  |      |      |      |      |      |      |      |      |      |      |      |      |
  
  数据在传输的时候也正是这样穿插进行的
  
+ 数据位划分
  $$
  \begin{aligned}
  C_{1}=D_{1} \oplus D_{2} \oplus D_{4} \oplus D_{5} \oplus D_{7} \\
  C_{2}=D_{1} \oplus D_{3} \oplus D_{4} \oplus D_{6} \oplus D_{7} \\
  C_{3}=D_{2} \oplus D_{3} \oplus D_{4} \oplus D_{8} \\
  C_{4}=D_{5} \oplus D_{6} \oplus D_{7} \oplus D_{8}
  \end{aligned}
  $$
  **快速记忆法**：对应故障字中对应位为1的数据位都要参与到该校验位的计算中
  
  
  

上面的公式

### 循环冗余校验 (CRC)

#### 奇偶校验问题

+ 需要将数据 划分为字节级
+ 额外开销很大

#### CRC 适用于以流形式存储和传输大型数据

用数学函数产生数据和校验码的关系

#### 过程

+ 对于 $n$ 位的数据 $D$, 次数为 $K$ 的生成多项式(用二进制表示的话有 $K+1$ 位),需要将 $D$ 左移 $K$ 位.
+ 进行模二取余的除法运算, 具体过程如下图:
  <img src="http://i0.hdslb.com/bfs/album/42ae5b4ac3450a3bf179f67b6827527ba37e9b7c.gif@518w.gif"/>

#### 检查

+ 将 $n+k$ 位的内容对生成多项式做上述操作,如果无误,所得结果为0