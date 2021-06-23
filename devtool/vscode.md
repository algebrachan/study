# VSCode工具

### 1. 使用过程存在的问题
- 内置终端没有管理员权限
	```shell
	Set-ExecutionPolicy -Scope CurrentUser
	输入 RemoteSigned
	```

- vscode中python的代码校验功能，pylint总是提示错误，实际上能运行

  ​	在设置的settings.json中添加代码：

  ```json
  // 配置 pylint 位置 本地路径
  "python.linting.pylintArgs":["C:\\Users\\wangchen\\AppData\\Roaming\\Python\\Python38\\Scripts\\pylint.exe"],

  ```
  
- 如何使用远程开发

  下载远程开发插件，使用ssh连接服务器

- python import校验出错，找不到包

  ```json
  // 在settings.json中添加
  "python.analysis.extraPaths": [
          "D:\\Program_Files\\python38\\Lib\\site-packages",　　　　　
          "D:\\codeworkplace\\python\\WorkEnvs\\fastapi\\Lib\\site-packages"
      ],
  ```

  