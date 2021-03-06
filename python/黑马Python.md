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

### 服务端编程

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
  
  - tcp客户端
  
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
  
  - tcp服务端
  
  ```python
  import socket
  import multprocessing
  
  def handler_client_request(client_socket):
      """处理客户端请求"""
      
      while True:   
      # 5.接收数据
      # 参数：接受数据的大小（字节）
      client_data = client_socket.recv(1024)
      
      # 如果接受到的数据长度为0 则证明客户端关闭
      if len(client_data) == 0:
          print("客户端关闭了")
          break
          
      # 对二进制解码
      client_data = client_data.decode()
      print(client_data)
  
      # 6.发送数据
      send_data = "123".encode()
      client_socket.send(send_data)
  
  
  def main():
  # 1.创建服务器套接字对象
  # 参数1 ipv4
  # 参数2 选择协议 SOCK_STREAM:tcp
  tcp_server_socket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
  
  # 端口复用设置: 一旦服务端关闭 端口立马释放
  # setsocketopt: 设置socket选项
  # 参数1：socket选项列表（SQL）
  # 参数2：地址复用
  # 参数3：True开启选项 False(默认不开启)
  tcp_server_socket.setsocketopt(socket.SQL_SOCKET,socket.SO_REUSEADDR,True)
  
  # 2.绑定端口号
  # 参数：元组（两个元素）元素1：IP地址 元素2：端口号
  # 不写默认就是本机ip地址
  tcp_server_socket.bind(("192.168.144.28",8080))
  
  # 3.设置监听
  # 参数：最大监听个数
  # 从主动套接字变成被动套接字
  tcp_server_socket.listen(128)
  
  while True:
      # 4.等待接受客户端的连接请求
      # 返回值是一个元组：元素1 和客户端进行通讯的socket 元素2 客户端的地址信息
      # 拆包语法，元组拆包
      client_socket,client_addr = tcp_server_socket.accept()
  
      # 创建一个子进程
      sub_process = multprocessing.Process(target=handler_client_request,args=(client_socket,))
      
      sub_process.start()
  
  # 7.关闭套接字
  client_socket.close()
  tcp_server_socket.close()
  
  if __name__ == '__main__':
      main()
  
  ```
  
  - 动态绑定端口
  
  ```python
  import sys
  print(sys.argv[1])# 获取命令行输入参数
  ```



### 数据库sql

- 普通查询

  ```sql
  -- 条件查询
  select * from students where age>18;
  select * from students where age>18 and gender=2;
  
  -- 模糊查询 %任意多个 _一个空
  select * from students where name like "小%"
  
  -- 范围查询 
  select name from students where not age in (18,34);
  select * from students where age between 18 and 34;
  select * from students where height (not) is null;
  
  -- 排序查询
  select * from students where (age between 18 and 34) and gender=1 order by age asc; 
  select * from students where (age between 18 and 34) and gender=1 order by height desc; 
  
  -- 聚合函数 count max min sum
  select count(*) from students where gender=1;
  
  -- 分页查询 limit 页数-1 每页数量
  select * from students limit 0,5;
  
  -- 分组查询
  select gender,group_concat(name) from students group by gender;
  
  -- 子查询
  select * from students where height>(select avg(height) from students);
   
  ```

- 连接查询

  ```sql
  -- 内连接 ？应用场景 
  -- 语法
  select 字段 from 表1 inner join 表2 on 表1.字段1 = 表2.字段2
  select * from students inner join classes on students.cls_id=classes.id;
  
  -- 外连接
  -- 左连接：查询的结果为两个表匹配到的数据和左表特有的数据，以左表为主表
  select * from students left join classes on students.cls_id=classes.id where classes.name is null;
  
  -- 自连接
  select c.* from areas as c inner join areas as p on c.pid = p.id where p.title='陕西省';
  
  ```

- pymysql

  ```python
  import pymysql
  # 1 建立连接
  conn = pymysql.connect(host='localhost',port=3306,user='root',password='mysql')
  
  # 2 创建游标
  cur = conn.cursor()
  
  # 3 执行sql
  sql = "select * from goods where name=%s;"
  cur.excute(sql,[good_name]) # 防止sql注入的写法
  
  # 4 获取数据
  data = cur.fechall()
  print(data) # 元组类型 元组中的每一个元素都是一个数据库中的数据
  # 只要对数据库中的数据进行修改，就需要进行提交操作
  conn.commit()
  
  # 5 关闭
  cur.close()
  conn.close()
  ```

- 事务

  - 原子性：不可分割

  - 一致性：要么成功，要么失败

  - 隔离性：同时只能一个事务操作

  - 持久性：事务一旦提交，所有都保存

- 索引

  alter table test_index add index(title)

### 高级编程

- 拷贝

  ```python
  # 普通赋值
  b = a # 地址的复制
  # 浅拷贝 地址不同 没有嵌套的时候安全
  b = copy.copy(a)
  
  # 深拷贝 安全
  b = copy.deepcopy(a)
  
  ```
  
- 闭包

  ```python
  def func_out(data):
      def func_in():
          print(data)
      return func_in
  func = func_out(10)
  func() # 10
  ```

- 装饰器

  ```python
  # 装饰器：在不改变原有的函数代码的前提下， 给函数增加新的功能
  def func_out(func):
      def func_in(a):
          print('验证')
          func(a)
      return func_in
  
  @func_out #login = func_out(login) 包装的纸
  def login(a):
      print('登陆',a)
      
  login(a) # 验证 登陆
  # 结论1：调用login() 相当于 ===> func_in()
  #		调用被装饰者的函数就相当于调用闭包中的内层函数
  # 结论2：外层函数的参数 func ==> 原始的login
  
  # 通用版本的装饰器 装饰器只能装饰函数
  def func_out(func):
        def func_in(*args,**kwargs):
    		ret = func(*args,**kwargs)
            return ret   
        return func_in
  
  @func_out('+') #装饰器参数 外套一个装饰器
  def my_test():
  	return 100
  
  a = my_test()
  print(a)
  
  # 类装饰器
  
  class Func:
  	def __init__(self,fn):# fn就是用来保存原始的被装饰的函数      
          self.__fn = fn # __私有属性
    	# 让我们的对象() 就可以直接调用这个call方法
   	def __call__(self): # 可调用对象
        print("this is call")
        self.__fn()
  @Func # my_test = Func(my_test)
  def my_test():
  	print("登陆")
    
  my_test()
  ```

- property属性

  ```python
  # 把方法当做属性一样去使用
  # 这个方法一定有返回值
  class Person:
      def __init__(self,age):
          self.__age = age #私有属性
      
      @property
      def age(self):
          return self.__age
      @age.setter
      def age(self,new_age):
          if new_age > 200:
              print("错误")
          else:
              self.__age = new_age
      # 在已有的基础上修改
      age = property(get_age,set_age)
      
      
  p = Person(100)
  print(p.age) # 100
  p.age = 200
  print(p.age) # 200
  ```

- 生成器：

  - 节省内存空间
  - 记录函数执行状态

  ```python
  # 生长器可以记录代码的执行状态
  # 节省空间资源
  def func():
      for i in range(5):
          print("start")
          yeild i
          print("end")
  a = func() # 创建了生成器a
  ret = next(a) # 执行生成器 next一旦超出范围会报异常，（异常处理）
  for i in a:
      print(i)
  ```

- 正则表达式

  ```python
  import re 
  # 匹配
  result = re.match(正则表达式,要匹配的字符串)
  # 提取数据
  result.group() 
  ```



## python企业级开发

### redis相关

- [github-py-redis](https://github.com/andymccurdy/redis-py)
- [redis总结](..\database\redis.md)

### git相关

- [git总结](..\git\git.md)

### Django

- [官方说明文档](https://docs.djangoproject.com/en/3.1/)

```python
# 创建项目
django-admin startproject name
# 创建子应用
python manage.py startapp book
# 运行
python manage.py runserver
# 迁移数据库
python manage.py migrate
# 创建超级管理员
python manage.py createsuperuser
```

#### ORM

- 定义模型类

```python
""" models.py 
"""
from django.db import models

class BookInfo(models.Model):
    #id
    name=models.CharField(max_length=10,unique=True)
    pub_date=models.DateField(null=True)
    readcount=models.IntegerField(default=0)
    commentcount=models.IntegerField(default=0)
    is_delete=models.BooleanField(default=False)
    
    class Meta:
        db_table='bookinfo' # 修改表的名字  默认为类名的小写下划线格式
        varbose_name='名字' # 站点使用
    
    def __str__(self):
    	return self.name
    
class PeopleInfo(models.Model):
    
    #定义一个有序字典
    GENDER_CHOICE={
        (1,'male'),
        (2,'female')
    }
    name=models.CharField(max_length=10)
    gender=models.BooleanField(choices=GENDER_CHOICE,default=1)
    description=models.CharField(max_length=100,null=True)
    id_delete=models.Boolean(default=False)
    # 外键
    book=models.ForeignKey(BookInfo,on_delete=models.CASCADE)

```

- 模型迁移

```python
# 生成迁移文件
python manage.py makemigrations 
# 进行迁移
python manage.py migrate

```

- 数据库配置

```python
# 默认的sqllite3配置
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
# mysql配置
DATABASES = {
    'default': {
        'ENGINE':'django.db.backends.mysql',
        'HOST':'127.0.0.1',
        'PORT':3306,
        'USER':'root',
        'PASSWORD':'123456',
        'NAME':'db_name'
    }
}
# 下载mysqlclient 驱动
pip install mysqlclient

- 数据操作

​```python
from book.models import BookInfo
# 新增数据
# 方式1
book = BookInfo(
	name='Django',
	pub_date='2020-1-1',
	readcount=10
)
book.save()
# 方式2
BookInfo.objects.create(
	name='Django',
	pub_date='2020-1-1',
	readcount=10
)

# 修改数据
book=BookInfo.objects.get(id=6)
book.name='运维开发'
book.save()
BookInfo.objects.filter(id=6).update(name='爬虫入门',commentcount=666)
book.delete()

# 查询
BookInfo.objects.get(id=1) # 获取一个
BookInfo.objects.all() # 获取所有
BookInfo.objects.count()
BookInfo.objects.filter() #得到的是列表

# 两个属性补交 F对象
# objects.filter(readcount_gte=F('commentcount'))
# Q对象 | 或 & 并 ~ 非
BookInfo.objects.filter(readcount_gt=20,id_lt=3)
BookInfo.objects.filter(Q(readcount_gt=20)|Q(id_lt=3))

# 聚合函数 Sum Max Min Avg Count
# objects.aggregate(Xxx('字段名'))
# order_by('字段名')

# 管理查询，存在外键
book=BookInfo.objects.get(id=1)
book.peopleinfo_set.all()

# 关联查询 模型类名.objects.(关联模型类名小写__字段名__运算符=值)
BookInfo.objects.filter(peopleinfo__name__exact='郭靖')
BookInfo.objects.filter(peopleinfo__name='郭靖')
```

#### http

```python
from django.urls import converters

# url 路径参数
urlpatterns = [
    path('<int:cat_id>/<goods_id>/',goods)
]

from django.http import HttpResponse,JsonResponse

def index(request):
    print('path',request.path)
    print('GET',request.GET) # QueryDict对象
    print('POST',request.POST) # 先注释验证     # 'django.middleware.csrf.CsrfViewMiddleware',
    body = request.body.decode()
    print('body',body) # 获得json形式字符串
    json.loads(body)
    print('META',request.META) # dict
	# 获取服务端ip
    if request.META.has_key('HTTP_X_FORWARDED_FOR'):
		ip =  request.META['HTTP_X_FORWARDED_FOR']
	else:
		ip = request.META['REMOTE_ADDR']

    res = {'name':'wc','item':123}
    # return HttpResponse(json.dumps(res))
    return JsonResponse(data=res,safe=False)

	
```

- 跨域问题

```python
# settings文件中设置如下
INSTALLED_APPS = [
    ...
    'corsheaders',
	...
]
MIDDLEWARE_CLASSES = [
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware', # 注意顺序
]
#跨域增加忽略
CORS_ALLOW_CREDENTIALS = True
CORS_ORIGIN_ALLOW_ALL = True
ALLOWED_HOSTS=['*']
CORS_ALLOW_METHODS = [
    'DELETE',
    'GET',
    'OPTIONS',
    'PATCH',
    'POST',
    'PUT',
    'VIEW',
]
CORS_ALLOW_HEADERS = [
    'XMLHttpRequest',
    'X_FILENAME',
    'accept-encoding',
    'authorization',
    'content-type',
    'dnt',
    'origin',
    'user-agent',
    'x-csrftoken',
    'x-requested-with',
    'Pragma',
]
```

#### 状态

```python
# cookie
def set_cookie(request):
    username = request.GET.get("username")
    request.session['user_id']=1
    request.session['user_name']=username
    response=HttpResponse('set_cookie')
    response.set_cookie('name',username)
    return response

def get_session(request):
    user_id = request.session.get('user_id')
    user_name = request.session.get('user_name')
    content = '{},{}'.format(user_id,user_name)
    return HttpResponse(content)

session.clear() #删除值 v
session.flush() #删除k-v
session.set_expriy(value)

```

- 中间件

```python
# 视图处理前 从上到下 视图处理后 从下到上
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
from django.utils.deprecation import MiddlewareMixin
class TestMiddleWare(MiddlewareMixin):
    def process_request(self,request):
        print('请求前 处理')
        
    def process_response(self,request,response):
        print('响应前 处理')
        return response
```

#### 项目实战

- 目录结构

  - apps：子应用包
  - libs：第三方库包
  - utils：公共类包
  - templates：模板包

- 实例

  ```python
  from django.views import View,APIView
  from django.http import JsonResponse
  
  class UsernameCountView(View): # 或者继承APIView
      def get(self,request,usename):
          count = User.objects.filter(username=username).count()
          return JsonResponse('code':1)
      def post(self,request):
          return 1
      def put(self,request):
          return 2
      def delete(self,request):
          return 3
      
  # 短信验证码生成随机6位数字
  from random import randint
  sms_code = '%06d'%randint(0,999999)
  ```

- 管道技术--pipeline

  ```python
  r = redis.Redis(...)
  pipe = r.pipeline()# 创建一个管道
  pipe.set('foo','bar')
  pipe.execute()
  ```

- [Celery](https://docs.celeryproject.org/en/master/django/first-steps-with-django.html#using-celery-with-django)

  ```python
  pip install -u celery
  import os
  
  from celery import Celery
  os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'proj.settings')
  app = Celery('proj')
  app.config_from_object('django.conf:settings', namespace='CELERY')
  app.autodiscover_tasks()
  
  @app.task(bind=True)
  def debug_task(self):
      print(f'Request: {self.request!r}')
      
  # proj/proj/__init__.py:
  from .celery import app as celery_app
  
  __all__ = ('celery_app',)
  
  # settings.py
  CELERY_TIMEZONE = "Australia/Tasmania"
  CELERY_TASK_TRACK_STARTED = True
  CELERY_TASK_TIME_LIMIT = 30 * 60
  
  # demoapp/tasks.py:
  # Create your tasks here
  from celery import shared_task
  from demoapp.models import Widget
  
  @shared_task
  def add(x, y):
      return x + y
  
  ```
  
- 数据加密

  ```python
  # 使用itsdangerous 加密
  from itsdangerous import TimedJSONWebSignatureSerializer as Serializer
  s = Serializer(secret_key='',expires_in=3600)
  s.dumps('openid':'123456') # 加密
  s.loads(token) # 解密
  
  ```

### Docker

- 镜像Image：虚拟环境
- 容器Container：没有可视化界面的虚拟机，运行在虚拟环境的项目，运行的镜像就是容器
- 仓库Repository

#### 镜像操作

```shell
sudo docker image ls
sudo docker image pull redis
sudo docker save -o redis.rar redzis

sudo docker container ls -a # 正在运行的容器
sudo docker rm -f $(sudo docker ps -a -q)
sudo docker run [选项] 镜像名 [指令]

# sudo docker run -it ubuntu
# 守护进程的方式运行
sudo docker run -dit --name=myubuntu ubuntu:latest
sudo docker exec -it 容器id /bin/bash # 进入终端 只能进入正在运行的容器

# 关闭
sudo docker container stop myubuntu


```

#### Fastdfs 分布式文件系统

- [介绍](https://www.jianshu.com/p/b7c330a87855)
- [下载](https://sourceforge.net/projects/fastdfs/)

#### docker搭建mysql主从

```shell
# 下载
sudo docker image mysql:版本号
# 或者
sudo docker -i 文件路径/mysql_docker_5370.tar

# 指定MySQL从机配置文件
cd ~
mkdir mysql_slave
cd mysql_slave
mkdir data
cp -r /etc/mysql/mysql.conf.d ./

# 编辑 ~/mysql_slave/mysql.conf.d/mysqld.cnf
# 从机端口号
port = 8306
# 关闭日志
general_log = 0
# 从机唯一编号
server-id = 2

# 安装从机mysql
sudo docker run --name mysql-slave -e MYSQL_ROOT_PASSWORD=mysql -d --network=host -v /home/ubuntu/mysql_slave/data:/var/lib/mysql -v /home/ubuntu/mysql_slave/mysql.conf.d:/etc/mysql/mysql.conf.d mysql:5.7.30

# 从服务器访问
mysql -uroot -pmysql -h 127.0.0.1 --port=8306

# 主从同步实现
# 开启日志
general_log_file = /var/log/mysql/mysql.log
general_log = 1
server_id = 1
log_bin = /var/log/mysql/mysql-bin.log
# 从机备份主机原有数据
mysqldump -uroot -pmysql --all-databases --lock-all-tables > ~/master_db.sql
mysql -uroot -h127.0.0.1 --port=8306 < ~/master_db.sql
# 主从同步实现
mysql -uroot -pmysql
GRANT REPLICATION SLAVE ON *.* TO 'slave'@'%' identified by 'slave';
FLUSH PRIVILEGES;
SHOW MASTER STATUS;
# docker 中的 mysql 连接主机mysql
mysql -uroot -pmysql -h127.0.0.1 --port=8036
change master to master_host='127.0.0.1',master_user='slave',master_password='slave',master_log_file='mysql-bin.xxx',master_log_pos=xxxx;
start slave;
show slave status \G
```



### 数据操作

- base64加密

  ```python
  import pickle
  import base64
  dict = {'a':1}
  b = pickle.dumps(dict) # dict转 byte
  b1 = base64.b64encode(b) # byte转 base64
  b2 = base64.b64decode(b1) # base64解码 byte
  d = pickle.loads(b2) # byte 转 dict
  
  ```

### 性能优化

- 数据库主从搭建，读写分离（主机用来写，从机用来读）

  


## 数据挖掘

### Jupyter使用

https://jupyter.org/

```
pip install jupyterlab
pip install notebook
pip install voila
jupyter notebook
```

### Matplotlib

- 专门用于开发2D图表
- 使用起来及其简单
- 以渐进、交互式实现数据可视化

**三层结构**

- 容器层：Canvas、Figure、Axes
- 辅助显示层：
- 图像层

#### 折线图

```python
import matplotlib.pyplot as plt
import random

# 1.创建画布
plt.figure(figsize=(20,8),dpi=80)

x = range(60)
y_shanghai = [random.uniform(15,18) for i in x]

# 2.绘制折线图
plt.plot(x,y_shanghai)

# 修改x y刻度
x_label = [f'11:{i}' for i in x]
plt.xticks(x[::5],x_label[::5])
plt.yticks(range(0,40,5))

# 添加网格
plt.grid(True,linestyle='--',alpha=0.5)
plt.xlabel("time")
plt.ylabel("temp")
plt.title("title")

plt.savefig("test78.png")
# 3.显示图像
plt.show() # 当调用show时，会释放对象
```

```python
# 散点图
import matplotlib.pyplot as plt
plt.figure(figsize=(20,8),dpi=80)
plt.scatter(x,y)
plt.show()

# 柱状图
plt.bar()
# 直方图
plt.hist()
# 饼图
plt.pie(x,labels=,autopct=,colors)
```

### Numpy

numpy - 数值计算库

num - numerial 数值化的

py - python

**ndarray**

​	n - 任意个

​	d - dimension维度

​	array - 数组



#### ndarray

属性

|     属性名字     |      属性解释      |
| :--------------: | :----------------: |
|  ndarray.shape   |   数组维度的元组   |
|   ndarray.ndim   |      数组维数      |
|   ndarray.size   |  数组中的元素数量  |
| ndarray.itemsize | 一个数组元素的长度 |
|  ndarray.dtype   | 数组元素的类型 ‘’  |

```python
# 生成数组的方法
import numpy as np

np.array()
 
np.zeros(shape) # shape 几行几列的元组
np.ones(shape)
np.copy() 	# 深拷贝
np.asarray() # 浅拷贝
np.linspace(0,100,10) [0,100] # 等距离
np.arange(a,b,c) # [a,b) c步长

# 生成随机数组 
np.random.uniform(low=0.0,high=1.0,size=None) # 返回[0.0,1.0)内的一组均匀分布的数

# 正态分布
np.random.normal(loc=0.0,scale=1.0,size=None)
# loc: 均值
# scale: 标准差

# 逻辑运算
np.unique(temp) # 去重
stock_change > 0.5 # 大于0.5就标记为True 
np.all() # 是否全部满足
np.any() # 存在一个满足
np.where() # 三元运算符

# 统计指标函数 
min	max	mean	
median # 中位数	 
var	# 方差
std # 标准差

# 数组间运算
arr1 * arr2

# 矩阵运算
np.matmul(data,weights)
np.dot(data,weights)

# 合并
np.hstack(tup) # column wise
np.vstack(tup) # row wise
np.concatenate(a1,a2,...,axis=0)

```

### Pandas

专门用于数据挖掘的开源python库

```python
# DataFrame属性
data.shape
data.index
data.columns
data.values
data.T 

# MultiIndex 多级分层索引对象
df.index,names
df.index.levels

# Panel
pd.Panel(data=None,items=None,major_axis=None,minor_axis=None,copy=False,dtype=None)

# Series 带索引的一维数组

# 索引 先列后行

# 排序
df.sort_values(key=,ascending=)
```

> 缺失值处理

1）删除含有缺失值的样本

2）替换/插补

```python
import numpy as np
np.any(pd.isnull(movie)) # 返回True，说明数据中存在缺失值
np.all(pd.notnull(movie)) # 返回False，说明数据中存在缺失值


pd.concat()
pd.merge()

```





