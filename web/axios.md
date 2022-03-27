## Axios

[npm地址](https://www.npmjs.com/package/axios)

[github地址](https://github.com/axios/axios)



Promise based HTTP client for the browers and node.js

> axios作用

- 在浏览器端实现XMLHttpRequests（ajax）请求
- 在nodejs中实现http请求
- 支持Promise的api
- 请求响应可以做拦截
- 对请求和响应数据做转换
- 取消请求
- 自动转化结果为JSON
- 做保护，阻挡XSRF攻击



`npm install axios --save`

### Axios基本使用

```javascript
import axios from 'axios';

// 方式一 创建实例对象
axios({
    method:'post', // or 'get'
    url:'/xxx',
    data:{
        name:'wc'
    },
    headers:{
        'Content-Type':'application/json'
    }
}).then(res=>
    console.log(res) // 请求成功响应   
  ).catch(err=>
    console.log(err) // 请求失败响应
  );

// 方式二 使用axios对象的方法 request、post、get
axios.request({
    method:'get',
    url:'/xxx'
}).then(res=>console.log(res))

axios.post(
	'/xxx',
    {
        "name":"wc"
    }).then(res=>console.log(res))

```

> axios请求响应结果的结构

- config：请求相关内容在该处保存
- data: 响应数据data对象,axios自动对结果做了自动json解析
- headers: 响应头信息
- request：原生XMLHttpRequest对象
- status：响应状态200
- statusText："OK"

> axios配置对象

```javascript
{
    url:'/xxx',
    method:'get',
    baseURL:'https://some-domain.com/api', // 将baseURL和url做拼接请求
    transformRequest:[function(data,headers){
        return data; // 对请求数据进行处理再发送
    }],
    transformResponse:[function(data){
        return data; // 对响应数据进行处理
    }]，
    headers:{
        '':'', // 对请求头信息做控制
    },
    params:{
        id:1234 // get请求参数
    },
    data:{},// 请求体内容
    auth:{
        username:'',
        password:''
    },
    proxy:{ // 代理
        protocol:'https',
        host:'127.0.0.1',
        port:9000,
        auth:{}
    }
}
// 默认配置
axios.defaults.method = 'GET'
axios.defaults.timeout = 3000
axios.defaults.headers.post['Content-Type'] = 'application/json'
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN

```

> axios 创建实例对象发送请求

```javascript
import axios from 'axios'
const instance = axios.create({
    baseURL:'https://www.xxx.com',
    timeout:3000,
})
```

> Interceptors

```javascript
axios.interceptors.request.use(function(config){ // 请求拦截器
    return config;
},function(error){
    return Promise.reject(error);
})
axios.interceptors.response.use(function(config){
    return config;
},function(error){
    return Promise.reject(error);
})
// 请求拦截器，后进先执行
// 响应拦截器，先进先执行
```

> 取消请求

```javascript
let cancel = null 

btn1.onclick = function(){
    cancel()
}

btn2.onclick = function(){
    if(cancel !== null){
        cancel();
    }
    axios({
        method:'get',
        url:'http://localhost:3000/posts',
        cancelToken:new axios.CancelToken(function(c){
            cancel = c;
        })
    }).then(
        res=>console.log(res)
        cancel = null
    )
}

```

