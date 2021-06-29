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
#enumerate 应用在list中
my_list = ['apple', 'banana', 'grapes', 'pear']
for c, value in enumerate(my_list, 1):
    print(c, value)
# dict循环
knights = {'gallahad': 'the pure', 'robin': 'the brave'}
for k, v in knights.items():
	print(k, v)
# 循环多个序列 zip
questions = ['name', 'quest', 'favorite color']
answers = ['lancelot', 'the holy grail', 'blue']
for q, a in zip(questions, answers):
	print('What is your {0}?  It is {1}.'.format(q, a))
    
# 输出:
(1, 'apple')
(2, 'banana')
(3, 'grapes')
(4, 'pear')

# dir 对象自省， 返回该对象的所有属性

# str.format 字符串格式化
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

