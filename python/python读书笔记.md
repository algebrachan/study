## Python学习手册(第4版)

### 1. 使用入门

python是一门面向对象的脚本语言，扮演角色：shell工具、控制语言、使用快捷

python的缺点：执行速度不够快

> python可以做什么

- 系统编程

- 用户图形接口

- Internet脚本

- 组件集成

- 数据库编程

- 快速原型：用于移植C或C++

- 数值计算和科学计算编程

- 游戏、图像、人工智能、XML、机器人

  

> python解释器：运行python代码的环境

程序执行 => 字节码编译 => 在PVM解释运行

.pyc文件就是编译过的文件



### 2. 类型和运算

> 内置对象

数字，字符串，列表，字典，元组，文件，集合，其他类型(None、布尔型)

python是动态类型(自动跟踪你的类型而不是要求声明代码)，强类型语言(只能对一个对象进行适合该类型的有效操作)。

list不能给列表边界外的元素赋值，为了让一个列表增大，我们可以调用append的列表方法

元组一旦创建，就不能再改变

```python
# dict 排序
D = {'a':1,'c':3,'b':2}
Ks = list(D.keys())
Ks.sort()
for key in Ks:
    print(key,'=>',D[key])
# 最新的dict排序方法
for key in sorted(D):
    print(key,'=>',D[key])
    
# dict直接取值存在的问题，没有该key就会报错
D.get('d',0)
D['d'] if 'd' in D else 0

```

十六进制 0x 开头

八进制 0o开头

二进制 0b 开头

> 内置数学工具和扩展

表达式：+ - * / >> ** &

内置数学函数：pow、abs、round、int、hex、bin

公用模块 random、math等

> python表达式操作符及程序

| 操作符                    | 描述                                             |
| ------------------------- | ------------------------------------------------ |
| yield x                   | 生成器函数发送协议                               |
| lambda  args : expression | 生成匿名函数，lambda表达式                       |
| x if y else z             | 三元选择表达式                                   |
| x or y                    | 逻辑或（只有x为假，才会计算y）                   |
| x and y                   | 逻辑与（只有x为真，才会计算y）                   |
| not x                     | 逻辑非                                           |
| x in y, x not in y        | 成员关系                                         |
| x<y,x==y,x!=y,            | 大小比较                                         |
| x\|y                      | 位或，集合并集                                   |
| x^y                       | 位异或，集合对称差                               |
| x&y                       | 位与，集合交集                                   |
| x<<y,x>>y                 | 左移或右移y位                                    |
| x+y,x-y                   | 加法/合并，减法，集合差集                        |
| x*y,x%y,x/y,x//y          | 乘法/重复，余数/格式化，除法/floor除法、截断除法 |
| -x,+x                     | 一元减法，识别                                   |
| ~x                        | 按位求补                                         |
| x**y                      | 幂运算                                           |
| x[i]                      | 索引，点号，                                     |
| x[i:j:k]                  | 分片                                             |
| x(...)                    | 调用（函数、方法、类及其他可调用的）             |
| x.attr                    | 属性引用                                         |
| (...)                     | 元组，表达式，生成器表达式                       |
| [...]                     | 列表，列表解析                                   |
| {...}                     | 字典，集合，集合和字典解析                       |

```python
# 分数类型
form fractions import Fraction
x = Fraction(1,3) # 分数 1/3
# 集合的转换，可以去重，
L = [1,2,2,3,4,5,5,6]
set(L)
>>> {1,2,3,4,5,6}
L = list(set(L))
>>> [1,2,3,4,5,6]

```

> 动态类型

变量在赋值的时候才创建,它可以引用任何类型的对象

变量名没有类型，类型属于对象。

每当一个变量名被赋予了一个新的对象，之前的那个对象占用的空间就会被回收···

```python
# 共享引用
a = 3
b = a
a = a + 2
>>> a=5,b=3 # 整数是不可变的，5是新的对象

# 共享了list引用，会在原有的基础上修改
L1 = [2,3,4]
L2 = L1
L1[0] = 24 
L1
>>> [24,3,4]
L2
>>> [24,3,4]
# 使用分片来复制列表
L1 = [2,3,4]
L2 = L1[:]
L1[0] = 24 
L1
>>> [24,3,4]
L2
>>> [2,3,4]

# == 操作符 测试两个被引用的对象是否有相同的值
# is操作符 检查对象的同一性

```

> 字符串: 一个有序的字符的集合

字符串相关操作均可使用`f''`函数来操作

```python
# 不能混合字符串和数字类型进行像+ 这样的加法
# 字符串代码转换
ord('s') # 转ASCII码
chr(115) # 转字符

# 修改字符串，不能直接在原地修改一个字符串
S = 'splot'
S = S.replace('pl','pamal')
S
>>>'spamalot'
```

> 列表

```python
L1 + L2 
L * 3
3 in L
for x in L: print(x)
L.append(4)
L.extend([5,6,7])
L.insert(I,X)
L.index(1)
L.Count(X)
L.sort()
L.reverse()
L.pop()
L[i:j] = [4,5,6] # 分片赋值
L = [x**2 for x in range(5)] 
# 等价于
L = []
for x in range(5):
    L.append(x**2)
list(map(ord,'spam'))
# map函数可做类似的工作
list(map(abs,[-1,-2,0,1,2]))
>>>[1,2,0,1,2]

# 矩阵 
matrix = [[1,2,3],[4,5,6],[7,8,9]]
matrix[1]
>>>[1,2,3]
matrix[2][0]
>>>7

# 排序
L = ['abc','ABD','aBe']
L.sort(key=str.lower,reverse=True)
sorted(L,key=str.lower,reverse=True)

```

> 字典

```python
D={}
D={'apam':2,'egg':3}
D=dict(name='bob',age=42)
D=dict(zip(keyslist,valslist))
D['food']['ham']
D.keys() # 获取键，返回一个可迭代视图，需要list()转化
D.values() # 获取值，
D.items() # 键 + 值,
D.copy()
D.get(key,default)
len(D)
D.update(D2)
D.pop(key)

# 统一初始化
dict.fromkeys(['a','b'],0)
{'a':0,'b':0}


```

> 元组、文件、其他

元组出自数学概念,通常用来表示数据库表的一行

元组的角色类似于其他语言中的“常数”声明

```python
# 文件操作
output = open(r'C:\spam','w') # 'w'是写入 'r'是读写
input = open('data')
aString = input.read()	# 把整个文件读进单一字符串
aString = input.readline() # 读取下一行
aList = input.readlines() 
```



### 3. 语句和语法



```python
# continue语句，立即调到循环的顶端
x = 10
while x:
    x = x-1
    if x % 2 !=0:continue
    print(x,end=' ')

# break语句会立刻离开循环
while True:
    name = input('Enter name:')
    if name == 'stop':break
    age = input('Enter age:')
    print('Hello',name,'=>',int(age)**2)

# for循环
for <target> in <object>:
    <statements>
    if <test>:break
    if <test>:continue
else:
    <statements>

# for循环中常使用range(n)
# 并行遍历 zip

# 可迭代对象
S = 'spam'
for (offset,item) in enumerate(S):
    print(item,'appear at offset',offset)


```

> 迭代器

```python
# 文件迭代器
f = open('xx.py')
f.readline()
# __next__() 方法 也可迭代，但是到文件末尾时，__next__会引发内置的StopIteration异常

# 内置函数next() 自动调用对象的__next__方法
# 迭代器 iter()
L = [1,2,3]
I = iter(L)
next(I)
next(I)
# 迭代器有记录功能，一旦迭代了，上一条的数据不会重复,dict对象可用迭代器遍历
# map zip filter迭代器,遍历一次，就用尽了,只支持一个迭代器

# range()支持多个爹地阿奇

# 字典视图迭代器 它返回连续的键，无需在环境中调用keys

```

> 文档

| 形式                | 角色                       |
| ------------------- | -------------------------- |
| #注释               | 文件中的文档               |
| dir函数             | 对象中可用属性的列表       |
| 文档字符串 __ doc__ | 附加在对象上的文件中的文档 |



### 4. 函数

函数是python为了代码最大程度的重用和最小化代码冗余而提供的最基本的程序结构

def	创建了一个对象并将其赋值给某一变量名

lambda	创建了一个对象但将其作为结果返回

return	将一个结果对象发送给调用者

yield	向调用者发回一个结果对象，但是记住它离开的地方

global	声明了一个模块级的变量并被赋值

```python
# 可以将函数赋值给一个不同的变量名，并通过新的变量名进行了调用
othername = func
othername()

```

> 作用域

- 内嵌的模块是全局作用域
- 全局作用域的作用范围仅限于单个文件
- 每次对函数的调用都创建了一个新的本地作用域
- 赋值的变量名除非声明为全局变量或非本地变量，否则均为本地变量 global来声明

```python
# 工厂函数
def maker(N):
    def action(X):
        return X ** N
    return action
f = maker(2)
g = maker(3)
f(3) # 9
g(3) # 27
# nonlocal使用 内嵌变量可以改变父函数的值
def tester(start):
    state = start
    def nested(label):
        nonlocal state
        print(label,state)
        state += 1
    return nested

F = tester(0)
F('spam')
```

> 参数

```python
def(*args):print(args) # 把所有参数收集到一个新的元组

def(**args):print(args) # 把关键字参数传递给一个新的字典
    
# 解包参数

```

> 函数高级

函数式编程工具：filter，reduce，map

```python
filter((lambda x:x>0),range(-5,5)) # 过滤
reduce((lambda x,y:x*y),[1,2,3,4]) # 计算 24
# map是对每一个元素进行操纵 
```

yeild 保存状态

return 直接返回值



### 5. 模块

- 模块语句会在首次导入时执行
- 顶层的赋值语句会创建模块属性
- 模块的命名空间能通过属性_dict_或dir获取
- 模块是一个独立的作用域

包导入，必须包含init.py这个包文件

```python
#  以__name__进行单元测试
#  模块设计理念
# 1.总是在python的模块内编写代码
# 2.模块耦合要降低到最低：全局变量
# 3.最大化模块的粘合性：统一目标
# 4.模块应该少去修改其他模块的变量

```



### 6. 类和OOP

类通过继承进行定制

- 超类(父类)列在了类开头的括号中
- 类从其超类中继承属性
- 实例会继承所有可读取类的属性
- 每个object.attribute都会开启新的独立搜索
- 逻辑的修改是通过创建子类，而不是修改超类

```python
# class中
# method(self,params) # class中的方法都需要加self

# 类与字典的关系
# x.__dict__ 类实例x转化为字典

# __str__ 方法打印 类显示的文字
```

> 类与实例

类产生多个实例对象

- class语句创建类对象并将其赋值给变量名
- class语句内的赋值语句会创建类的属性
- 类属性提供对象的状态和行为

实例对象是具体的元素

- 像函数那样调用类对象会创建新的实例对象
- 每个实例对象继承类的属性并获得了自己的命名空间
- 在方法内对self属性做赋值运算会产生每个实例自己的属性

> 类代码编写

```python
# python会自动把实例方法的调用对应到类方法函数
instance.method(args...)
class.method(instance,args...)

# 希望子类实现父类的方法
class Super:
    def delegate(self):
        self.action()
#    def action(self):
#       	raise NotImplementedError('action must be defined!')
	@abstractmethod  # 等同于上述的作用 不能产生一个实例，除非在类的较低层级定义了该方法
    def action(self):
        pass
class Sub(Super):
    def action(self):
        print('spam')

```

> 运算符重载

```python
#	__init__	构造函数	X=Class(args)
#	__del__		析构函数	X对象回收
#	__add__		运算符+	X+Y
#	__or__		运算符|
#	__repr__,__str__	打印	print(X) repr(X) str(X)
#	__iter__,__next__	迭代环境
#	__new__		在init之前创建对象
#	__getitem__	索引
#	__setitem__	分片
#   __dict__    类的属性
```

```python
# Call表达式: __call__
# 当调用实例时，使用__call__方法

class Prod:
    def __init__(self,value)
    	self.value = value
    def __call__(self,other)
    	return self.value * other
x = Prod(2)
x(3)
>>>6
x(4)
>>>8

# self.__class__.__name__ 获取一个实例的类的名称

```



### 7. 异常和工具

```python
try/except # 捕捉异常 并处理

try/finally # 无论是否发生异常都执行

raise # 手动抛出异常

assert # 有条件地在程序代码中触发异常

with/as

```

**异常对象**

### 8. 高级话题

#### 字符串

```python
# ASCII 互转
ord('a')

chr(97) # 

hex(97) # 16进制

# 字符串转byte
str.encode()
bytes(S,encoding)

# byte转字符串
bytes.decode()
str(B,encoding)

import pickle # 使用模块转化
```

#### 

## Python核心编程(第3版)

### 1. 正则表达式

re模块

match()

search()

compile()

```python
# match 匹配 从字符串的起始部分开始匹配模式
m = re.match('foo','foo')
if m is not None: m.group() # 输出
# 匹配多个字符串 使用择一符号|隔开
    
# search 搜索 搜索字符串出现
m = re.search('foo','seafood')
if m is not None: m.group()

```

### 2. 网络编程

**套接字**是计算器网络数据结构，它提现了上节中所描述的“通信端点”的概念。在任何类型的通信开始之前，网络应用程序必须创建套接字。可以将它们比作电话插孔，没有它将无法通信

**tcp服务端**

```python
from socket import *
from time import ctime

HOST = 'localhost'
PORT = 21567
BUFSIZ = 1024
ADDR = (HOST,PORT)

tcpSerSock = socket(AF_INET,SOCK_STREAM)
tcpSerSock.bind(ADDR)
tcpSerSock.listen(5)

while True:
    print('waiting for connection...')
    tcpCliSock,addr = tcpSerSock.accept()
    print('...connect from:',addr)
    
    while True:
        data = tcpCliSock.recv(BUFSIZ)
        if not data:
            break
        tcpCliSock.send('[%s] %s' %(bytes(ctime(),'utf-8'),data))
        
    tcpCliSock.close()
tcpSerSock.close()

```

**tcp 客户端**

```python
from socket import *
HOST = 'localhost'
PORT = 21567
BUFSIZ = 1024
ADDR = (HOST,PORT)

tcpCliSock = socket(AF_INET,SOCK_STREAM)
tcpCliSock.connect(ADDR)

while True:
    data = input('>')
    if not data:
        break
    tcpCliSock.send(data.encode('utf-8'))
    data = tcpCliSock.recv(BUFSIZ)
    if not data:
        break
    print(data.decode('utf-8'))
tcpCliSock.close()
```

**UDP服务器**

```python
from socket import *
from datetime import datetime

HOST = '127.0.0.1'
PORT = 21568
BUFSIZ = 1024
ADDR = (HOST,PORT)

udpSerSock = socket(AF_INET,SOCK_DGRAM)
udpSerSock.bind(ADDR)
print('waiting for message...')

while True:
  data,addr = udpSerSock.recvfrom(BUFSIZ)
  data = data.decode('utf-8')
  data = f'{data}:{datetime.now()}'
  udpSerSock.sendto(data.encode('utf-8'),addr)
  print('...received from and returned to:',addr)

udpSerSock.close()
```

**upd客户端**

```python
from socket import *

HOST = 'localhost'
PORT = 21568
BUFSIZ = 1024
ADDR = (HOST,PORT)

udpCliSock = socket(AF_INET,SOCK_DGRAM)

while True:
  data = input('> ')
  if not data:
    break
  udpCliSock.sendto(data.encode('utf-8'),ADDR)
  data,ADDR = udpCliSock.recvfrom(BUFSIZ)
  if not data:
    break
  print(data.decode('utf-8'))

udpCliSock.close()
```

**SocketServer TCP**

```python
from datetime import datetime
from socketserver import (TCPServer as TCP,StreamRequestHandler as SRH)


class MyRequestHandler(SRH):
  def handle(self):
    print('...connect from:',self.client_address)
    self.data = self.request.recv(BUFSIZ).strip()
    self.request.sendall(f'{self.data}:{datetime.now()}'.encode('utf-8'))

tcpServ = TCP(ADDR,MyRequestHandler)
print('waiting for connection...')
tcpServ.serve_forever()
```



- 注意事项
  - 数据传输是二进制，需要encode和decode
  - tcp是wait connect
  - udp是wait message



### 3. 因特网客户端编程

文件传输	FTP

网络新闻	Usenet	NNTP

电子邮件	SMTP	POP	IMAP	POP3	IMAP4



### 4. 多线程编程

进程：是一个执行中的程序。有自己的地址空间、内存、数据栈以及其他用于跟踪执行的辅助数据。

线程：在同一个进程下执行，共享相同的上下文

守护线程：退出时，不需要等待这个线程执行完成

Thread类：

| 属性                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Thread对象数据属性**                                       |                                                              |
| name                                                         | 线程名                                                       |
| ident                                                        | 线程的标识符                                                 |
| daemon                                                       | 布尔标志，表示这个线程是否是守护线程                         |
| **Thread对象方法**                                           |                                                              |
| __init__(group=None,target=None,args=(),kwargs={},verbose=None,daemon=None) | 实例化一个线程对象，需要有一个可调用的target，参数，         |
| start()                                                      | 开始执行该线程                                               |
| run()                                                        | 定义线程功能的方法                                           |
| join(timeout=None)                                           | 直至启动的线程终止之前一直挂起，除非给出timeout，否则一直阻塞 |

**使用threading模块**

```python
import threading
from datetime import datetime
from time import sleep

loops = [4,2]

def loop(nloop,nsec):
  print('start loop',nloop,'at:',datetime.now())
  sleep(nsec)
  print('loop',nloop,'done at:',datetime.now())

def main():
  print('starting at:',datetime.now())
  threads = []
  nloops = range(len(loops))

  for i in nloops:
    t = threading.Thread(target=loop,args=(i,loops[i]))
    threads.append(t)

  for i in nloops:
    threads[i].start()

  for i in nloops:
    threads[i].join() # join 将子线程逻辑同步到主线程，

  print('all DONE at:',datetime.now()) # 使用join的情况下，子线程执行完毕才会执行该语句

if __name__ == '__main__':
    main()
    
```

```python
# 使用可调用的类
import threading
from datetime import datetime
from time import sleep

loops = [4,2]

def loop(nloop,nsec):
  print('start loop',nloop,'at:',datetime.now())
  sleep(nsec)
  print('loop',nloop,'done at:',datetime.now())

class ThreadFunc(object):
  def __init__(self,func,args,name=''):
    self.name = name
    self.func = func
    self.args = args
  
  def __call__(self): # 可执行函数
    self.func(*self.args)

def main():
  print('starting at:',datetime.now())
  threads = []
  nloops = range(len(loops))

  for i in nloops:
    t = threading.Thread(target=ThreadFunc(loop,(i,loops[i]),loop.__name__))
    threads.append(t)

  for i in nloops:
    threads[i].start()

  for i in nloops:
    threads[i].join()

  print('all DONE at:',datetime.now())
  

if __name__ == '__main__':
    main()

# 子类化的Thread
class MyThread(threading.Thread):
    def __init__(self,func,args,name=''):
        threading.Thread.__init__(self)
        self.name = name
        self.func = func
        self.args = args
        
    def getResult(self):
        return self.res
    
    def run(self): # 重写run函数
        self.func(*self.args)
# t = MyThread(loop,(i,loops[i]),loop.__name__)
```

**生产者-消费者**

```python
import threading
from datetime import datetime
from time import sleep
from random import randint
from queue import Queue

class MyThread(threading.Thread):
  def __init__(self,func,args,name=''):
    threading.Thread.__init__(self)
    self.name = name
    self.func = func
    self.args = args

  def run(self):
    self.func(*self.args)

def writeQ(queue):
  print('producing object for Q...')
  queue.put('xxx',1)
  print('size now',queue.qsize())

def readQ(queue):
  val = queue.get(1)
  print(val,'consumed object from Q ... size now',queue.qsize())

def writer(queue,loops):
  for i in range(loops):
    writeQ(queue)
    sleep(randint(1,3))

def reader(queue,loops):
  for i in range(loops):
    readQ(queue)
    sleep(randint(2,5))

funcs = [writer,reader]
nfuncs = range(len(funcs))

def main():
  nloops = randint(2,5)
  print('nloops',nloops)
  q = Queue(32)

  threads = []
  for i in nfuncs:
    t = MyThread(funcs[i],(q,nloops),funcs[i].__name__)
    threads.append(t)
  
  for i in nfuncs:
    threads[i].start()

  for i in nfuncs:
    threads[i].join()

  print('all DONE')

if __name__ == '__main__':
    main()
```

- 注：
  - join()方法只有在你需要等待线程完成的时候才是有用的
- 线程替代方案
  - subprocess模块
  - multiprocessing模块: 子进程
  - concurrent.futures模块

### 5. GUI编程

Tkinter

**GUI编程介绍**

窗口和控件

事件驱动处理

布局管理器

```python
import tkinter

if __name__ == '__main__':
    top = tkinter.Tk(screenName='wc')
    label = tkinter.Label(top,text='hello world!')
    label.pack() # pack() 渲染
    quit = tkinter.Button(top,text='hello world!',command=top.quit())
    quit.pack()
    tkinter.mainloop()
```

- 注：只做介绍，一般不使用python做GUI



### 6. 数据库编程

ORM:将纯SQL转为对象处理

SQLAlchemy

SQLObject



PyMongo:连接mongodb的工具



### 7. *Microsoft Office编程

COM:客户端编程



### 8. python扩展

任何可以集成或导入另一个python脚本的代码都是一个扩展。这些新代码可以使用纯python编写，也可使用C和C++这样的编译语言编写



### 9. Web客户端和服务器

urlparse模块 用于处理URL字符串

urllib模块

​	urlopen() 返回一个文件类型对象

​	f.read()	f.readline() f.readlines() f.close() f.fileno()



