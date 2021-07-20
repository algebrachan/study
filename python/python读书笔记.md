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

```



