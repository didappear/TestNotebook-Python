#一、字符编码
##0.原理
###1.文本编辑器读取文件的原理
- 打开编辑器就打开了启动了一个进程，是在内存中的，所以在编辑器编写的内容也都是存放与内存中的，断电后数据丢失
- 因而需要保存到硬盘上，点击保存按钮，就从内存中把数据刷到了硬盘上。
- 在这一点上，我们编写一个py文件（没有执行），跟编写其他文件没有任何区别，都只是在编写一堆字符而已。
###2.pycharm读取文件的原理
- python解释器启动，此时就相当于启动了一个文本编辑器
- python解释器相当于文本编辑器，去打开test.py文件，从硬盘上将test.py的文件内容读入到内存中
- python解释器解释执行刚刚加载到内存中test.py的代码
###3.总结
- python解释器具备读py文件的功能，这点同文本编辑器
- 与文本编辑器不同的是，python解释器还可以执行文件内容
##1.定义
字符--------（翻译过程）------->数字 
字符如何对应数字的标准。字符和数字的对应关系。

##2.发展史
- ASCII：1Bytes= 1个字符（英文字符/键盘上的所有其他字符），1Bytes=8bit，8bit可以表示0~2**8-1种变化，即256个字符
- Unicode：万国码，英文：2字节，中文：3字节。内存中、python3中的str都使用该编码
- UTF-8：对Unicode的压缩。英文：1字节，中文3字节。Python3解释器使用该编码
- GBK：满足中文需要，2Bytes = 1个字符

##3.文件保存和读取


##4.程序执行时发生了什么


##5.字符编码的使用
###1.Unicode跟各种编码都有一个对应关系 
- python3中，字符串变量以Unicode编码保存，执行过程中可以encode为任意字符编码的二进制格式
- python3，默认utf-8编码。指的是python解释器的默认编码。文件头中进行控制。控制的是python解释器怎样读文件（不指定时默认utf-8）
- 识别字符串，是python解释器自己数据类型的问题。eg怎么识别str（python3中识别为Unicode）

###2.执行过程中发生了什么
- s = '林' 
	- 读取出来未执行时，以Unicode格式保存在内存中
	- 执行时，python解释器在内存中开辟一块空间，以Unicode编码保存字符串'林'，变量名s指向该内存地址
- print s 
	- 未执行时，同上
	- 执行时，找到s指向的内存地址，在终端中打印地址中保存的内容（字符串），这就涉及到终端采用了什么字符编码

###3.编码与解码 Unicode---转换关系---其他字符编码
- unicode --encode--> utf-8(bytes) 编码，结果是utf-8编码的bytes格式（字节码）
- utf-8 --decode--> unicode  解码：任意格式编码转换为Unicode
- 程序未执行前，文件内容都会统一转成Unicode
- 执行过程中，可以转换为任意编码。例如声明一个字符串变量时，字符串可以是任意编码
>内存中的字符编码是unicode，用空间换时间（程序都需要加载到内存才能运行，因而内存应该是尽可能的保证快）

>硬盘中或者网络传输用utf-8，网络I/O延迟或磁盘I/O延迟要远大与utf-8的转换延迟，而且I/O应该是尽可能地节省带宽，保证数据传输的稳定性。

>所有程序，最终都要加载到内存，程序保存到硬盘不同的国家用不同的编码格式，但是到内存中我们为了兼容万国（计算机可以运行任何国家的程序原因在于此），统一且固定使用unicode，这就是为何内存固定用unicode的原因，你可能会说兼容万国我可以用utf－8啊，可以，完全可以正常工作，之所以不用肯定是unicode比utf－8更高效啊（uicode固定用2个字节编码，utf－8则需要计算），但是unicode更浪费空间，没错，这就是用空间换时间的一种做法，而存放到硬盘，或者网络传输，都需要把unicode转成utf－8，因为数据的传输，追求的是稳定，高效，数据量越小数据传输就越靠谱，于是都转成utf－8格式的，而不是unicode。

##6.python3的2种字符串
###1.unicode
str即unicode

###2.bytes
str.encode()的结果

##7.python2的2中字符串
###1.str

###2.unicode

	s = '林'
	print s #pycharm中字符编码为utf-8
	
	# python2 E:\PycharmProjects\Prcatice\day03\day03 - python2字符串.py
	# 终端执行为乱码
	#命令行解释器中直接print '林'，正常显示，因为解释器默认

python2中str默认都会encode为python解释器的编码，utf-8编码以后的bytes格式
unicode ---encode（utf-8）--->bytes
打印时，需要decode到另一个内存空间中。decode用到的是终端的编码：
pycharm中为utf-8： bytes---encode（utf-8）--->unicode
cmd中为gbk：bytes---encode（gbk） --->unicode

	s = u'林'
	print s 

这样就不会乱码，因为字符串本身就是unicode编码，可以encode为任意编码


#二、文件处理
## 文件操作 ##
###1、文件处理流程
- 打开文件，得到文件对象并赋值给一个变量
- 通过文件对象对文件进行操作（发起系统调用）
- 关闭文件（否则一直占用操作系统资源）

###2、过程分析

应用程序<-->操作系统<-->硬件  

- 应用程序向操作系统系统发起系统调用，open() 打开文件获取文件对象
- OS从硬盘中取出文件，返回一个对象f
- 应用程序将文件对象赋值给变量f，再告诉OS想做什么操作。比如读取、打印。是OS拿数据，所以读取时decode用的OS的字符编码

### 3、注意  ###
- 保存时：utf-8
- 读取时：open 可用encoding指定，默认使用当前平台的编码。存时utf-8，读时gbk，会报错
- 内核态：CPU的两种状态，通过cpu里中的寄存器进行控制。OS系统有效，对硬件有操作权限
- 用户态：应用程序，不能直接操作硬件。有操作硬件需求时，发系统调用，切到内核态，再去操作硬件拿数据，并返回。
- python2中还有file('a.txt')文件，python3中取消

##打开文件的模式
 
- r 只能读
- w 只能写不能读
- a 追加
- 
		#1. 打开文件的模式有(默认为文本模式)：
		r ，只读模式【默认模式，文件必须存在，不存在则抛出异常】
		w，只写模式【不可读；不存在则创建；存在则清空内容】
		a， 之追加写模式【不可读；不存在则创建；存在则只追加内容】
		
		#2. 对于非文本文件，我们只能使用b模式，"b"表示以字节的方式操作（而所有文件也都是以字节的形式存储的，使用这种模式无需考虑文本文件的字符编码、图片文件的jgp格式、视频文件的avi格式）
		rb 
		wb
		ab
		注：以b方式打开时，读取到的内容是字节类型，写入时也需要提供字节类型，不能指定编码
	
		#3. 了解部分
		"+" 表示可以同时读写某个文件
		r+， 读写【可读，可写】
		w+，写读【可读，可写】
		a+， 写读【可读，可写】
	
		x， 只写模式【不可读；不存在则创建，存在则报错】
		x+ ，写读【可读，可写】
		xb

##文件处理方法--r
### seek(n) ###
光标定位
### buffer和cache ###
- buffer 内存中还没有写到硬盘上的数据。当数据量小时，避免IO延迟
- cache 读到内存中，CPU经常取的数据。
### f.close() ###
判断文件是否是关闭状态
### encoding() ###
### f.name ###
文件名
### f.readable() ###
 判断文件是否是r模式打开


##文件处理方法--w

### 注意 ###
- 文件有修改的说法，操作系统中的一个概念
- 硬盘上没有修改一说，只有覆盖操作
