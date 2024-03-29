Chapter10-异常处理



# 一、什么是异常

异常就是程序运行时发生错误的信号（在程序出现错误时，则会产生一个异常，若程序没有处理它，则会抛出该异常，程序终止运行），在python中,错误触发的异常如下 

![image-20190203173015565](chapter10-Exception.assets/image-20190203173015565.png)



**而错误分成两种：**

**1.语法错误导致的异常**

这种错误，根本过不了python解释器的语法检测，必须在程序执行前就改正

```python
#SyntaxError: invalid syntax
#语法错误示范一
if
#语法错误示范二
def test:
    pass
#语法错误示范三
class Foo
    pass
#语法错误示范四
print(haha
```

**2.逻辑错误导致的异常**

```python
#TypeError:int类型不可迭代
for i in 3:
    pass

#ValueError
num=input(">>: ") #输入hello
int(num)

#NameError
aaa

#IndexError
l=['amy','aa']
l[3]

#KeyError
dic={'name':'amy'}
dic['age']

#AttributeError
class Foo:pass
Foo.x

#ZeroDivisionError:无法完成计算
res1=1/0
res2=1+'str'
```



# 二、异常的种类

在python中不同的异常可以用不同的类型（python中统一了类与类型，类型即类）去标识，一个异常标识一种错误

**常用异常：**

```python
AttributeError 试图访问一个对象没有的属性，比如foo.x，但是foo没有属性x
IOError 输入/输出异常；基本上是无法打开文件
ImportError 无法引入模块或包；基本上是路径问题或名称错误
IndentationError 语法错误（的子类）；代码没有正确对齐
IndexError 下标索引超出序列边界，比如当x只有三个元素，却试图访问x[5]
KeyError 试图访问字典里不存在的键
KeyboardInterrupt Ctrl+C被按下
NameError 使用一个还未被赋予对象的变量
SyntaxError Python代码非法，代码不能编译(个人认为这是语法错误，写错了）
TypeError 传入对象类型与要求的不符合
UnboundLocalError 试图访问一个还未被设置的局部变量，基本上是由于另有一个同名的全局变量，导致你以为正在访问它
ValueError 传入一个调用者不期望的值，即使值的类型是正确的。例如文件关闭后仍进行读写操作。int转换字符串类型。int("abc")
```

**更多异常：**

```python
ArithmeticError
AssertionError
AttributeError
BaseException
BufferError
BytesWarning
DeprecationWarning
EnvironmentError
EOFError
Exception
FloatingPointError
FutureWarning
GeneratorExit
ImportError
ImportWarning
IndentationError
IndexError
IOError
KeyboardInterrupt
KeyError
LookupError
MemoryError
NameError
NotImplementedError
OSError
OverflowError
PendingDeprecationWarning
ReferenceError
RuntimeError
RuntimeWarning
StandardError
StopIteration
SyntaxError
SyntaxWarning
SystemError
SystemExit
TabError
TypeError
UnboundLocalError
UnicodeDecodeError
UnicodeEncodeError
UnicodeError
UnicodeTranslateError
UnicodeWarning
UserWarning
ValueError
Warning
ZeroDivisionError
```



# 三、异常处理

为了保证程序的健壮性与容错性，即在遇到错误时程序不会崩溃，我们需要对异常进行处理。

应用程序运行在操作系统。程序一旦出错，错误交给操作系统。如果自己没有处理，操作系统就抛出异常，程序崩溃。自己程序里处理异常，避免交给操作系统。

**如果错误发生的条件是可预知的，我们需要用 if 进行处理：在错误发生之前进行预防**

```python
AGE=10
while True:
    age=input('>>: ').strip()
    if age.isdigit(): #只有在age为字符串形式的整数时,下列代码才不会出错,该条件是可预知的
        age=int(age)
        if age == AGE:
            print('you got it')
            break
```

 

**如果错误发生的条件是不可预知的，则需要用到try...except：在错误发生之后进行处理**

```python
#基本语法为
try:
    被检测的代码块
except 异常类型：
    try中一旦检测到异常，就执行这个位置的逻辑

#举例
try:
    f=open('a.txt') #a.txt中共三行数据
    g=(line.strip() for line in f)
    print(next(g))
    print(next(g))
    print(next(g))
    print(next(g))
except StopIteration:
    f.close()
```



```python
#1 异常类只能用来处理指定的异常情况，如果非指定异常则无法处理。
s1 = 'hello'
try:
    int(s1)
except IndexError as e: # 未捕获到异常，程序直接报错
    print e

#2 异常处理的多分支
s1 = 'hello'
try:
    int(s1)
except IndexError as e:
    print(e)
except KeyError as e:
    print(e)
except ValueError as e:
    print(e)

#3 万能异常Exception
s1 = 'hello'
try:
    int(s1)
except Exception as e:
    print(e)

#4 多分支异常与万能异常
#4.1 如果你想要的效果是，无论出现什么异常，我们统一丢弃，或者使用同一段代码逻辑去处理他们，那么骚年，大胆的去做吧，只有一个Exception就足够了。
#4.2 如果你想要的效果是，对于不同的异常我们需要定制不同的处理逻辑，那就需要用到多分支了。

#5 也可以在多分支后来一个Exception
s1 = 'hello'
try:
    int(s1)
except IndexError as e: #顺序执行。判断成功一个，后面的判断就不会执行
    print(e)
except KeyError as e:
    print(e)
except ValueError as e:
    print(e)
except Exception as e:
    print(e)

#6 异常的其他机构
s1 = 'hello'
try:
    int(s1)
except IndexError as e:
    print(e)
except KeyError as e:
    print(e)
except ValueError as e:
    print(e)
#except Exception as e:
#    print(e)
else: # try中代码块没有异常发生时，会执行
    print('try内代码块没有异常，则执行我')
finally: # try中有没有异常，都会执行。
    print('无论异常与否,都会执行该模块,通常是进行清理工作，放一些回收资源的代码')

# finally应用场景
try:
    f = open('a.txt',mode='r',encoding='utf-8')
    next(f)
    next(f)
    next(f)
    next(f)
# except Exception as e:
#     print(e)
finally:
    print("执行finally……")
    f.close()
    
#7 主动触发异常
try:
    raise TypeError('类型错误')
except Exception as e:
    print(e)

#8 自定义异常
class AmyException(BaseException):
    def __init__(self,msg):
        self.msg=msg
    def __str__(self):
        return self.msg

try:
    raise AmyException('类型错误')
except AmyException as e:
    print(e)

#9 断言:assert 条件
assert 1 == 1  
assert 1 == 2

#10 总结try..except

1：把错误处理和真正的工作分开来
2：代码更易组织，更清晰，复杂的工作任务更容易实现；
3：毫无疑问，更安全了，不至于由于一些小的疏忽而使程序意外崩溃了；
```



# 四、什么时候用异常处理

有的同学会这么想，学完了异常处理后，好强大，我要为我的每一段程序都加上try...except，干毛线去思考它会不会有逻辑错误啊，这样就很好啊，多省脑细胞===》2B青年欢乐多

首先，try...except是你附加给你的程序的一种异常处理的逻辑，与你的主要的工作是没有关系的，这种东西加的多了，会导致你的代码可读性变差

然后，异常处理本就不是你写bug的擦屁股纸，只有在**错误发生的条件无法预知**的情况下，才应该加上try...except

