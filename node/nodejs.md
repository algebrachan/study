## nodejs

```ini
说明:部分内容与web中的JavaScript重合，固重复的常见的知识不再记录
```



### 基础知识

运行命令 `node xxx.js`

node中没有DOM和BOM

```javascript
// fs 是 file-system的简写 文件系统
var fs = require('fs')
// 读文件
fs.readFile('./hello.txt',(error,data)=>{
    console.log(data.toString())
})
// 写文件
fs.writeFile('./hello.md', 'this is write md', (error) => {
  console.log('成功')
})

var http = require('http')

var server = http.createServer()

server.on('request', (request, response) => {

  console.log('收到客户端的请求了,请求路径是' + request.url)
  var url = request.url
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

server.listen(3000, () => {
  console.log('服务器启动成功，通过http://127.0.0.1:3000/ 来访问')
})

```

> 核心模块 需要 require('xx') 来引入

- fs
- http
- path
- os



