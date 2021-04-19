## 尚硅谷react技术全家桶

### 1. react相关

- [官网](https://reactjs.org/)

- 特点：声明式编码、组件化编码、React Native编写原生应用、高效的diffing算法

- 高效原因：虚拟DOM

- 相关js库：react.js，react-dom.js，babel.min.js

- [bootCDN](https://www.bootcdn.cn/)

  

### 2. react面向组件编程

常见用法已在[react.md](./react.md)中记录

- this：函数使用箭头函数调用

- state：

- props：

- 类式组件

  ```typescript
  class Person extends React.Component{
  	state = {isHot:false,wind:'微风'}
      const {name,age,sex} = this.props
      render(){
          return <div>{this.state.wind}</div>
      }
      changeWeather=()=>{
          const isHot = this.state.isHot
          this.setState({isHot:!isHot})
      }
  }
  // 限制props数据类型
  Person.propTypes = {
      name:PropTypes.string.isRequired, // 引入prop-types对属性进行限制
  	sex:PropTypes.string,
      age:PropTypes.number,
      speak:PropTypes.func // 限制为函数
  }
  // 指定默认标签值
  Person.defaultProps={
      sex:'中性',
      age:18,
  }
  const p = {name:'wc',age:18,sex:'男'}
  let p2 = {...p} // 字面量的方法复制对象 
  
  ReactDOM.render(< Person {...p} />,document.getElementById('test3'))
  
  class Person extends React.Component{
  	static propTypes = {
          name:PropTypes.string.isRequired,
  		sex:PropTypes.string,
      	age:PropTypes.number,
      	speak:PropTypes.func,
      }
      static defaultProps={
  		sex:'中性',
      	age:18,
      }
      const {name,age,sex} = this.props
      render(){
          return <div>{name}</div>
      }
  }
  
  ```

- 函数式组件

  ```typescript
  function Person(props){
      const {name,age,sex} = props
      return(
          <ul>
          	<li>{name}</li>
          	<li>{age}</li>
          	<li>{sex}</li>
          </ul>
      )
  }
  Person.propType
  Person.defaultProps
  const p = {name:'wc',age:18,sex:'男'}
  ReactDOM.render(< Person {...p} />,document.getElementById('test'))
  ```

- refs：组件内的标签可以定义ref属性来标识自己

  - 字符串命名的方式（未来弃用）
  - 回调函数的方式，写成class绑定函数
  - React.createRef()的方式

  ```typescript
  class Demo extends React.Component{
  	showData = ()=>{
  		//console.log(this.refs.input1);//拿到真正的节点
          const {input1} = this.refs;
          alert(input1.value)
  	}
      showData2 =()=>{
          //alert(this.input2.value) 
          alert(this.myRef.current.value)
      }
      saveInput = (c)=>{
          this.input3 = c
          console.log('@',c)
      }
      /*
      	React.createRef 调用后可以返回一个容器，该容器可以存储被ref所标识的节点，该容器是专用的
      */
      myRef = React.createRef()
  	render(){
  		return(
          	<div>
              	<input ref="input1" placeholder="" type="text"/>
              	<button onClick={this.showData}>按钮</button>
              	<input ref={(currentNode) => {this.input2=currentNode}} onBlur={this.showData2} type="text"/>//使用回调函数的方式ref 内联函数形式
              	<input ref={this.saveInput} /> //定义成class的绑定函数
                  <input ref={this.myRef} /> //定义成class的绑定函数
              </div>
          )
  	}
  }
  ```

- 受控组件

  - 受控组件可以省略ref

  ```typescript
  // 非受控组件 现用现取
  class Login extends React.Component{
  	handleSubmit = (event)=>{
          event.preventDefault()//组织默认提交
          const {username,password} = this
           alert(`用户名:${username.value}密码:${password.value}`)
      }
      render(){
          return(
          	<form action="http://www.baidu.com" onSubmit={this.handleSubmit}>
          	用户名:<input ref={c=>this.username=c} type="text" name="username"/>  
              密码 :<input ref={c=>this.password=c} type="password" name="password"/>  
              </form>
          )        
      }
  }
  // 受控组件 onChange()等的使用 受控组件
  class Login extends React.Component{
      state = {
          username:'',
          password:'',
      }
      saveUsername = (event)=>{
          this.setState({username:event.target.value})
      }
      savePassword = (event)=>{
          this.setState({password:event.target.value})
      }
  	handleSubmit = (event)=>{
          event.preventDefault()//组织默认提交
          const {username,password} = this.state
          alert(`用户名:${username}密码:${password}`)
      }
      render(){
          return(
          	<form action="http://www.baidu.com" onSubmit={this.handleSubmit}>
          	用户名:<input onChange={this.saveUsername} type="text" name="username"/>  
              密码 :<input onChange={this.savePassword} type="password" name="password"/>  
              </form>
          )        
      }
  }
  ```

- 高阶函数

  - 传参为函数
  - 返回值为函数
  - 函数柯里化：通过函数调用继续返回函数的方式，实现多次接收**参数**最后统一处理的函数编码形式

  ```javascript
  // 简化form里面的setState方法
  class Login extends React.Component{
      state = {
          username:'',
          password:'',
      }
  	// 高阶函数
  	saveFormData = (dataType)=>{
          return (event)=>{
              this.setState({[dataType]:event.target.value]})
          }
      }	
  	handleSubmit = (event)=>{
          event.preventDefault()//组织默认提交
          const {username,password} = this.state
          alert(`用户名:${username}密码:${password}`)
      }
      render(){
          return(
          	<form action="http://www.baidu.com" onSubmit={this.handleSubmit}>
          	用户名:<input onChange={this.saveFormData('username')} type="text" name="username"/>  
              密码 :<input onChange={this.saveFormData('password')} type="password" name="password"/>  
              </form>
          )        
      }
  }
  ```

- 生命周期

  ```javascript
  //组件挂载完毕
  componentDidMount(){
      // 初始化的事：开启定时器、发送网络请求、订阅消息
  }
  //组件将要卸载
  componentWillUnmount(){
      // 收尾工作，关闭定时器、取消订阅消息
  }
  //初始化渲染、状态更新之后
  render(){
  
  }
  //父组件render
  componentWillReceiveProps(){
      
  }
  //setState()之后
  shouldComponentUpdate(){
      // 默认返回true , 若手动设置成false则不能更新
  }
  //forceUpdate()之后
  componentWillUpdate(){
  
  }
  render(){}
  componentDidUpdate(preProps,preState){
  
  }
  
  // 新的生命周期
  static getDerivedStateFromProps(props,state){
      
  }
  getSnapshotBeforeUpdate(prevProps,prevState){}
  ```

  

- diffing算法

  - 遍历list，key最好不要用index，相同key节点复用
  - 虚拟dom中key的作用，减少没必要的多次渲染
  - 如果不存在对数据的逆序添加和逆序删除，index做key是没问题的

  
### 3. react应用

- react脚手架: react + webpack + es6 + eslint

  ```javascript
  // 1.全局安装
  npm install -g create-react-app
  // 2.
  create-react-app hello-react
  cd hello-react
  npm start
  npm run build
  ```

- vscode插件：

  - ES7 React...

- 功能界面的组件化编码流程

  - 拆分组件：拆分页面，抽取组件
  - 实现静态组件
  - 实现动态组件：数据、交互

- react案例分析

  ```javascript
  // 子组件修改父组件的state 父组件给子组件传一个函数修改自己的状态
  state = {todos:[
      {id:'01',name:'wc',done:true},
      {id:'02',name:'zx',done:false},
  ]}
  addTodo = (todoObj)=>{
      const {todos} = this.state // 解构赋值
      const newTodos = [todoObj,...todos]
      this.setState({todos:newTodos})
  }
  // 使用 nanoid 生成uuid唯一的key
  // onMouseEnter onMouseLeave 鼠标移入移出
  // 多用解构赋值
  updateTodo = (id,done)=>{
      const {todos} = this.state;
      const newTodos = todos.map((todoObj)=>{
          if(todoObj.id===id) return {...todoObj,done}
          else return todoObj
      })
      this.setState({todos:newTodos})
  }
  // 限制props属性
  import PropTypes from 'prop-types';
  static propTypes = {
      addTodo:PropTypes.func.isRequired,
      updateTodo:PropTypes.array.isRequired
  }
  arr.map
  arr.filter
  arr.reduce
  ```



### 4. react ajax

- axios:xhr

  - 封装XmlHttpRequest对象的ajax
  - promise风格
  - 可以用在浏览器端和node服务器端

  ```javascript
  npm install http-proxy-middleware -D
  // npm run eject
  // src下创建 setupProxy.js  存在一些问题，build打包之后，不能跨域访问
  const { createProxyMiddleware } = require('http-proxy-middleware')
  
  module.exports = function (app) {
      app.use(
          createProxyMiddleware('/wc', {
              target: 'http://10.50.60.206:8065',
              changeOrigin: true,
              pathRewrite: { '^/wc': '' },
          })
      )
}
  // 封装state更新方法
  updateAppState(stateObj)=>{
  	this.setState(stateObj);
  }
  // 三目运算符可以连着写
  ```
  
- 消息订阅-发布机制

  - 工具库 PubSubJS
  - 下载:npm install pubsub-js --save

  ```javascript
  // 兄弟组件 状态通信
  import PubSub from 'pubsub-js'
  
  componentDidMount(){
      // 订阅消息
      PubSub.subscribe('wc',(_,data)=>{
  		console.log(data);
          this.setState(data)
      })
  }
  // 发布消息
  PubSub.publish('wc',{name:'xx',age:18})
  ```

- fetch

  ```javascript
  search = ()=>{
      // 优化前版本
  	fetch('/xx').then(
      	res=>{
              return res.json()
          },
          error=>{
              console.log(error)
          }
      ).then(
      	res=>{},
          err=>{}
      )
      // 优化后版本
      try{
          const response = await fetch('/xx')
          const data = await response.json()
          console.log(data)
      }catch(error){
          console.log('请求出错',error)
      }
  }
  
  ```

  - promise

  ```javascript
  const p = new Promise((resolve,reject)=>{
      if xx resolve(xx)
      else reject(xx)
  })
  // Promise链式应用 p1 p2 都是返回Promise对象
  p.then(res=>return p1)
  	.then(res=>return p2)
  	.catch(err=>console.log(err))
  ```

  

### 5. react路由

- SPA：单页面应用

  - 点击页面中的链接不会刷新页面，只会局部更新

  ```javascript
  history.push() // 跳转
  history.replace()// 替代
  history.back() // 回退
  history.forword()// 前进
  ```

- react-router-dom 

  ```javascript
  import {BrowserRouter,Route,Switch,Link,NavLink,Redirect} from 'react-router-dom';
  
  class AppStart extends React.Component{
      render(){
          return(
          	<BrowserRouter>// 包在最外面
              	<Link to="/about"></Link> // 点击跳转
              	<Link to=`/home/1213` replace={true}></Link>
              	<Link to={{pathname:'/demo',state:{id:'123',name:'wc'}}}></Link> // 路径传递state对象
              	<NavLink activeClassName="active"></NavLink> // 点击加active样式
              	<Switch>
              		<Route path="/about" component={About}></Route> // 注册Route exact严格匹配
  					<Route path="/home/:id" component={Home}></Route>// 路由传参params
  					<Redirect to="/about">  // 兜底的
              	</Switch>
  			</BrowserRouter>
          )
      }
  }
  // 路由组件固定属性
  history:
  	go()
  	goBack()
  	goForward()
  	push()
  	replace()
  location:
  	pathname:
  	search
      state
  match:
  	params:
  	path:
  	url:
  
  // 封装NavLink组件
  // 标签体内容是特殊的标签属性children
  import React, { Component } from 'react'
  import { NavLink } from 'react-router-dom'
  
  export default class MyNavLink extends Component {
  
    render() {
      const { to, title } = this.props;
      return (
        <NavLink activeClassName="active" className="xxx" {...this.props} /> 
      )
    }
  }
  // 多级路由 前面的路由要添加，父路由不能开启严格匹配
  // 接收params参数 
  const {id,title} = this.props.match.params
  // 接收query参数
  // key=value&key=value urlencoded编码
  import qs from 'querystring'
  const {search} = this.props.location
  const {id,title} = qs.parse(search.slice(1))
  // 接收state参数
  const {state} = this.props.location
  
  // 编程式路由跳转
  this.props.history.push('...')
  
  import {withRouter} from 'react-router-dom';
  //withRouter可以加工一般组件，使一般组件具有 路由组件的api
  ```


### 6.ReactUI组件库

- antd

  ```javascript
  // 按需引入antd样式
  npm install react-app-rewired customize-cra
  // 修改package.json
"scripts":{
      "start":"react-app-rewired start",
      "build":"react-app-rewired build",
  	"test":"react-app-rewired test",
      "eject":"react-scripts eject", 
  }
  
  // 根目录下创建config-overrides.js
  const {override,fixBabelImports,addLessLoader} = require('customize-cra');
  
  module.exports = overide(
  	fixBabelImports('import',{
          libraryName:'antd',
          libraryDirectory:'es',
          style:true
      }),
      addLessLoader({
          lessOptions:{
              javascriptEnabled:true,
              modifyVars:{'@primary-color':'orange'},//修改默认主题样式
          }
      })
  )
  ```
  



### 7.redux

- 什么情况下需要使用redux

  - 某个组件的状态，需要让其他组件可以随时拿到（共享）
  - 一个组件需要改变另一个组件的状态（通信）
  - 总体原则：能不用就不用

  ![image-20210419144830272](\sgg_react.assets\image-20210419144830272.png)

















