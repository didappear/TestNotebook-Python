Chapter11-操作系统



# 一、为什么要有操作系统

现代的计算机系统主要是由一个或者多个处理器，主存，硬盘，键盘，鼠标，显示器，打印机，网络接口及其他输入输出设备组成。

一般而言，现代计算机系统是一个复杂的系统。

其一：如果每位应用程序员都必须掌握该系统所有的细节，那就不可能再编写代码了（严重影响了程序员的开发效率：全部掌握这些细节可能需要一万年....）

其二：并且管理这些部件并加以优化使用，是一件极富挑战性的工作，于是，计算安装了一层软件（系统软件），称为操作系统。它的任务就是为用户程序提供一个更好、更简单、更清晰的计算机模型，并管理刚才提到的所有设备。

**总结：**

**程序员无法把所有的硬件操作细节都了解到，管理这些硬件并且加以优化使用是非常繁琐的工作，这个繁琐的工作就是操作系统来干的，有了他，程序员就从这些繁琐的工作中解脱了出来，只需要考虑自己的应用软件的编写就可以了，应用软件直接使用操作系统提供的功能来间接使用硬件。**



# 二、什么是操作系统

**精简的说的话，操作系统就是一个协调、管理和控制计算机硬件资源和软件资源的控制程序。操作系统所处的位置如图1**

```python
#操作系统位于计算机硬件与应用软件之间，本质也是一个软件。操作系统由操作系统的内核（运行于内核态，管理硬件资源）以及系统调用（运行于用户态，为应用程序员写的应用程序提供系统调用接口）两部分组成，所以，单纯的说操作系统是运行于内核态的，是不准确的。
```

![img](chapter11-OperatingSystem.assets/1036857-20170118112402265-1697763795.png)

​                                                                      图1

**细说的话，操作系统应该分成两部分功能：**

1.管理硬件，提供更好用的系统调用接口

2.把应用程序对硬件资源的无序竞争变得有序

```python
#一：隐藏了丑陋的硬件调用接口，为应用程序员提供调用硬件资源的更好，更简单，更清晰的模型（系统调用接口）。应用程序员有了这些接口后，就不用再考虑操作硬件的细节，专心开发自己的应用程序即可。
例如：操作系统提供了文件这个抽象概念，对文件的操作就是对磁盘的操作，有了文件我们无需再去考虑关于磁盘的读写控制（比如控制磁盘转动，移动磁头读写数据等细节），

#二：将应用程序对硬件资源的竞态请求变得有序化
例如：很多应用软件其实是共享一套计算机硬件，比方说有可能有三个应用程序同时需要申请打印机来输出内容，那么a程序竞争到了打印机资源就打印，然后可能是b竞争到打印机资源，也可能是c，这就导致了无序，打印机可能打印一段a的内容然后又去打印c...,操作系统的一个功能就是将这种无序变得有序。
```

**详解：**

```python
#作用一：为应用程序提供如何使用硬件资源的抽象
例如：操作系统提供了文件这个抽象概念，对文件的操作就是对磁盘的操作，有了文件我们无需再去考虑关于磁盘的读写控制

注意：
操作系统提供给应用程序的该抽象是简单，清晰，优雅的。为何要提供该抽象呢？
硬件厂商需要为操作系统提供自己硬件的驱动程序（设备驱动，这也是为何我们要使用声卡，就必须安装声卡驱动。。。），厂商为了节省成本或者兼容旧的硬件，它们的驱动程序是复杂且丑陋的
操作系统就是为了隐藏这些丑陋的信息，从而为用户提供更好的接口
这样用户使用的shell，Gnome，KDE看到的是不同的界面，但其实都使用了同一套由linux系统提供的抽象接口


#作用二：管理硬件资源
现代的操作系统运行同时运行多道程序，操作系统的任务是在相互竞争的程序之间有序地控制对处理器、存储器以及其他I/O接口设备的分配。
例如：
同一台计算机上同时运行三个程序，它们三个想在同一时刻在同一台计算机上输出结果，那么开始的几行可能是程序1的输出，接着几行是程序2的输出，然后又是程序3的输出，最终将是一团糟（程序之间是一种互相竞争资源的过程）
操作系统将打印机的结果送到磁盘的缓冲区，在一个程序完全结束后，才将暂存在磁盘上的文件送到打印机输出，同时其他的程序可以继续产生更多的输出结果（这些程序的输出没有真正的送到打印机），这样，操作系统就将由竞争产生的无序变得有序化。
```



![img](chapter11-OperatingSystem.assets/1036857-20170825154958011-1047256710.png)

​                                                     图 2



# 三、操作系统与普通软件的区别

1.主要区别是：你不想用暴风影音了你可以选择用迅雷播放器或者干脆自己写一个，但是你无法写一个属于操作系统一部分的程序（时钟中断处理程序），操作系统由硬件保护，不能被用户修改。

2.操作系统与用户程序的差异并不在于二者所处的地位。特别地，操作系统是一个大型、复杂、长寿的软件，

- 大型：linux或windows的源代码有五百万行数量级。按照每页50行共1000行的书来算，五百万行要有100卷，要用一整个书架子来摆置，这还仅仅是内核部分。用户程序，如GUI，库以及基本应用软件（如windows Explorer等），很容易就能达到这个数量的10倍或者20倍之多。
- 长寿：操作系统很难编写，如此大的代码量，一旦完成，操作系统所有者便不会轻易扔掉，再写一个。而是在原有的基础上进行改进。（基本上可以把windows95/98/Me看出一个操作系统，而windows NT/2000/XP/Vista则是两位一个操作系统，对于用户来说它们十分相似。还有UNIX以及它的变体和克隆版本也演化了多年，如System V版，Solaris以及FreeBSD等都是Unix的原始版，不过尽管linux非常依照UNIX模式而仿制，并且与UNIX高度兼容，但是linux具有全新的代码基础）



# 四、操作系统发展史

## **第一代计算机（1940~1955）：真空管和穿孔卡片**

### 第一代计算机的产生背景：

第一代之前人类是想用机械取代人力，第一代计算机的产生是计算机**由机械时代进入电子时代**的标志，从Babbage失败之后一直到第二次世界大战，数字计算机的建造几乎没有什么进展，第二次世界大战刺激了有关计算机研究的爆炸性进展。

lowa州立大学的john Atanasoff教授和他的学生Clifford Berry建造了据认为是第一台可工作的数字计算机。该机器使用300个真空管。大约在同时，Konrad Zuse在柏林用继电器构建了Z3计算机，英格兰布莱切利园的一个小组在1944年构建了Colossus，Howard Aiken在哈佛大学建造了Mark 1，宾夕法尼亚大学的William Mauchley和他的学生J.Presper Eckert建造了ENIAC。这些机器有的是二进制的，有的使用真空管，有的是可编程的，但都非常原始，设置需要花费数秒钟时间才能完成最简单的运算。

在这个时期，同一个小组里的工程师们，设计、建造、编程、操作及维护同一台机器，所有的程序设计是用纯粹的机器语言编写的，甚至更糟糕，需要通过成千上万根电缆接到插件板上连成电路来控制机器的基本功能。没有程序设计语言（汇编也没有），操作系统则是从来都没听说过。使用机器的过程更加原始，详见下‘工作过程’

### 特点：

没有操作系统的概念
所有的程序设计都是直接操控硬件

### 工作过程：

程序员在墙上的机时表预约一段时间，然后程序员拿着他的插件版到机房里，将自己的插件板接到计算机里，这几个小时内他独享整个计算机资源，后面的一批人都得等着(两万多个真空管经常会有被烧坏的情况出现)。

后来出现了穿孔卡片，可以将程序写在卡片上，然后读入机而不用插件板

### 优点：

程序员在申请的时间段内独享整个资源，可以即时地调试自己的程序（有bug可以立刻处理）

### 缺点：

浪费计算机资源，一个时间段内只有一个人用。

### 注意：

同一时刻只有一个程序在内存中，被cpu调用执行，比方说10个程序的执行，是串行的

## **第二代计算机（1955~1965）：晶体管和批处理系统**

### 第二代计算机的产生背景：

由于当时的计算机非常昂贵，很自然的想办法减少机时的浪费。通常采用的方法就是批处理系统。

### 特点：

设计人员、生产人员、操作人员、程序人员和维护人员直接有了明确的分工，计算机被锁在专用空调房间中，由专业操作人员运行，这便是‘大型机’。

有了操作系统的概念

有了程序设计语言：FORTRAN语言或汇编语言，写到纸上，然后穿孔打成卡片，再将卡片盒带到输入室，交给操作员，然后喝着咖啡等待输出接口

### 工作过程：

插图

![img](chapter11-OperatingSystem.assets/1036857-20170118153002781-1203239513.png)

![img](chapter11-OperatingSystem.assets/1036857-20170118153032921-860551349.png)

```python
d)7094机先读入一段磁带，这段磁带上是一段程序，这个程序负责把程序员的程序一个一个往内存里读。事先装入一段程序在机器上运行，再按顺序读入作业。
```



### 第二代如何解决第一代的问题/缺点：

1.把一堆人的输入攒成一大波输入，
2.然后顺序计算（这是有问题的，但是第二代计算也没有解决）
3.把一堆人的输出攒成一大波输出

### 现代操作系统的前身:(见图）

**优点：**批处理，节省了机时

**缺点：**

*1.整个流程需要人参与控制，将磁带搬来搬去（中间俩小人）*

2.计算的过程仍然是顺序计算-->串行

3.程序员原来独享一段时间的计算机，现在必须被统一规划到一批作业中，等待结果和重新调试的过程都需要等同批次的其他程序都运作完才可以（这极大的影响了程序的开发效率，无法及时调试程序）

## **第三代计算机（1965~1980）：集成电路芯片和多道程序设计**

### 第三代计算机的产生背景：

20世纪60年代初期，大多数计算机厂商都有两条完全不兼容的生产线。

一条是面向字的：大型的科学计算机，如IBM 7094，见上图，主要用于科学计算和工程计算

另外一条是面向字符的：商用计算机，如IBM 1401，见上图，主要用于银行和保险公司从事磁带归档和打印服务

开发和维护完全不同的产品是昂贵的，同时不同的用户对计算机的用途不同。

IBM公司试图通过引入system/360系列来同时满足科学计算和商业计算，360系列低档机与1401相当，高档机比7094功能强很多，不同的性能卖不同的价格

360是第一个采用了（小规模）芯片（集成电路）的主流机型，与采用晶体管的第二代计算机相比，性价比有了很大的提高。这些计算机的后代仍在大型的计算机中心里使用，**此乃现在服务器的前身**，这些服务器每秒处理不小于千次的请求。

### 如何解决第二代计算机的问题1：
（1.整个流程需要人参与控制，将磁带搬来搬去）

卡片被拿到机房后能够很快的将作业从卡片读入磁盘，于是任何时刻当一个作业结束时，操作系统就能将一个作业从磁带读出，装进空出来的内存区域运行，这种技术叫做同时的外部设备联机操作：SPOOLING，该技术同时用于输出。当采用了这种技术后，就不在需要IBM1401机了，也不必将磁带搬来搬去了（中间俩小人不再需要）

### 如何解决第二代计算机的问题2：

（2.计算的过程仍然是顺序计算-->串行）

第三代计算机的操作系统广泛应用了第二代计算机的操作系统没有的关键技术：多道技术

**cpu在执行一个任务的过程中，若需要操作硬盘，则发送操作硬盘的指令，指令一旦发出，硬盘上的机械手臂滑动读取数据到内存中，这一段时间，cpu需要等待，时间可能很短，但对于cpu来说已经很长很长，长到可以让cpu做很多其他的任务，如果我们让cpu在这段时间内切换到去做其他的任务，这样cpu不就充分利用了吗。这正是多道技术产生的技术背景**

**多道技术：**

产生背景：为了实现单cpu下的并发效果。

多道技术中的多道指的是多个程序，多道技术的实现是为了解决多个程序竞争或者说共享同一个资源（比如cpu）的有序调度问题，解决方式即多路复用，多路复用分为时间上的复用和空间上的复用。

**1）空间上的复用**：将内存分为几部分，每个部分放入一个程序，这样，同一时间内存中就有了多道程序。

![img](chapter11-OperatingSystem.assets/1036857-20170313010137401-1096621807.png)

 

**2）时间上的复用**：当一个程序在等待I/O时，另一个程序可以使用cpu，如果内存中可以同时存放足够多的作业，则cpu的利用率可以接近100%，类似于我们小学数学所学的**统筹方法**。（操作系统采用了多道技术后，可以控制进程的切换，或者说进程之间去争抢cpu的执行权限。这种切换不仅会在一个进程遇到io时进行，一个进程占用cpu时间过长也会切换，或者说被操作系统夺走cpu的执行权限）

详解

```python

现代计算机或者网络都是多用户的，多个用户不仅共享硬件，而且共享文件，数据库等信息，共享意味着冲突和无序。

# 操作系统主要是用来：

1.记录哪个程序使用什么资源

2.对资源请求进行分配

3.为不同的程序和用户调解互相冲突的资源请求。

我们可将上述操作系统的功能总结为：处理来自多个程序发起的多个（多个即多路）共享（共享即复用）资源的请求，简称多路复用

# 多路复用有两种实现方式

1.时间上的复用

当一个资源在时间上复用时，不同的程序或用户轮流使用它，第一个程序获取该资源使用结束后，在轮到第二个。。。第三个。。。

例如：只有一个cpu，多个程序需要在该cpu上运行，操作系统先把cpu分给第一个程序，在这个程序运行的足够长的时间（时间长短由操作系统的算法说了算）或者遇到了I/O阻塞，操作系统则把cpu分配给下一个程序，以此类推，直到第一个程序重新被分配到了cpu然后再次运行，由于cpu的切换速度很快，给用户的感觉就是这些程序是同时运行的，或者说是并发的，或者说是伪并行的。至于资源如何实现时间复用，或者说谁应该是下一个要运行的程序，以及一个任务需要运行多长时间，这些都是操作系统的工作。

2.空间上的复用

每个客户都获取了一个大的资源中的一小部分资源，从而减少了排队等待资源的时间。

例如：多个运行的程序同时进入内存，硬件层面提供保护机制来确保各自的内存是分割开的，且由操作系统控制，这比一个程序独占内存一个一个排队进入内存效率要高的多。

有关空间复用的其他资源还有磁盘，在许多系统中，一个磁盘同时为许多用户保存文件。分配磁盘空间并且记录谁正在使用哪个磁盘块是操作系统资源管理的典型任务。

这两种方式合起来便是多道技术
# 总结
多道技术包括空间上的复用和时间上的复用。
内存中运行多道程序，如果一个程序运行足够长的时间或者遇到I/O阻塞，CPU在多道程序间快速切换，最终实现并发的效果。

```



**空间上的复用最大的问题是**：

程序之间的内存必须分割，这种分割需要在硬件层面/物理级别实现，由操作系统控制。如果内存彼此不分割，则一个程序可以访问另外一个程序的内存，

- 首先丧失的是安全性，比如你的qq程序可以访问操作系统的内存，这意味着你的qq可以拿到操作系统的所有权限。

- 其次丧失的是稳定性，某个程序崩溃时有可能把别的程序的内存也给回收了，比方说把操作系统的内存给回收了，则操作系统崩溃。

**第三代计算机的操作系统仍然是批处理**

许多程序员怀念第一代独享的计算机，可以即时调试自己的程序。为了满足程序员们很快可以得到响应，出现了分时操作系统

### 如何解决第二代计算机的问题3：

（3.程序员原来独享一段时间的计算机，现在必须被统一规划到一批作业中，等待结果和重新调试的过程都需要等同批次的其他程序都运作完才可以（这极大的影响了程序的开发效率，无法及时调试程序））

分时操作系统：
多个联机终端+多道技术

20个客户端同时加载到内存，有17在思考，3个在运行，cpu就采用多道的方式处理内存中的这3个程序，由于客户提交的一般都是简短的指令而且很少有耗时长的，所以计算机能够为许多用户提供快速的交互式服务，所有的用户都以为自己独享了计算机资源

CTTS：麻省理工（MIT）在一台改装过的7094机上开发成功的，CTSS兼容分时系统，**第三代计算机广泛采用了必须的保护硬件（程序之间的内存彼此隔离）之后，分时系统才开始流行**

MIT，贝尔实验室和通用电气在CTTS成功研制后决定开发能够同时支持上百终端的MULTICS（其设计者着眼于建造满足波士顿地区所有用户计算需求的一台机器），很明显真是要上天啊，最后摔死了。

后来一位参加过MULTICS研制的贝尔实验室计算机科学家Ken Thompson开发了一个简易的，单用户版本的MULTICS，**这就是后来的UNIX系统**。基于它衍生了很多其他的Unix版本，为了使程序能在任何版本的unix上运行，IEEE提出了一个unix标准，即**posix（可移植的操作系统接口Portable Operating System Interface）**

后来，在1987年，出现了一个UNIX的小型克隆，即minix，用于教学使用。芬兰学生Linus Torvalds基于它编写了Linux

## **第四代计算机（1980~至今）：个人计算机**

略 




