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

### 2 面向对象

- 操作属性

  ```python
  class 类名(object):
  	def 函数名(self): # self为该对象本身
          pass
  # 私有属性
  # 在方法和属性前加两个下划线，就变为私有
  # 实例对象.__dict__ 可以查看对象具有的属性信息
  # 定义共有方法提供接口去修改私有属性
  
  @staticmethod # 定义静态方法
  
  
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
  
  __repr__()
  # 类似于 __str__()
  ```
  ```
  
  ```
  
- 继承：单继承、多继承、多层继承

  ```python
  # 重写：子类使用和父类同样的方法
  class Dog(object):
      def __init__(self,name)
      	self.age = 0
          self.name = name 
      def bark(self):
          print('汪汪汪叫....')
  
  class XTQ(Dog):
      def __init__(self,name,color):
          super().__init__(name)
          self.color = color
      def bark(self):
          print('嗷嗷嗷叫....')
          
      def see_host(self):
          print('看见主人了,',end=',')
          Dog.bark(self) # 对象.方法() 不需要传self 类名.方法 需要传self
          super(XTQ,self).bark()
          super().bark()
          pass
  # 继承中的init掌握
  
  print(XTQ.__mro__) # 查看当前类的继承顺序
  ```

- 异常

  ```python
  try:
      # 可能发生异常的代码
  except Err:
      print('发生异常')
  # 捕获多个异常 使用多个except
  else:
      # 代码没有发生异常执行
  finally:
      # 不管有没有异常都会执行
      
  # 捕获所有异常
  try:
      
  except Exception as e:
      print(e)
      pass
  
  # 抛出异常
  raise ErrObject # 当程序遇到 raise的时候，程序报错
  
  
  ```

- 模块

  ```python
  # 方法一
  # import 模块名
  # 模块名.功能名
  
  # 方法二
  # from 模块名 import 功能名1，功能名2 
  # 如果存在相同的方法名，则会覆盖
  
  as
  # as 起别名
  
  __all__ = ['name']
  # 如果定义了__all__ 则只能导入变量中定义的内容
  
  if __name__ == '__main__': # 主程序运行
  ```



## 进阶教程

- 进程

  ```python
  # 多进程
  import multiprocessing
  import os
  def dance():
      pass
  def sing():
      pass
  if __name__ == '__main__'
  	print('主进程id',os.getpid())
  	my_dance = multiprocessing.Process(target=dance,name='跳舞')
      my_sing = multiprocessing.Process(target=sing,args-(5,),kwargs={"num":5})
      my_dance.start()
      my_sing.start()
      
     
  # 守护进程
  子进程对象.daemon = True # 守护
  子进程对象.terminate() # 销毁
  ```

- 线程

  ```python
  # 线程之间是无序的
  my_func = threading.Thread(target=func,daemon=True)
  
  # 守护线程 让子线程随着主线程的结束而结束
  my_func.setDaemon(True)
  my_func.start()
  # 线程之间共享全局变量
  # 线程间会争夺资源，需要使用互斥锁
  # 1.线程等待 thread.join()让主线程进行等待
  # 2.互斥锁
  mutex = threading.Lock()

  # 上锁
  mutex.acquire()
  # 解锁
  mutex.release()
  ```
  
- 通讯

  - ip地址：网络中设备的地址

    ```shell
    ipconfig 
    ping:ip地址
    # 端口号有65536个
    ```
    
  - tcp：三次握手
  
  - socket：是进程之间通信
  
  - 客户端
  
  ```python
  import socket
  # 1.创建客户端套接字对象
  # 参数1 ipv4
  # 参数2 选择协议 SOCK_STREAM:tcp
  tcp_client_socket = socket.soceket(socket.AF_INET,socket.SOCK_STREAM)
  # 2.和服务器套接字建立连接
  # 参数：元组 (服务器IP地址,服务器端口号)
  tcp_client_socket.connect("127.0.0.1",8080)
  # 3.发送数据
  data = "123"
  data = data.encode("utf8")
  
  # 4.接收数据
  # recv会阻塞数据的到来
  recv_data = tcp_client_socket.recv(1024)
  recv_data = recv_data.decode()
  print(recv_data)
  
  # 5.关闭客户端套接字
  tcp_client_socket.close()
  ```
  
  - 服务端
  
  ```python
  # 1.创建服务器套接字对象
  # 2.绑定端口号
  # 3.设置监听
  # 4.等待接受客户端的连接请求
  # 5.接收数据
  # 6.发送数据
  # 7.关闭套接字
  
  ```
  
  
