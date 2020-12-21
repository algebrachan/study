# linux操作系统

```ini
时间:2020年12月21日13:40:49
编写:王晨
```



## 1. 版本介绍





## 2. 基本操作





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