## 黑马Python入门到精通教程

### 1 基础知识

- PEP8 编码规范
  - 单行注释 # 后面应该有空格
  - python代码最后一行是空行

- 变量类型

  - Numbers：int 、float
  - Boolean
  - String
  - List(列表)
  - Tuple(元组)
  - Dictionary(字典)

- print

  ```python
  print('hello')
  print(123)
  print('isaac',18)
  print(1+2)
  print("我的名字是%s，我很开心",% name) # %占位符
  
  password = input('请输入密码')
  
  ```

- 循环

  ```python
  变量 = 表达式1 if 判断条件 else 表达式2 # 推荐使用扁平化代码
  while True： # 无限循环
  ```

- 字符串

  ```python
  my_str.find(sub_str,start,end) # 返回 rfind从右边开始查找
  my_str.index(sub_str,start,end) # rindex
  my_str.count(sub_str) 
  my_str.replace(old_str,new_str,count)# 替换
  my_str.split(sub_str,count) # 返回[] rsplit
  my_str.join(可迭代对象)
  
  ```

- 列表

  ```python
  # 遍历
  for i in my_list:
  	print(i)
  
  while j < len(my_list):
  	print(my_list[j])
  	j += 1
  #
  my_list.append(数据) # 向列表的尾部增加数据
  my_list.insert(下标,数据) # 插入
  my_list.extend(可迭代对象) # 
  my_list.remove(数据值)
  my_list.pop(下标) #默认删除最后一个数据，返回删除的内容
  
  my_list.sort() # 不会在原来的list中排序
  my_list.sort(reverse=True) # 升序 
  my_list[::-1] #逆置
  my_list.reverse()
  
  # enumerate 将可迭代序列中元素所在的下标和具体元素数据组合在一起 变成元组
  for i in enumerate(my_list):
  
  # 复合列表排序
  list = [{'name':'a','age':10}]
  # 匿名函数的形参 是列表中的每一个数据
  list.sort(key=lambda x: x['name'])
  
  # 列表推导式
  my_list = [(i,j) for i in range(3) for j in range(3)]
  ```

- 元组

- 字典

  ```python
  my_dict[1] = my_dict[1.0]
  my_dict.pop(key)
  my_dict.clear()
  # 遍历
  my_dict.keys()
  my_dict.values()
  my_dict.items()
  for k,v in my_dict:
      print(k,v)
      
  # 字典推导式
  my_dict = {f"name{i}":i for i in range(3)}
```
  



### 2 基础语法

- 递归

  ```python
  def get_age(num):
      age = get_age(num-1)+2
      return age
  ```

- 匿名函数

  ```python
  def my_calc(a,b,func):
  	num = func(a,b)
      return num
  my_calc(a,b,lambda a,b:a*b)
  ```

- 文件操作

  文件在硬盘中存储的格式都是二进制

  ```python
  # 1.打开文件 mode r(只读) w(写) a(追加)
  f = open(filename,mode='r',encoding=None)
  # 2.读文件 写文件
  f.read()
  f.readline()#一次读取一行的内容
  f.readlines() #一次读取所有行的内容
  f.write()
  # 3.关闭文件
  f.close()
  
  # 读取大文件
  f = open('a.txt','r',encoding='utf-8')
  while True:
      buf = f.readline()
      if buf: # if len(buf)>0 容器，可以直接作为判断条件，有数据为True
          print(buf,end='')
      else:
          # 文件读完了
          break
  
  # 以二进制形式打开 ab wb rb 
  # 文件备份，打开两个文件，拷贝
  
  import os
  os.rename('old_name','new_name')
  os.mkdir() #创建文件夹
  os.rmdir() # 移除
  os.getcwd() #获取当前目录
  
  ```

### 3 面向对象

- 操作属性

  ```python
  class 类名(object):
  	def 函数名(self): # self为该对象本身
          pass
  ```

- 魔法方法

  ```python
  __int__()
  # 类似与构造函数 创建对象的时候自动调用
  
  __str__()
  # 输出打印的结果
  
  
  __del__()
  # 析构函数
  # 对象销毁的时候调用
  # 引用计数：一块内存有多少个变量在引用
  ```

  

