## 尚硅谷react技术全家桶

### 1. react相关

- [官网](https://reactjs.org/)

- 特点：声明式编码、组件化编码、React Native编写原生应用、高效的diffing算法

- 高效原因：虚拟DOM

- 相关js库：react.js，react-dom.js，babel.min.js

- [bootCDN](https://www.bootcdn.cn/)

  

### 2. React面向组件编程

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
              this.setState({[dataType:event.target.value]})
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

- 























