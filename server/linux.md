# linux操作系统

```ini
时间:2020年12月21日13:40:49
编写:王晨
```



## 1. 版本介绍





## 2. 基本操作

- 进程操作

```shell
# 查看进程
ps -ef | grep xxx
# kill进程
kill-9 PID
# 批量kill uvicorn进程
ps -ef |grep uvicorn |awk '{print $2}'|xargs kill -9
# 查看端口占用情况
lsof -i:端口号
```

- 文件操作

```shell
# 删除目录
rm -rf .git/
```



## 3. 实操问题

### 3.1 部署前端项目
-  **复制工程打包文件到服务器**

   ```shell
   mkdir wangchen 
   # 上传工程文件至服务器指定位置 build
   scp -r build wangchen@10.50.63.63:/home/wangchen
   
   ```

- 安装nginx

   ```shell
   # 安装 nginx
   sudo apt-get install nginx
   # 在nginx配置文件夹中新增配置文件 
   sudo vim /ect/nginx/sites-enabled/webconf
   # 增加server配置 
   server{
   	listen 8064;
   	charset utf-8;
   	location / {
   		root 指定的build文件夹所在根目录
   		index index.html index.htm;
   		try_files $uri $uri/ /index.html;
   	}
   }
   
   # 启动 nginx 
   sudo service nginx start
   # 重启 nginx 
   sudo service nginx reload
   # 关闭 nginx
   sudo service nginx stop
   
   ```


### 3.2 部署fastapi项目

- 拷贝python代码到服务器

  ```shell
  uvicorn main:app --reload
  uvicorn main:app --host '0.0.0.0' --port 8065 --reload
  ```

- 使用nohup遇到的问题？

  - nohup: ignoring input and appending output to 'nohup.out'

    ```shell
    # 方法一 丢到dev/null 不需要输出日志 风险高需要工程内部处理日志
    nohup command >/dev/null 2>&1 &
    # 后台运行
nohup uvicorn main:app --host '0.0.0.0' --port 8065 >/dev/null 2>&1 &
    ```
    
    