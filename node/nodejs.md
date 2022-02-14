## nodejs

```ini
说明:部分内容与web中的JavaScript重合，固重复的常见的知识不再记录
```



### 基础知识

运行命令 `node xxx.js`

node中没有DOM和BOM

```javascript
// fs 是 file-system的简写 文件系统
const fs = require('fs')
// 读文件
fs.readFile('./hello.txt','utf8',(error,dataStr)=>{
    console.log(dataStr)
})
// 写文件
fs.writeFile('./hello.md', 'this is write md', (error) => {
  console.log('成功')
})
console.log(__dirname) // 表示当前文件所处的目录

const path = require('path')
const pathStr = path.join(__dirname,'/1.txt')
path.basename(pathStr) // path.basename() 获取名称部分


const http = require('http')

const server = http.createServer()
server.on('request', (request, response) => {

  console.log('收到客户端的请求了,请求路径是' + request.url)
  var url = request.url
  response.setHeader('Content-Type','text/html;charset=utf-8') // 防止乱码
  if (url === '/wc') {
    response.end('this is wc')
  } else if (url === '/products') {
    let products = [
      {
        name: '苹果',
        price: 230,
      },
      {
        name: '小辣椒',
        price: 212,
      },
    ]
    response.end(JSON.stringify(products))
  }
  else {
    response.end('null data')
  }
})

server.listen(3005, () => {
  console.log('服务器启动成功，通过http://127.0.0.1:3005/ 来访问')
})

```

> 核心模块 需要 require('xx') 来引入

- fs
- http
- path
- os

### 模块化

编程领域中的模块化，就是遵守固定的规则，把一个大文件拆成独立并互相依赖的多个小模块

- 提高了代码的复用性
- 提高了代码的可维护性
- 可以实现按需加载

> 模块分类

内置模块、自定义模块、第三方模块



> 模块加载

```javascript
// 1.加载内置的 fs 模块
const fs = require('fs')

// 2.加载用户的自定义模块 省略.js也可加载
const custom = require('./custom.js')

// 3.加载第三方模块
const moment = require('moment')

// 注意： 使用require() 方法加载其他模块时，会执行被加载模块中的代码

```

> 模块作用域

防止全局变量污染问题

module对象，存储和当前模块有关的信息

```shell
PS F:\wc\test\jstest\node\node-demo> node .\test.js
Module {
  id: '.',
  path: 'F:\\wc\\test\\jstest\\node\\node-demo',
  exports: {}, # 共享成员
  filename: 'F:\\wc\\test\\jstest\\node\\node-demo\\test.js',
  loaded: false,
  children: [],
  paths: [
    'F:\\wc\\test\\jstest\\node\\node-demo\\node_modules',
    'F:\\wc\\test\\jstest\\node\\node_modules',
    'F:\\wc\\test\\jstest\\node_modules',
    'F:\\wc\\test\\node_modules',
    'F:\\wc\\node_modules',
    'F:\\node_modules'
  ]
}
```

module.exports对象，将模块内的成员共享出去，供外界使用 等于 exports对象，有多个指向，以最后一个module.exports对象为准

外界使用require方法导入，得到导出的共享对象

```js
module.exports = {
    nickname:'wc',
    age:10,
}
```

> npm

包管理工具 

网址：https://www.npmjs.com/

`npm install 包的名称@指定版本号`

```js
// 例子
const moment = require('moment')

const dt = moment().format('YYYY-MM-DD HH:mm:ss')

console.log(dt) // 打印时间

// 版本说明		大版本.功能版本.Bug修复版本
```

> 包管理配置文件 package.json

```shell
npm init -y # 初始化 package.json
# 必须包含 name，version，main(入口)


# 有些包只在开发过程用到 则记录到 devDependencies 节点中
npm i 包名 -D
npm install 包名 --save-dev


# 有些包 开发和上线都用到 记录到 dependencies 节点中

# 切换镜像源
npm config get registry
npm config set registry=https://registry.npm.taobao.org/

# nrm 镜像源切换工具
npm i nrm -g
nrm ls
nrm use taobao

# i5ting_toc 实现 md 文件转化html
npm install -g i5ting_toc
i5ting_toc -f 文件 -o
```

项目包 C:\Users\wangchen\AppData\Roaming\npm\node_modules 文件夹中 

全局包 npm中



> 模块加载机制

模块在第一次加载后会被缓存，这意味着多次调用require不会导致模块的代码被执行多次



### express

> nodemon

npm install -g nodemon

代码被修改自动重启

> 路由

从上到下依次匹配

```js
const express = require('express')
const app = express()

app.get('/',(req,res)=>{res.send('Hello World')})
app.post('/',(req,res)=>{res.send('Post Request.')})

app.listen(3500,()=>{console.log('server running at http://127.0.0.1:3500')})

// 路由模块化
const express = require('express')
const router = express.Router()

router.get('/test/:id', (req, res, next) => {
  req.params
  res.send(req.params)
})

router.get('/t', (req, res, next) => {
  req.query
  res.send(req.query)
})
module.exports = router;

const routes = require('./routes/index');
app.use('/wc', routes);

const qs = require('querystring')
const body = qs.parse(str) // 解析字符串

```

> 中间件

next函数，是实现多个中间件连续调用的关键，表示把流转关系转交给下一个中间件或路由

app.use() 全局生效中间件

app.get('',(req,res,next)=>{next()}) 局部生效中间件

连续调用多个中间件时，多个中间件之间共享 res，req

- 应用级别中间件
- 路由级别中间件
- 错误级别中间件: 捕获整个项目的异常 ，必须注册在所有路由之后
- Express内置中间件
  - express.static
  - express.json 解析json格式，4.16.0+版本
  - express.urlencoded 解析 URL-encoded 格式

```js
// 错误级别中间件
app.use((err,req,res,next)=>{res.send('error'+err.message)})

app.use(express.json())
app.use(express.urlencoded({extended:false}))

// session 中间件
const session = require('express-session')

app.use(session({
    secret:'keyboard cat',
    resave:false,
    saveUninitialized:true,
}))
// 配置成功后 在req.session来访问session对象
req.session.user

req.session.destroy()// 清空session信息
```

> CORS 跨域资源共享

npm install cors 

```js
const cors = require('cors')
app.use(cors())
```

> 参数校验

`npm install @hapi/joi`

`npm i @escook/express-joi`

### mysql

node项目中使用mysql

```js
/**
 * mysql 直接使用 操作sql的形式 来操作数据库，故使用的时候还需要对其进行封装
 */
const mysql = require('mysql')

const db = mysql.createPool({
  host: '127.0.0.1',
  user: 'root',
  password: '123456',
  database: 'demo'
})

// sql 语句
const sqlStr = 'select * from course'
db.query(sqlStr, (err, results) => {
  if (err) return console.log(err.message)
  console.log(results)
})

```



### 身份认证

> session验证,	适合于服务端渲染SSR

cookie特性：

- 自动发送
- 域名独立
- 过期时限
- 4kb 限制

不能使用cookie存储重要且私密的数据，比如用户身份信息、密码

session跨域会存在问题

> JWT认证 

在服务器端，将用户信息加密，生成token，发送客户端。通过请求头 **Authorization**字段发送给服务器端，解密出用户信息

JWT组成部分： 头部. 有效荷载.签名

Header.**Payload**.Signature

Payload是真正的用户信息

客户端 将JWT存储在localStorage或者**sessionStorage**中

```js
// npm install jsonwebtoken express-jwt

const jwt = require('jsonwebtoken')
const expressJWT = require('express-jwt')

const secretKey = 'itheima' // 密钥

const  tokenStr = jwt.sign({username:userinfo.username},secretKey,{expiresIn:'30s'}) // 生成token

// 解析 api开头的不需要访问权限
app.use(expressJWT({secret:secretKey}).unless({path:[/^\/api\//]}))

req.user // 获取解析出来的对象

// 请求头Authorization 添加Bearer token字符串 
```

### 项目相关

#### 遇到问题

- mongoose中Schema数据类型定义问题，如何定义double类型数据？

  Number类型包含int和double

[mongoose中间件](https://mongoosejs.com/docs/index.html)