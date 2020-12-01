# VSCode工具

### 使用过程存在的问题
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
  
  ​	