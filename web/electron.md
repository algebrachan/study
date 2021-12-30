## Electron

### 1.入门教程

> 初次使用

```shell
mkdir demo 
cd demo
npm init
# 修改 script "start": "electron ."
# 创建 main.js index.html

npm install electron --save-dev # 遇到electron版本问题，故采用 cnpm
# 启动
npm start

# 打包 

# 该 electron-forge 方案存在问题
cnpm install --save-dev @electron-forge/cli # 遇到安装问题， 升级node版本
npx electron-forge import # 将 Electron Forge 添加到您应用的开发依赖中

# 使用 electron-packager
cnpm install electron-packager -g
electron-packager .

```



