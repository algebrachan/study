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
使用SQLALchemy 连接MySql数据库 [案例参考](https://www.jianshu.com/p/aaadf6e7d688)