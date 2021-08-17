# Python

## 0.常见问题及解决方案

- pip报No module named 'pip’

  当在执行pip更新时，出现失败或警告，再次执行pip命令报错时，分别按顺序执行以下2条命令即可完成修复。
  `python -m ensurepip`
  `python -m pip install --upgrade pip`

## 1.python 配置虚拟环境

### 1.1为什么需要虚拟环境？

Python的包管理，版本之间不兼容，所以每个项目都需要独特的环境来进行编译，故而引入虚拟环境

- 解决pip下载速度慢问题

```shell
# pip配置国内镜像 
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
pip config set install.trusted-host mirrors.aliyun.com
```


​	

### 1.2虚拟环境配置

```shell
# 安装python 
# 安装虚拟环境
pip install virtualenv
virtualenv envname
# 安装 虚拟环境配置
pip install virtualenvwrapper-win
# 新的方法新建虚拟环境
mkvirtualenv -p python3 envname

# 配置环境安装为指定目录
# 编辑系统变量 WORKON_HOME
# 列出虚拟环境列表
workon 
# 新建虚拟环境
mkvirtualenv [虚拟环境名称] 
# 删除虚拟环境
rmvirtualenv [虚拟环境名称]
# 启动/切换虚拟环境
workon [虚拟环境名称]
# 离开虚拟环境
deactivate
```

### 1.3 安装包

```shell
# 启动虚拟环境

# 在安装包的时候 使用如下 形式安装

pip install fastapi[all] 

pip freeze > requirements.txt

# 当 clone一个项目的时候 只需安装 配置文件中的包

pip install -r requirements.txt
```

## 2.python基本语法

- [python3中文手册](https://docs.pythontab.com/python/python3.5/appetite.html)

### 2.1 逻辑语言

```python
# 异常处理语法
try:
    ...
    pass
except Exception as e:
    pass

finally:
    pass
# 三目运算
a=(x if (x>y) else y)
# 逻辑语言
or #逻辑或
not #逻辑非
and #逻辑与

# 全局变量
global 
# lambda表达式
lambda 参数:操作(参数)
```

### 2.2 基本语法

```python
# 基本语法
any(data) # 判断一个data是否为空 数据存在 返回true
# *args 不定量的参数
# **kwargs 不定量的键值对
# 使用顺序
some_func(fargs, *args, **kwargs)

# map的使用
items = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, items))

# filter的使用
number_list = range(-5, 5)
less_than_zero = filter(lambda x: x < 0, number_list)

# reduce的使用
from functools import reduce
product = reduce( (lambda x, y: x * y), [1, 2, 3, 4] )

# Output: 24
# __slots__来告诉Python不要使用字典，而且只给一个固定集合的属性分配空间
class MyClass(object):
    __slots__ = ['name', 'identifier'] # 节约内存空间
    def __init__(self, name, identifier):
        self.name = name
        self.identifier = identifier
        self.set_up()

# collections defaultdict 与dict不同，不需要检查key是否存在
from collections import defaultdict

colours = (
    ('Yasoob', 'Yellow'),
    ('Ali', 'Blue'),
    ('Arham', 'Green'),
    ('Ali', 'Black'),
    ('Yasoob', 'Red'),
    ('Ahmed', 'Silver'),
)

favourite_colours = defaultdict(list)

for name, colour in colours:
    favourite_colours[name].append(colour)
# 多个key嵌套
import collections
tree = lambda: collections.defaultdict(tree)
some_dict = tree()
some_dict['colours']['favourite'] = "yellow"

# 循环技巧
# enumerate 应用在list中
# c为序号，从1开始
my_list = ['apple', 'banana', 'grapes', 'pear']
for c, value in enumerate(my_list, 1):
    print(c, value)
# 输出:
(1, 'apple')
(2, 'banana')
(3, 'grapes')
(4, 'pear')    

# dict循环
knights = {'gallahad': 'the pure', 'robin': 'the brave'}
for k, v in knights.items():
	print(k, v)
    
    
# 循环多个序列 zip 
# 1对1循环
questions = ['name', 'quest', 'favorite color']
answers = ['lancelot', 'the holy grail', 'blue']
for q, a in zip(questions, answers):
	print('What is your {0}?  It is {1}.'.format(q, a))
    
# dir 对象自省， 返回该对象的所有属性

# str.format 字符串格式化
# 建议使用 f'{}'
print('{0} and {1}'.format('spam', 'eggs'))
spam and eggs

```

- 其他说明：为了让 Python 将目录当做内容包，目录中必须包含 `__init__.py` 文件



### 2.3 调试

```python
python -m pdb my_script.py
# c: 继续执行
# w: 显示当前正在执行的代码行的上下文信息
# a: 打印当前函数的参数列表
# s: 执行当前代码行，并停在第一个能停的地方（相当于单步进入）
# n: 继续执行到当前函数的下一行，或者当前行直接返回（单步跳过）
```

### 2.4 装饰者

```python
from functools import wraps
def decorator_name(f):
    @wraps(f)
    def decorated(*args, **kwargs):
        if not can_run:
            return "Function will not run"
        return f(*args, **kwargs)
    return decorated

# 装饰者 在func函数之前再进行一步操作，常见应用场景，拦截器
@decorator_name
def func():
    return("Function is running")

can_run = True
print(func())
# Output: Function is running

can_run = False
print(func())
# Output: Function will not run
```

### 2.5 数据结构

```python
# 列表推导式 中尽量少用 for循环
squares = list(map(lambda x: x**2, range(10)))
# 等价于
squares = [x**2 for x in range(10)]
# 转置
list(zip(*matrix))
```

### 2.6 语法糖

- 字符串前加字母

```python
# 字符串前加 r
# 1.r'' 的作用是去除转义字符.
# 2.正则表达式 
str = r'input\n'

# 字符串前加 f
# 以 f开头表示在字符串内支持大括号内的python 表达式
f'{name} done in {time.time() - t0:.2f} s'

# 字符串前加 b
# b'' 表示字符串是byte类型
str.encode('utf-8')
bytes.decode('utf-8')

# 字符串前加 u
# 表示有中文的字符串 防止乱码


```



## 3.fastapi使用说明

### 3.1 [官网地址](https://fastapi.tiangolo.com/project-generation/)

轻量级webapi 使用python编写：快速，直观，简易

- 安装fastapi、uvicorn

- 创建 main.py 入口文件

- 运行 uvicorn main:app --reload

- 查看api：http://127.0.0.1:8000/docs 、 http://127.0.0.1:8000/redoc

- 详细代码编写参照官网案例

### 3.2 持久层选型
- 使用SQLALchemy 连接MySql数据库 [案例参考](https://www.jianshu.com/p/aaadf6e7d688)

- sqlalchemy表设计[参考文档](https://zhuanlan.zhihu.com/p/270623816)



### 3.3 sqlalchemy通用说明

- 参考链接：https://blog.csdn.net/qq_29730101/article/details/81660999

- 常用字段类型

  | 类型名       | python中类型      | 说明             |
  | ------------ | ----------------- | ---------------- |
  | Integer      | int               | 普通整数         |
  | SmallInteger | int               | 范围小的整数     |
  | BigInteger   | int/long          | 不限制精度的整数 |
  | Float        | float             | 浮点数           |
  | Numeric      | decimal.Decimal   | 可设置精度的数   |
  | String       | str               | 变长字符串       |
  | Text         | str               | 较长字符串       |
  | Boolean      | bool              | 布尔值           |
  | Date         | datetime.date     |                  |
  | Time         | datetime.datetime |                  |
  | LargeBinary  | str               |                  |
  | Enum         |                   | Enum("男","女")  |
  | JSON         |                   |                  |
  | ARRAY        |                   |                  |
  | DECIMAL      |                   | 保留小数位精度   |

- 常用的列选项

  | 选项名        | 说明                       |
  | ------------- | -------------------------- |
  | primary_key   | 主键                       |
  | unique        | 是否唯一                   |
  | index         | 是否创建索引               |
  | nullable      | 是否为空                   |
  | default       | 默认值                     |
  | autoincrement | 是否自增长                 |
  | onupdate      | 更新的时间执行的函数       |
  | name          | 该属性在数据库中的字段映射 |
  |               |                            |

- 聚合函数

  - func.count：统计行的数量
  - func.avg：平均值
  - func.max：最大值
  - func.min：最小值
  - func.sum：求和

  ```python
  from sqlalchemy import create_engine
  from  sqlalchemy.ext.declarative import declarative_base
  from  sqlalchemy import Column,INTEGER,String,DECIMAL,Boolean,Enum,DateTime
  from sqlalchemy.dialects.mysql import LONGTEXT
  from sqlalchemy.orm import sessionmaker
  from sqlalchemy import func
  from datetime import datetime
  username='root'
  passwd='root'
  port=3306
  ip_dz='127.0.0.1'
  database='aaa'
  
  db_url="mysql+pymysql://{}:{}@{}:{}/{}".format(username,passwd,ip_dz,port,database)
  engine=create_engine(db_url)
  
  Base=declarative_base(engine)
  class Aaa(Base):
      __tablename__='class1'
      id=Column(INTEGER,autoincrement=True,primary_key=True,nullable=False)
      name=Column(String(10),nullable=False)
      float_uid=Column(DECIMAL(10,5))
      is_delete=Column(Boolean)
      # gender=Column(Enum("男","女"))
      # time=Column(DateTime)
      # longtext=Column(LONGTEXT)
  Base.metadata.drop_all()
  Base.metadata.create_all()
  
  Session=sessionmaker(bind=engine)
  session=Session()
  for i in range(10):
      user=Aaa(name="user{}".format(i+1),float_uid=i+1,is_delete=True)
      session.add(user)
  # user01=Aaa(name='user01',uid=2.232,is_delete=True,gender="男",time=datetime(2020,10,31,17,38,30),longtext="xxxxxxxxxxx")
  
  session.commit()
  
  # data=session.query(func.count(Aaa.name)).all()
  .all()  
  .first()
  # print(data)
  
  ```

- 过滤条件

  ```python
  # ==  / !=
  data=session.query(Aaa).filter(Aaa.name=="user1").all()
  for i in data:
      print(i)
  # 模糊查询 like
  data=session.query(Aaa).filter(Aaa.name.like('%us%')).all()
  # in 
  data=session.query(Aaa).filter(Aaa.name.in_(['user1','user02'])).all()
  # not in 
  data=session.query(Aaa).filter(~Aaa.name.in_(['user1','user02'])).all()
  # is null / is not null
  data=session.query(Aaa).filter(Aaa.name!=None).all()
  # and 需要导入 / or 需要导入
  from sqlalchemy import and_ / or_
  data=session.query(Aaa).filter(and_(Aaa.name!=None,Aaa.name=="user1")).all()
  data=session.query(or_(Aaa.name=="user1",Aaa.name=="user2")).all()
  
  #order_by降序并取第一条数据
  query.order_by(desc(Usser_ID)).first()
  ```

### 3.4 websocket

websocket应用于长连接场景，减少资源的调用

- fastapi内集成websocket用法

  ```python
  from fastapi import FastAPI, WebSocket, WebSocketDisconnect
  
  app = FastAPI()
  class ConnectionManager:
      def __init__(self):
          self.active_connections: List[WebSocket] = []
  
      async def connect(self, websocket: WebSocket):
          await websocket.accept()
          self.active_connections.append(websocket)
  
      def disconnect(self, websocket: WebSocket):
          self.active_connections.remove(websocket)
  
      async def send_personal_message(self, message: str, websocket: WebSocket):
          await websocket.send_text(message)
  
      async def broadcast(self, message: str):
          for connection in self.active_connections:
              await connection.send_text(message)
  
  manager = ConnectionManager()
  
  @app.websocket("/ws")
  async def websocket_endpoint(websocket: WebSocket):
      await manager.connect(websocket)
      print('ws连接')
      try:
          while True:
              data = await websocket.receive_text()
              await manager.broadcast(f"Client #1 says: {data}")
      except WebSocketDisconnect:
          manager.disconnect(websocket)
          await manager.broadcast("Client #1} left the chat")
  ```

  




### 3.x 语法小计

- 通用语法区别

  ```python
  # 异常处理语法
  try:
      ...
      pass
  except Exception as e:
      pass
  
  finally:
      pass
  # 三目运算
  a=(x if (x>y) else y)
  
  ```


### 3.6 fastapi相关问题

- post请求参数验证问题

  https://blog.csdn.net/wgPython/article/details/107525950

## 4. python模块使用说明

### 4.0 python标准库

```python
# 操作系统接口
import os
os.getcwd() # return the current working directory
os.chdir('/../') # change current working directory
os.system('mkdir today') # run the command mkdir in the system shell

# 文件通配符
import glob
glob.glob('*.py') # ['primes.py', 'random.py', 'quote.py']

# 命令行参数
import sys 
print(sys.argv) # ['demo.py', 'one', 'two', 'three']
sys.exit() # 终止脚本

# 字符串正则匹配 冗余 
import re
 re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest') # ['foot', 'fell', 'fastest']
re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')# 'cat in the hat'

# 数字
import math
import random
math.cos(math.pi/4.0)
math.log(1024,2)
random.choice(['apple', 'pear', 'banana']) # 'apple'
random.sample(range(100),10) # sampling without replacement
random.random() # random float
random.randrange(6) # random integer chosen from range(6)

# 互联网访问
from urllib.request import urlopen
import smtplib # 发送邮件
server = smtplib.SMTP('localhost')
server.sendmail('soothsayer@example.org', 'jcaesar@example.org',
"""To: jcaesar@example.org
From: soothsayer@example.org
Beware the Ides of March.
""")
server.quit()

# 时间和日期
from datetime import date
now = date.today()
now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")

# 数据压缩  打包和压缩格式：zlib， gzip， bz2， lzma， zipfile 以及 tarfile
import zlib
s = b'witch which has which witches wrist watch'
t = zlib.compress(s)
zlib.decompress(t)
zlib.crc32(s)

# 性能度量
from timeit import Timer
Timer('具体语法').timeit()

# 质量控制 测试

class TestStatisticalFunctions(unittest.TestCase):
    def test_average(self):
        self.assertEqual(average([20, 30, 70]), 40.0)
        self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
        with self.assertRaises(ZeroDivisionError):
            average([])
        with self.assertRaises(TypeError):
            average(20, 30, 70)
unittest.main() # Calling from the command line invokes all tests

```

- 标准库2

```python
# 输出格式
import reprlib
reprlib.repr()
import pprint # 美化输出
import textwrap # 模块格式化文本段落以适应设定的屏宽:
import locale # 国家信息数据库

# 模板
from string import Template
t = Template('${village}folk send $$10 to $cause.')
t.substitute(village='Nottingham', cause='the ditch fund') # 'Nottinghamfolk send $10 to the ditch fund.'
safe_substitute # 数据不完整不会抛出异常

# 多线程
import threading, zipfile

class AsyncZip(threading.Thread):
    def __init__(self, infile, outfile):
        threading.Thread.__init__(self)
        self.infile = infile
        self.outfile = outfile
    def run(self):
        f = zipfile.ZipFile(self.outfile, 'w', zipfile.ZIP_DEFLATED)
        f.write(self.infile)
        f.close()
        print('Finished background zip of:', self.infile)

background = AsyncZip('mydata.txt', 'myarchive.zip')
background.start()
print('The main program continues to run in foreground.')

background.join()    # Wait for the background task to finish
print('Main program waited until background was done.')

# 弱引用

# 列表工具


```



### 4.1 requests

- ~~报Max retries exceeded with url错误

  ```python
  # 原因http连接太多未关闭
  import requests
  requests.adapters.DEFAULT_RETRIES = 5 # 增加重连次数
  s = requests.session()
  s.keep_alive = False # 链接不保活
  s.get('url')
  ```



### 4.2 logging

- 通用日志输入 控制台和文件格式

  ```python
  import logging  # 日志模块
  from logging.handlers import TimedRotatingFileHandler
  import os
  
  # 设置日志存放路径 可变
  path = '/home/wangchen/log/'
  if(not os.path.exists(path)):
      os.mkdir(path)
  class Logger(object):
      level_relations = {
          'debug': logging.DEBUG,
          'info': logging.INFO,
          'warning': logging.WARNING,
          'error': logging.ERROR,
          'crit': logging.CRITICAL
      }  # 日志级别关系映射
  
      def __init__(self, filename, level='info', when='D', interval=1, backCount=5, fmt='%(asctime)s - %(name)s - %(levelname)s - %(message)s'):
          self.logger = logging.getLogger()
          formaat_str = logging.Formatter(fmt)  # 设置日志格式
          self.logger.setLevel(self.level_relations.get(level))  # 设置日志级别
          sh = logging.StreamHandler()
          sh.setFormatter(formaat_str)
          th = TimedRotatingFileHandler(
              filename=filename, interval=interval, when=when, backupCount=backCount, encoding='utf-8')
          # interval是时间间隔，backupCount是备份文件的个数，如果超过这个个数，就会自动删除，when是间隔的时间单位，单位有以下几种：
          # S 秒
          # M 分
          # H 小时、
          # D 天、
          # W 每星期（interval==0时代表星期一）
          # midnight 每天凌晨
          th.setFormatter(formaat_str)
          self.logger.addHandler(sh)
          self.logger.addHandler(th)
  
  logger = Logger(path+'server.log', when='D').logger
  ```



#### logging

>   `logging`：【标准库】功能齐全且灵活的日志记录模块，可方便地设置输出日志的等级、输出方式、保存路径、日志文件回滚。

- 日志是对软件执行时所发生事件的追踪方式。常用于记录异常的错误信息，并让程序继续执行下去。
- 在最简单的情况下，日志消息被发送到文件或 `sys.stderr`
- 日志设计的好坏决定了问题排查的难易程度，对于日志的评价标准有三个：
    - 覆盖度：是否对程序运行应记录处进行记录，如异常、外部调用、一条调用链路上的入口、出口和路径关键点上等。
    - 清晰度：清晰的表达程序目前的状态，统一风格。
    - 信息量：调用的上下文、外部的返回值，用于查询的关键字等，便于分析信息。
        对于线上系统来说，一般可以通过调整日志级别来控制日志的数量，所以打印日志的代码只要不对阅读造成障碍，基本上都是可以接受的。

#### 结构

- 日志系统可以直接从 Python 配置，也可以从用户配置文件加载，以便自定义日志记录而无需更改应用程序。
- Logger: 日志器，提供程序可用接口
- Handler: 处理器，将 logger 创建的日志发送到合适目的地输出
    - StreamHandler：将日志信息输出到 sys.stdout, sys.stderr 或类文件对象
    - FileHander：将日专信息输出到磁盘文件
    - NullHandler：空操作 handler
- Filter：过滤器，提供更细的控制，控制输出哪条，丢弃哪条
- Formatter：格式化器，决定日志输出格式
- Logger 通过 handler 将日志输出到不同目标位置，可以设置多个处理器输出到不同位置
    每个 handler 可以设置自己的 filter，实现日志过滤
    每个 handler 可以设置自己的 formater, 实现同一条日志 以不同格式输出到不同地方



#### 简单模式

```python
import logging
# filename 指定日志存放文件，level 指定 logging 级别
logging.basicConfig(filename="out.log",		# 指定日志文件名
                    level=logging.INFO,
                    filemode='a',			# 指定日志文件的打开模式
                    format='%(asctime)s - %(filename)s[line:%(lineno)d]  - %(levelname)s - %(message)s)
logging.debug('test')
                    
datefmt: 指定时间格式，同 time.strftime()，默认为%Y-%m-%d %H:%M:%S
level: 设置日志级别，默认为 logging.WARNING
stream: 指定将日志的输出流，可以指定输出到 sys.stderr,sys.stdout 或者文件，默认输出到 sys.stderr，当 stream 和 filename 同时指定时，stream 被忽略
format 参数指定输出的格式和内容：
  %(name)s                 Logger 的名字
  %(levelno)s             数字形式的日志级别
  %(levelname)s           文本形式的日志级别
  %(pathname)s           调用日志输出函数的模块的完整路径名，可能没有
  %(filename)s             调用日志输出函数的模块的文件名
  %(module)s               调用日志输出函数的模块名
  %(funcName)s           调用日志输出函数的函数名
  %(lineno)d                 调用日志输出函数的语句所在的代码行
  %(created)f                当前时间，用 UNIX 标准的表示时间的浮 点数表
  %(relativeCreated)d    输出日志信息时的，自 Logger 创建以 来的毫秒数
  %(asctime)s                字符串形式的当前时间。默认格式是 "年-月-日 时:分:秒,毫秒",'%a, %d %b %Y %H:%M:%S',
  %(thread)d                 线程 ID。可能没有
  %(threadName)s        线程名。可能没有
  %(process)d               进程 ID。可能没有
  %(message)s              用户输出的消息
```

```python
TimedRotatingFileHandler(
    filename, 	# 输出日志文件名的前缀
    [when,		# 字符串，“S”: Seconds, “M”: Minutes, “H”: Hours, “D”: Days, “W”: Week day (0=Monday), “midnight”: Roll over at midnight
     [interval,	# 指等待多少个单位 when 的时间后，Logger 会自动重建文件，当然，这个文件的创建取决于 filename+suffix，若这个文件跟之前的文件有重名，则会自动覆盖掉以前的文件，所以有些情况 suffix 要定义的不能因为 when 而重复。
      [backupCount	# 保留日志个数。默认的 0 是不会自动删除掉日志。若设 10，则在文件的创建过程中库会判断是否有超过这个 10，若超过，则会从最先创建的开始删除。
      ]]])
```

```python
def setup_logger(log_file, name='test', log_level=logging.DEBUG):
    """创建日志器

    Arguments:
        log_file {rstr} -- 日志保存的文件

    Keyword Arguments:
        name {str} -- 日志器的名称 (default: {'test'})
        log_level {int} -- 日志器的级别 (default: {logging.DEBUG})

    Returns:
        logger -- 创建的日志器
    """

    # 创建 logger
    my_logger = logging.getLogger(name)
    my_logger.setLevel(log_level)
    # 创建 handler
    handler_file_size = logging.handlers.RotatingFileHandler(
        filename=log_file, mode='a', maxBytes=1 * 1024 * 1024, backupCount=3, encoding='utf-8')
    # 设置输出格式
    formatter = logging.Formatter(
        # 时间 文件名 文件行号 函数名 日志等级 输出的信息
        fmt="[%(asctime)s %(filename)s:%(lineno)d:%(funcName)s]:%(levelname)s: %(message)s",
        datefmt='%m-%d %H:%M:%S')
    handler_file_size.setFormatter(formatter)
    my_logger.addHandler(handler_file_size)
    # 设置日志器输出到 print 流
    print_stream = logging.StreamHandler()
    print_stream.setFormatter(formatter)
    my_logger.addHandler(print_stream)
    return my_logger
```

#### logger 日志器

- 可代码配置，也可使用 conf 配置文件

```python
logger=logging.getLogger('mylogger')	# 日志器，提供程序可用接口
logger.setlevel(logging.DEBUG)     
# 如果把 logger 的级别设置为 INFO， 那么小于 INFO 级别的日志都不输出(即 debug 的不输出)，大于等于 INFO 级别的日志都输出，
logger.debug('test message') 	# 等级 1，调试，最低，如记录算法中每个循环的中间状态。
logger.info("test message") 	# 等级 2，信息，处理请求或状态变化等日常事务，确认程序按预期运行
logger.warning("test message")  # 等级 3，警告，发生重要的事，但并不是错误，如用户登录密码错误，程序仍按预期进行
logger.error("foobar")      	# 等级 4，错误，程序的某些功能已经不能正常执行。如 IO 操作失败，或连接失败
logger.critical("foobar")   	# 等级 5，危急，表明程序已不能继续执行，如内存、磁盘耗尽，一般较少使用

try:
    open("sklearn.txt","rb")
except (SystemExit,KeyboardInterrupt):
    raise
except Exception:
    logger.error("Faild to open sklearn.txt from logger.error",exc_info = True)
logger.info("Finish")
```

#### 文件配置

```python
import logging
import logging.config

logging.config.fileConfig('logging.conf')

# create logger
logger = logging.getLogger('simpleExample')

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warning('warn message')
logger.error('error message')
logger.critical('critical message')
```

这是 logging.conf 文件：

```
[loggers]
keys=root,simpleExample

[handlers]
keys=consoleHandler

[formatters]
keys=simpleFormatter

[logger_root]
level=DEBUG
handlers=consoleHandler

[logger_simpleExample]
level=DEBUG
handlers=consoleHandler
qualname=simpleExample
propagate=0

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=simpleFormatter
args=(sys.stdout,)

[formatter_simpleFormatter]
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
```

### 4.3 json

  ```python
  import json
  
  data = [ { 'a' : 1, 'b' : 2, 'c' : 3, 'd' : 4, 'e' : 5 } ]
  
  data2 = json.dumps(data) # 对象转 json字符串
  print(data2)
  jsonData = '{"a":1,"b":2,"c":3,"d":4,"e":5}';
  
  text = json.loads(jsonData)# json字符串转 python对象
  print(text)
  ```

  

### 4.4 apscheduler(定时轮询框架)

[参考网址](https://www.sohu.com/a/407444741_658944)

> `APScheduler`：Advanced Python Scheduler：基于 Quartz 的 Python 定时任务框架，轻量而功能强大的进程内任务调度器

- 定制性高（triggers）：时间间隔（Interval）、周期性时间（Date）、cron 形式（Linux crontab）
- 后台存储（job stores）：Memory、SQLAlchemy、Redis、MongoDB 等

组件：

- triggers：**触发器**：每个 job 都有触发器，包含它的调度逻辑

- job stores：**作业存储**：默认在内存中，序列化后可保存到其他地方，如 redis

- executors：**执行器**：处理作业的运行，将 job 中的可调用部分提交给线程或进程池来实现，job 完成后，执行器会通知调度器

  - ThreadPool、ProcessPool

- schedulers：**调度器**：将其他组件绑定在一起，对使用者提供接口通常只有一个调度器 scheduler 在程序中运行。

  - Add、Modify、Remove·

  ```python
  BlockingScheduler	# 阻塞性 调度器
  BackgroundScheduler	# 后台运行
  AsyncIOScheduler	# asyncio
  GeventScheduler		# gevent
  TornadoScheduler	# Tornado
  TwistedScheduler	# Twister
  QTScheduler			# QT
  ```

- 使用

  ```python
  pip install apscheduler
  ```

  ```python
  import time 
  import datetime
  from apscheduler.schedulers.background import BackgroundScheduler
  
  def task_test():
      print(time.strftime('%Y-%m-%d %H:%M:%S',time.localtime()))
  
  if __name__ == '__main__':
      schedular = BackgroundScheduler()   # 后台
      schedular.add_job(task_test,'interval',seconds=2)  # 以 interval 间隔性执行
      
      schedular.start()
  
      while True:
          print(time.time()) 
          time.sleep(5)
  
  def job_func(text):
      print(text)
  
  scheduler = BackgroundScheduler()
  # 在 2017-12-13 时刻运行一次 job_func 方法
  scheduler .add_job(job_func, 'date', run_date=date(2017, 12, 13), args=['text'])
  # 在 2017-12-13 14:00:00 时刻运行一次 job_func 方法
  scheduler .add_job(job_func, 'date', run_date=datetime(2017, 12, 13, 14, 0, 0), args=['text'])
  # 在 2017-12-13 14:00:01 时刻运行一次 job_func 方法
  scheduler .add_job(job_func, 'date', run_date='2017-12-13 14:00:01', args=['text'])
  
  scheduler.start()
  ```

  ```python
  def job_func(text):
      print(text)
  # date，指定时间运行一次
  scheduler.add_job(job_func,
                    'date',
                    run_date=datetime.datetime(2020,1,1,14,0,0)	
                    # run_date='2020-1-1 14:00:00'
                   args=['test'])
  # interval，固定时间
  scheduler.add_job(job_func, 
                    'interval',
                    minutes=2, 
                    start_date='2017-12-13 14:00:01' , 	# datetime or str
                    end_date='2017-12-13 14:00:10')		# datetime or str
  weeks,days,hours,minutes,seconds
  # cron，触发器，特定时间周期触发，与 Linux crontab 格式兼容
  # 在每年 1-3、7-9 月份中的每个星期一、二中的 00:00, 01:00, 02:00 和 03:00 执行 job_func 任务
  scheduler.add_job(job_func, 'cron', month='1-3,7-9',day='0, tue', hour='0-3')
  year 年 int or str 4 位数字
  month 月 int or str	1-12
  day	日	int or str 1-31
  week	周	int or str	1-53
  day_of_week	星期几	int or str 0-6 或 mon,tue,wed,thu,fri,sat,sun
  hour	时	int or str	0-23
  minute	分	int or str	0-59
  second	秒	int or str 0-59
  start_date	最早开始时间（包含）	datetime or str
  end_date	最晚结束时间（包含）	datetime or str
  timezone	指定时区	datetime.tzinfo or str
  分 minute[0-59]时 hour[0-23]日 day[1-31]月 month[1-12]周 week[0-7],0 或代表星期日
  
  大于最小有效值的字段默认为*，小于最小有效值的默认为最小值。
  参数表达式
  * 字段的任一个值
  */a	匹配每递增 a 后的值，如 hour 的*/5，为 0，5，10，15，20
  a/b	从 a 开始，匹配每递增 b 后的值，如 hour 的 2/9，匹配 2,11,20
  a-b	a 到 b 之间的取值，2-5,匹配 2,3,4,5
  a-b/c	a 到 b 间递增 c 的值，包括 a，不一定包括 b,如 1-20/5，匹配 1,6,11,16
  xth y	匹配 y 在当月的第 x 次，如 3rd fri，指当月的第三个周五
  last x	匹配 x 在当月的最后一次，如 last fri，当月的最后一个周五
  last	匹配当月的最后一天
  x,y,z	匹配多个表达式的组合
  ```

- 遇到的问题

  - Timezone offset does not match system offset: 0 != 28800

  - ```python
    # 因为系统时区和代码运行时区不一样导致，需在初始化的时候指定上海的时区
    scheduler = BlockingScheduler(timezone="Asia/Shanghai")
    ```




## 5. redis作为临时内存数据库

- 存储方案，使用hash存储对象，其中运用到json和python对象互转

- 可以事先定义好class对象，使用无参构造

  ```python
  import redis
  import json
  
  # json转化存储方法 hset 传入的是__dict__ 对象
  def hash2obj(obj: object, name: str):
      resp = r.hgetall(name)
      try:
          if any(resp):
              for key in obj:
                  obj[key] = json.loads(resp[key])
      except print(0):
          pass
      return obj
  
  # 对象转json hset 传入的是__dict__ obj
  def obj2hash(obj: object, name: str):
      try:
          for key in obj:
              r.hset(name,key,json.dumps(obj[key]))
          return True
      except print(0):
          pass
      return False
  
  def set_redis(obj, key, sec):
      obj2hash(obj, key)
      r.expire(key, sec)  # 过期时间
  
  ```




## 6. python应用消息队列

### 6.1 rabbitMq

```
pip install pika
```

```python
import pika
import json

class RabbitMq:
    def __init__(self, user, pwd, host, port):
        credentials = pika.PlainCredentials(user, pwd)  # mq用户名和密码
        self.connection = pika.BlockingConnection(
            pika.ConnectionParameters(host=host, port=port, credentials=credentials, heartbeat=0)
        )
        self.channel = self.connection.channel()
        self.channel.basic_qos(prefetch_count=1)

    def __create_queue(self, routing_keys):
        # 创建队列。有就不管，没有就自动创建
        if not isinstance(routing_keys, list):
            routing_keys = [routing_keys]
        for _, v in enumerate(routing_keys):
            self.channel.queue_declare(queue=v)

    '''单向发送消息'''
    def send(self, routing_key, body):
        # 使用默认的交换机发送消息。exchange为空就使用默认的
        self.__create_queue(routing_keys=routing_key)
        msg_props = pika.BasicProperties()
        msg_props.content_type = "application/json"
        if isinstance(body, dict):
            body = json.dumps(body)
        self.channel.basic_publish(exchange='', properties=msg_props, routing_key=routing_key, body=body)

    def received(self, routing_key, fun):
        self.__create_queue(routing_keys=routing_key)
        self.channel.basic_consume(routing_key, fun, True)

    #  传入k-fun，可以实现topic到函数路由功能
    def received_dict(self, fun_dict):
        for i, v in fun_dict.items():
            self.received(routing_key=i, fun=v)

    def consume(self):
        self.channel.start_consuming()

    def close(self):
        self.connection.close()


if __name__ == '__main__':
    mq = RabbitMq(user='furnance', pwd='123456', host='127.0.0.1', port=5672)
    queue = 'test'
    # mq.send(routing_key=queue, body={'test': 'json格式'})

    def callback(ch, method, properties, body):
        # print(ch)
        # print(method)
        # print(properties)
        # print(" [x] Received %r" % (body,))
        print(json.loads(body))
    mq.received(routing_key=queue, fun=callback)
    mq.consume()
```

- 问题：太重

### 6.2 kafka

[参考网址](https://zhuanlan.zhihu.com/p/38330574)

- 



## 7.常见中间件的python使用

### 7.1 redis

- [参考网址](https://www.runoob.com/w3cnote/python-redis-intro.html)
-  `pip install redis`

```python
import redis

pool = redis.ConnectionPool(host='localhost', port=6379,db=0,decode_responses=True)
r=redis.Redis(connection_pool=pool)
r.set('name', 'runoob')  # 设置 name 对应的值
print(r.get('name'))  # 取出键 name 对应的值

# string 操作 
set(name, value, ex=None, px=None, nx=False, xx=False) # ex-过期时间(秒) px-过期(毫秒) nx- 为true只有不存在时set xx-为true只有存在的时候才set
mset(*args, **kwargs) # 批量设置值
r.mset({"k1":"v1", "k2":"v2"})
mget(keys, *args) # 批量获取
getset(name, value) # 设置新值并获取原来的值
getrange(key, start, end) # 截取
setrange(key,offset,value) # 指定字符替换
append(key, value) # 

# hash 操作
hset(name, key, value) # 单个增加或修改
hkeys(name) # 获取hash所有key
hget(name,[keys]) # 获取hash 对应key的值
hsetnx(name,key,value) # key不存在时创建

hmset(name, mapping) # 批量增加 mapping-字典 hmset方法已过期
r.hset("hash2",mapping={"k6": "v6", "k7": "v7"})
hgetall(name) # 获取所有键值 返回一个字典mapping
hlen(name) # 获键值对个数
hvals(name) # 获取所有值
hexists(name,key) # key是否存在
hdel(name,*keys) # 删除指定的键值对
hincrby(name, key, amount=1) # 自增
hincrbyfloat(name, key, amount=1.0) # 浮点数 自增
hscan(name, cursor=0, match=None, count=None) # 分片读取
hscan_iter(name, match=None, count=None)# 分批读取

# list
lpush(name,values) # 添加到列表最左边
lpushx(name,value) # 只有name存在时才添加
lrange(name,start,end) # 取出索引号 s - e (-1为最后一个元素)
rpush(name,value) # 添加到右边
rpushx(name,value)
lset(name,index,value) # 指定索引号修改
lrem(name,value,num) # 要删除的值，num 从前到后删除几个 0为全部
lpop(name) # 获取左边第一个元素 返回并移除
rpop(name)# 右边
ltrim(name, start, end) # 删除索引之外的值
lindex(name, index) # 根据索引号取值
rpoplpush(src, dst) # 移动
blpop(keys, timeout)
brpop(keys, timeout)

# set
sadd(name,values)
scard(name) # 获取个数
smembers(name) # 获取所有成员

#zset
zadd
zcard
r.zrange( name, start, end, desc=False, withscores=False, score_cast_func=float)
zrem(name,values)

# 迭代器
for i in r.hscan_iter("hash1"):
    print(i)
sscan_iter
zscan_iter
```



### 7.2 MongoDB

- [参考网页](https://www.runoob.com/python3/python-mongodb.html)
-  `pip install pymongo`



```python
# 连接
myclient = pymongo.MongoClient("mongodb://admin:123456@10.50.63.63:27017")
dblist = myclient.list_database_names()
mydb = myclient["database"]
mycol = mydb["collection"]

# 插入
mydict = { "name": "RUNOOB", "alexa": "10000", "url": "https://www.runoob.com" }
mycol.insert_one(mydict)

# 查询 0和1 不能同时存在 _id 除外
for x in mycol.find({},{ "_id": 0, "name": 1, "alexa": 1 }):
  print(x)
find_one

# 修改数据
myquery = { "name": { "$regex": "^F" } }
newvalues = { "$set": { "alexa": "123" } }
 
x = mycol.update_many(myquery, newvalues)
update_one()
# 排序
mydoc = mycol.find().sort("alexa")
# 删除
myquery = { "name": "Taobao" }
mycol.delete_one(myquery)
myquery = { "name": {"$regex": "^F"} }
x = mycol.delete_many(myquery)
```



## 8.python数据分析常用模块

### 8.1 numpy

https://zhuanlan.zhihu.com/p/24988491

numpy基础

```python
import numpy as np

array = np.array([[1,2,3],[2,3,4]],dtype=np.int32)
# 属性 array.ndim array.shape array.size

# 零矩阵
a = np.zeros((3,4),dtype=np.int16) # 3行4列的零矩阵
b = np.ones((3,4),dtype=np.int16)  # 3行4列的 1矩阵
c = np.arange(12).reshape((3,4))   # 生成3行4列 0->11 的数列
d = np.linspace(1,10,20)    # 1-10 20个

# 基础运算
a = np.array([10,20,30,40])
b = np.range(4)
c = a -b
c = a * b # 矩阵中对应数量相乘
c_dot = np.dot(a,b) # 矩阵乘法
c_dot_2 = a.dot(b)  # 矩阵乘法第二种形式

# 
a = np.random.random((2,4)) # 随机生成2行4列的矩阵
np.sum(a,axis=1) # axis=1 在一列中输出聚合函数，axis=0在一行中输出聚合函数
np.min(a)
np.max(a)

A = np.arange(2,14).reshape((3,4))
np.argmin(A) # 最小值索引
np.argmax(A) # 最大值索引
np.mean(A) # 平均值
A.mean() # 平均值
np.median(A) # 中位数
np.cumsum(A) # 累加和 list
np.diff(A) # 累差 
np.nonzero(A) # 非零索引
np.transpose(A) # 矩阵转向
A.T
np.clip(A,5,9) # 小于5的数变为5，大于9的数变为9
```

numpy操作

```python
# numpy索引
A = np.arange(3,15).reshape((3,4))
A[1][1] # 索引从0开始

# array合并
A = np.array([1,1,1])[:,np.newaxis]
B = np.array([2,2,2])[:,np.newaxis]

C = np.concatenate((A,B,B,A),aixs=1)
# array 分割
np.split(A,2,axis=1) # 按列分为两块
np.array_split(A,3,axis=1) # 不等项 分割
np.vsplit(A,3) # 横向3块分割
np.hsplit(A,2) # 纵向2块分割

# 赋值之间 内存地址未改变
# 拷贝
b = a.copy() # deep copy
```



### 8.2 pandas

https://zhuanlan.zhihu.com/p/99889912

pandas基础

```python
import pandas as pd
import numpy as np
# 创建Series
s = pd.Series([1,3,6,np.nan,44,1])
dates = pd.date_range('20200101',periods=6) # DatetimeIndex
df = pd.DateFrame(np.random.randn(6,4),index=dates,columns=['a','b','c','d'])
df['a']
df.a
df[0:3]
df.loc
df.iloc

# 处理丢失数据
df.dropna(axis=0,how='any') # 行 how={'any','all'} 丢弃
df.fillna(value=0) 
df.isnull()

# 导入导出
read_csv
read_excel
read_hdf
read_sql
read_json
read_msgpack
read_html
read_state
read_sas
read_clipboard
read_pickle
 
# concatenating 
import numpy as np
import pandas as pd

df1 = pd.DataFrame(np.ones((3,4))*0,columns=['a','b','c','d']) # np.ones 1矩阵
df2 = pd.DataFrame(np.ones((3,4))*1,columns=['a','b','c','d'])
df3 = pd.DataFrame(np.ones((3,4))*2,columns=['a','b','c','d'])

res = pd.concat([df1,df2,df3],axis=0,ignore_index=True,join='inner',join_axes=[df1.index]) # 横向合并 join类似与mysql的内联查询
print(res)

# merge
res = pd.merge(left,right,on=['key1','key2'])
```



## 9.python爬虫

> 爬虫

通过编写程序，模拟浏览器上网，然后让其去互联网上抓取数据的过程

**爬虫分类**

- 通用爬虫

  抓取系统重要组成部分，抓取的是一整张页面数据

- 聚焦爬虫

  建立在通用爬虫的基础之上，抓取页面中特定的局部内容

- 增量式爬虫

  检测网站中数据更新的情况，只会抓取网站中最新更新出来的数据



> 反爬机制

门户网站，可以通过制定相应的策略或者技术手段，防止爬虫程序进行网站数据的爬取



> 反反爬策略

爬虫程序可以通过制定相关的策略或者技术手段，破解门户网站中具备的反爬机制，从而可以获取门户网站的数据



> robots.txt协议

规定了网站中哪些数据可以被爬虫爬取哪些数据不可以被爬取



### http&https

请求头：

User-Agent: 请求载体的身份标识

Connection：请求完毕后，是断开连接还是保持连接

响应头：

Content-Type：服务器响应的数据类型

加密方式：

对称密钥加密

非对称密钥加密

证书密钥加密

### requests

```python
import requests

url = 'https://www.sogou.com/'
response = requests.get(url=url)
page_text = response.text

with open('./sogou.html','w',encoding='utf-8') as fp:
    fp.write(page_text)
    

# UA伪装：让爬虫对应的请求载体身份标识伪装成某一款浏览器
headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36'
}
```

### 数据解析

- 正则：匹配字符串
- bs4：
- xpath：

```python
# 爬取图片
import requests

url = 'https://...' # 图片url地址
# text(字符串) content (二进制) json() (对象)
img_data = requests.get(url=url).content
with open('./1.jpg','wb') as fp:
    fp.write(img_data)
```



```python
# bs4 环境安装
# pip install bs4
# pip install lxml

page_text = response.text
soup = BeatigulSoup(page_text,'lxml') # html标签
# soup.tagName: 返回的是文档中第一次出现的tagName对应的标签

soup.find('div') 
# 属性定位法
soup.find('div',class_='song')
soup.find_all('a')
soup.select('.tang') # 支持css选择器的写法
soup.select('.tang > ul a')[0].get_text()

```



```python
# xpath 环境安装
# pip install lxml
from lxml import etree

page_text = response.text
# tree = etree.parse(page_text)
tree = etree.HTML('page_text')
r = tree.xpath('/html/body/div')

# etree.HTML('page_text')


# 如果请求过程产生了cookie， 使用session对象
session = requests.Session()

```

### xpath

```python
# 属性定位
xpath('//input[@id="kw"]') # 
xpath('//input[@class="bg s_btn"]')

# 层级定位
xpath('//div[@id="head"]/div/div[2]/a[1] ')

# 索引定位 索引从1开始
xpath('//div[@id="head"]/div/div[2]/a[1]')

# 逻辑运算
xpath('//input[@class="s_ipt" and @name="wd"]')

# 模糊匹配 contains		starts-with
xpath('//input[contains(@class,"s_i")]')
xpath('//input[starts-with(@class,"s")]')

# 取文本
xpath('//div[@id="u1"]/a[5]/text()') # 获取节点内容
xpath('//div[@id="u1"]//text()') # 获取节点里不带标签下所有内容

# 取属性
xpath('//div[@id="u1"]/a[5]/@href') # 获取href属性

```





### 代理

破解封IP这种反爬机制

快代理https://www.kuaidaili.com/

西祠代理

www.goubanjia.com

```python
# 使用代理
import requests


headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36'
}

url = 'https://www.baidu.com/s?ie=UTF-8&wd=ip'

# response = requests.get(url=url,headers=headers)
response = requests.get(url=url,headers=headers,proxies={"http":'118.163.13.200:8080'})
page_text = response.text

with open('./3.html','w',encoding='utf-8') as fp:
    fp.write(page_text)

```



### 异步爬虫

多线程

线程池：降低系统对线程创建销毁的频率

```python
# 单线程 串行
from datetime import datetime
import time

def get_page(str):
  print("正在下载：",str)
  time.sleep(2)
  print("下载成功：",str)

name_list = ["aa","bb","cc","dd"]

start_time = datetime.now()

for i in range(len(name_list)):
  get_page(name_list[i])

end_time = datetime.now()

print(f'{(end_time-start_time).seconds}s') # 8s


# 线程池操作
from datetime import datetime
from multiprocessing.dummy import Pool
import time

start_time = datetime.now()
def get_page(str):
  print("正在下载：",str)
  time.sleep(2)
  print("下载成功：",str)

name_list = ["aa","bb","cc","dd","ee"]
# 创建 4个线程的线程池
pool = Pool(4)

pool.map(get_page,name_list)

end_time = datetime.now()

print(f'{(end_time-start_time).seconds}s') # 4s

```

```python
# 单线程+异步协程
import asyncio

async def request(url):
  print('正在请求的url是',url)
  print('请求成功',url)
  return url

# async修饰的函数，调用之后返回的一个协程对象
c = request('www.baidu.com')

# # 创建一个事件循环对象
# loop = asyncio.get_event_loop()
# # 将协程对象注册到loop中，启动事件循环
# loop.run_until_complete(c)

# # task的使用
# loop = asyncio.get_event_loop()
# # 基于loop创建了一个task对象
# task = loop.create_task(c)
# print(task)
# loop.run_until_complete(task)
# print(task)

# # future使用
# loop = asyncio.get_event_loop()
# task = asyncio.ensure_future(c)
# print(task)
# loop.run_until_complete(task)
# print(task)

def callback_func(task):
  print(task.result())
    
# 绑定回调
loop = asyncio.get_event_loop()
task = asyncio.ensure_future(c)
# 将回调函数绑定到任务对象中
task.add_done_callback(callback_func)
loop.run_until_complete(task)

```

```python
# 异步多任务
import asyncio
from datetime import datetime
import time

async def request(url):
  print('正在下载',url)
  # 在异步协程中，如果出现了同步模块相关的代码，就无法实现异步
  # time.sleep(2)
  # 在asyncio中遇到阻塞操作必须手动挂起
  await asyncio.sleep(2)
  print('下载完毕',url)

start = datetime.now()
urls = [
  'www.baidu.com',
  'www.sogou.com',
  'www.doubanjia.com',
]

# 任务列表: 存放多个任务对象
tasks = []
for url in urls:
  c = request(url)
  task = asyncio.ensure_future(c)
  tasks.append(task)

loop = asyncio.get_event_loop()
# 需要将任务列表封装到wait中
loop.run_until_complete(asyncio.wait(tasks))

end = datetime.now()
print(f'{(end-start).seconds}s') 
```

```python
# 模拟flask服务
from flask import Flask
import time

app = Flask(__name__)

@app.route('/bobo')
def index_bobo():
  time.sleep(2)
  return 'hello bobo'

@app.route('/jay')
def index_javy():
  time.sleep(2)
  return 'hello jay'

@app.route('/tom')
def index_tom():
  time.sleep(2)
  return 'hello tom'

if __name__ == '__main__':
    app.run()
```

```python
# 使用requests模块，是同步的
import asyncio
from datetime import datetime
import requests


start = datetime.now()
urls = [
  'http://127.0.0.1:5000/bobo',
  'http://127.0.0.1:5000/jay',
  'http://127.0.0.1:5000/tom',
]
headers = {
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.131 Safari/537.36'
}
async def get_page(url):
  print('正在下载:',url)
  response = requests.get(url=url,headers=headers)
  print('下载完毕',response.text)

tasks =[]
for url in urls:
  c = get_page(url)
  task = asyncio.ensure_future(c)
  tasks.append(task)

loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(tasks))

end = datetime.now()

print(f'{(end-start).seconds}s')  # 6s


# 使用异步请求模块 aiohttp

import asyncio
from datetime import datetime
import aiohttp

start = datetime.now()
urls = [
  'http://127.0.0.1:5000/bobo',
  'http://127.0.0.1:5000/jay',
  'http://127.0.0.1:5000/tom',
]

async def get_page(url):
  async with aiohttp.ClientSession() as session:
    async with await session.get(url) as response:
      # text() 方法可以返回字符串形式的响应数据
      # read() 返回二进制
      # json() 返回json对象
      # 注意：获取响应数据操作之前一定要使用await进行手动挂起
      page_text = await response.text()
      print(page_text)

tasks =[]
for url in urls:
  c = get_page(url)
  task = asyncio.ensure_future(c)
  tasks.append(task)

loop = asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(tasks))

end = datetime.now()

print(f'{(end-start).seconds}s') 

```

### selenium

便捷的获取网站中动态加载的数据

便捷实现模拟登陆

> selenium模块

基于浏览器自动化的一个模块

```python
# pip install selenium
# 下载浏览器驱动程序
# http://chromedriver.storage.googleapis.com/index.html

from selenium import webdriver
from lxml import etree

# 实例化一个浏览器对象(传入chrome浏览器驱动)
bro = webdriver.Chrome(executable_path='./chromedriver.exe')
# 让浏览器发起一个指定url对应请求
bro.get('https://www.baidu.com/')

# 获取源码
page_text = bro.page_source

tree = etree.HTML(page_text)
a = tree.xpath('//div[@id="s-top-left"]/a/@href')

print('a',a)
```

- 自动化操作

```python
from selenium import webdriver
from time import sleep
# 实例化一个浏览器对象(传入chrome浏览器驱动)
bro = webdriver.Chrome(executable_path='./chromedriver.exe')
# 让浏览器发起一个指定url对应请求
bro.get('https://www.taobao.com/')

# 标签定位
search_input = bro.find_element_by_id('q')
# 标签交互 输入 send_keys
search_input.send_keys('Iphone')

# 执行一组js程序
bro.execute_script('window.scrollTo(0,document.body.scrollHeight)')
sleep(2)
btn = bro.find_element_by_class_name('btn-search')
btn.click()

sleep(1)
bro.get('https://www.baidu.com')
sleep(2)
bro.back() # 后退
sleep(2)
bro.forward() # 前进
sleep(5)
bro.quit() # 退出

# 如果定位的标签是存在于iframe标签之中的则必须通过如下操作进行标签定位
bro.switch_to.frame('iframeResult')

# 实例化动作练
action = ActionChains(bro)
# click_and_hold(div) 长按且点击操作
# move_by_offset(x,y) 移动
# perform() 让动作链立即执行
# action.release() 释放动作链对象
```

- 无头浏览器&规避检测

```python
from selenium import webdriver
from time import sleep
from selenium.webdriver.chrome.options import Options
from selenium.webdriver import ChromeOptions

# 实现可视化界面
chrome_options = Options()
chrome_options.add_argument('--headless')
chrome_options.add_argument('--disable-gpu')

# 规避检测
option = ChromeOptions()
option.add_experimental_option('excludeSwitches',['enable-automation'])

# 实例化一个浏览器对象(传入chrome浏览器驱动)
bro = webdriver.Chrome(executable_path='./chromedriver.exe',chrome_options=chrome_options,options=option)
# 无可视化界面
bro.get('https://www.baidu.com/')
sleep(2)
bro.save_screenshot('baidu.png')

sleep(5)
bro.quit()
```

### scrapy

> scrapy框架

高性能的持久化存储

异步数据下载

高性能数据解析

分布式

```shell
scrapy startproject firstBlood # 创建爬虫项目
scrapy genspider first www.xxx.com # 创建spider源文件
scrapy crawl spiderName # 执行爬虫文件

```

- 数据解析

```python
# settings.py 配置文件
ROBOTSTXT_OBEY = False

LOG_LEVEL = 'ERROR'

# spiders/first.py 数据解析

import scrapy

class FirstSpider(scrapy.Spider):
    # 爬虫文件的名称：就是爬虫源文件的一个唯一标识
    name = 'first'
    # 允许的域名：用来限定start_urls列表中哪些url可以进行请求发送
    # allowed_domains = ['www.baidu.com']

    # 起始的url列表：被scrapy自动发送请求
    start_urls = ['https://www.qiushibaike.com/text/']

    # 用于解析数据
    def parse(self, response):
      # 解析：作者的名称+段子内容
      # print(response)
      div_list = response.xpath('//div[starts-with(@id,"qiushi_tag")]')
      all_data = [] # 存储数据
      for div in div_list:
        # xpath返回的是列表，但是列表元素一定是Selector类型的对象
        # extract可以将Selector对象中data参数存储的字符串提取出来
        author = div.xpath('./div[1]/a[2]/h2/text()')[0].extract()
        content = div.xpath('./a[1]/div[@class="content"]//text()').extract()
        content = ''.join(content)
        dic = {
          'author':author,
          'content':content
        }
        all_data.append(dic)
      return all_data
```

- 持久化存储

  - 命令行，终端指令

     `scrapy crawl first -o ./qiubai.csv`

    ​	中文乱码问题： 在settings中添加  *FEED_EXPORT_ENCODING = 'utf-8-sig'*

  - 基于管道：

    - 数据解析
    - 在item类中定义相关的属性
    - 将解析的数据封装到item类型的对象
    - 将item类型的对象提交给管道进行持久化的操作
    - 在管道类的process_item中要将其接收到的item对象中存储的数据进行持久化存储操作
    - 在配置文件中开启管道

  ```python
  # items.py
  import scrapy
  
  class FirstbloodItem(scrapy.Item):
      # define the fields for your item here like:
      # name = scrapy.Field()
      author = scrapy.Field()
      content = scrapy.Field()
      pass
  
  # pipelines.py
  from itemadapter import ItemAdapter
  
  class FirstbloodPipeline:
      fp = None
      # 重写父类方法 该方法只有在开始爬虫的时候调用一次
      def open_spider(self,spider):
          print('开始爬虫')
          self.fp = open('./first.txt','w',encoding='utf-8')
  
      # 该方法每接到一次item就会被调用一次
      def process_item(self, item, spider):
          author = item['author']
          content = item['content']
          self.fp.write(author+':'+content+'\n')
          return item
  
      def close_spider(self,spider):
          print('结束爬虫')
          self.fp.close()
          
  # settings.py
  ITEM_PIPELINES = {
     'firstBlood.pipelines.FirstbloodPipeline': 300,
  }
  
  # first.py
  import scrapy
  from firstBlood.items import FirstbloodItem
  
  class FirstSpider(scrapy.Spider):
      # 爬虫文件的名称：就是爬虫源文件的一个唯一标识
      name = 'first'
      # 允许的域名：用来限定start_urls列表中哪些url可以进行请求发送
      # allowed_domains = ['www.baidu.com']
  
      # 起始的url列表：被scrapy自动发送请求
      start_urls = ['https://www.qiushibaike.com/text/']
  
      # 用于解析数据
      def parse(self, response):
        # 解析：作者的名称+段子内容
        # print(response)
        div_list = response.xpath('//div[starts-with(@id,"qiushi_tag")]')
        for div in div_list:
          # xpath返回的是列表，但是列表元素一定是Selector类型的对象
          # extract可以将Selector对象中data参数存储的字符串提取出来
          author = div.xpath('./div[1]/a[2]/h2/text()')[0].extract()
          content = div.xpath('./a[1]/div[@class="content"]//text()').extract()
          content = ''.join(content)
  
          item = FirstbloodItem()
          item['author'] = author
          item['content'] = content.strip()
          yield item # 将item提交给管道
  ```

  - 保存数据库管道

  ```python
  # pipelines.py
  from itemadapter import ItemAdapter
  import pymysql
  
  class FirstbloodPipeline:
      fp = None
      # 重写父类方法 该方法只有在开始爬虫的时候调用一次
      def open_spider(self,spider):
          print('开始爬虫')
          self.fp = open('./first.txt','w',encoding='utf-8')
  
      # 该方法每接到一次item就会被调用一次
      def process_item(self, item, spider):
          author = item['author']
          content = item['content']
          self.fp.write(author+':'+content+'\n')
          return item  # 就会执行给下一个管道
  
      def close_spider(self,spider):
          print('结束爬虫')
          self.fp.close()
  
  # 管道文件中一个管道类对应将一组数据存储到一个平台或载体中
  class mysqlPipeLine:
      conn = None
      cursor = None
      def open_spider(self,spider):
          self.conn = pymysql.Connect(host='127.0.0.1',port=3306,user='root',password='123456',db='scrapydb',charset='utf8')
  
      def process_item(self, item, spider):
          self.cursor = self.conn.cursor()
          try:
              sql = f'insert into qiubai (author,content) values("{item["author"]}","{item["content"]}")'
              self.cursor.execute(sql)
              self.conn.commit()
          except Exception as e:
              print(e)
              self.conn.rollback()
  
          return item 
  
      def close_spider(self,spider):
          self.cursor.close()
          self.conn.close()
  
  # settings.py
  ITEM_PIPELINES = {
     'firstBlood.pipelines.FirstbloodPipeline': 300,
     'firstBlood.pipelines.mysqlPipeLine': 400,
  }
  ```

  

  