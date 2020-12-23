# Python



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
mkvirtualenv envname

# 配置环境安装为指定目录
# 编辑系统变量 WORKON_HOME
# 列出虚拟环境列表
workon 
# 新建虚拟环境
mkvirtualenv [虚拟环境名称] 
# 启动/切换虚拟环境
workon [虚拟环境名称]
# 离开虚拟环境
deactivate
```

## 2.fastapi使用说明

### 2.1 [官网地址](https://fastapi.tiangolo.com/project-generation/)

轻量级webapi 使用python编写：快速，直观，简易

- 安装fastapi、uvicorn

- 创建 main.py 入口文件

- 运行 uvicorn main:app --reload

- 查看api：http://127.0.0.1:8000/docs 、 http://127.0.0.1:8000/redoc

- 详细代码编写参照官网案例

### 2.2 持久层选型
- 使用SQLALchemy 连接MySql数据库 [案例参考](https://www.jianshu.com/p/aaadf6e7d688)

- sqlalchemy表设计[参考文档](https://zhuanlan.zhihu.com/p/270623816)



### 2.3 sqlalchemy通用说明

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

  