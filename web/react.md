# React.js

## 1. 基本使用

- [官方文档](https://react.docschina.org/)

### 1.1 核心概念

- 创建React应用(官方脚手架)

  [官方脚手架说明文档](https://create-react-app.dev/)

  ```shell
  npx create-react-app my-app
  cd my-app
  npm start
  ```

- JSX简述  用对象来理解

  ```javascript
  const name = 'Josh Perez';
  const element = <h1>Hello, {name}</h1>;
  // 元素渲染
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
  
  // Babel会把JSX转化为 js
  const element = (
    <h1 className="greeting">
      Hello, world!
    </h1>
  );
  // 等价于
  const element = React.createElement(
    'h1',
    {className: 'greeting'},
    'Hello, world!'
  );
  ```

- 函数组件和class组件

  ```javascript
  // 函数组件 无状态
  function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
  }
  // class组件 有状态，生命周期
  class Welcome extends React.Component {
    render() {
      return <h1>Hello, {this.props.name}</h1>;
    }
  }
  // Porps的只读性
  ```

- State & 生命周期

  - props指父类属性，state当前组件中的属性

  - [生命周期](https://www.jianshu.com/p/b331d0e4b398)

    ```javascript
    constructor(props){super(props)} // React数据初始化 新版本弃用
    componentWillMount() // 组件挂载前(不常用)
    render() // 组件渲染
    componentDidMount() // 组件加载完成后
    componentWillUnmount(){ // 组件销毁
    	  //处理页面在异步调用过程中 销毁报错问题
            this.setState = (state, callback) => { return }
    }
    // 更新过程
    componentWillReceiveProps (nextProps) // 父组件改变
    shouldComponentUpdate(nextProps,nextState) // 用于部分更新
    componentWillUpdate(nextProps,nextState)
    componentDidUpdate(prevProps,prevState)
    render()
    ```
  
- 事件处理
  

推荐使用箭头函数 ()=>{} 这样不用 .bind(this)

- 条件渲染

  - &&的使用
  - 三目运算符

- 列表 & key

  map 渲染列表的时候，给每个元素设置一个key属性

  ```javascript
  function NumberList(props) {
    const numbers = props.numbers;
    return (
      <ul>
        {numbers.map((number) =>
          <ListItem key={number.toString()}
                    value={number} />
        )}
      </ul>
    );
  }
  ```

- 表单 (原生用法)

  ```javascript
  class NameForm extends React.Component {
    constructor(props) {
      super(props);
      this.state = {value: ''};
  
      this.handleChange = this.handleChange.bind(this);
      this.handleSubmit = this.handleSubmit.bind(this);
    }
  
    handleChange(event) {
      this.setState({value: event.target.value});
    }
  
    handleSubmit(event) {
      alert('提交的名字: ' + this.state.value);
      event.preventDefault();
    }
  
    render() {
      return (
        <form onSubmit={this.handleSubmit}>
          <label>
            名字:
            <input type="text" value={this.state.value} onChange={this.handleChange} />
          </label>
          <input type="submit" value="提交" />
        </form>
      );
    }
  }
  ```

- 状态提升

  解决方法是传入父组件修改父属性的方法给子组件

### 1.2 高级指引

- 代码分割

  ```javascript
  // 懒加载 基于路由的代码分割
  import React, { Suspense, lazy } from 'react';
  import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
  
  const Home = lazy(() => import('./routes/Home'));
  const About = lazy(() => import('./routes/About'));
  
  const App = () => (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Switch>
          <Route exact path="/" component={Home}/>
          <Route path="/about" component={About}/>
        </Switch>
      </Suspense>
    </Router>
  );
  // 异常捕获边界 服务端渲染问题需要捕获异常
  ```

- Context

  - Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据

    ```javascript
    const MyContext = React.createContext(defaultValue);
    <MyContext.Provider value={/* 某个值 */}>
    ```

- 错误边界(Error Boundarires)

  作用：可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会渲染出备用 UI

  错误边界的工作方式类似于 JavaScript 的 `catch {}`，不同的地方在于错误边界只针对 React 组件。只有 class 组件才可以成为错误边界组件。

  ```javascript
  class ErrorBoundary extends React.Component {
    constructor(props) {
      super(props);
      this.state = { hasError: false };
    }
  
    static getDerivedStateFromError(error) {
      // 更新 state 使下一次渲染能够显示降级后的 UI
      return { hasError: true };
    }
  
    componentDidCatch(error, errorInfo) {
      // 你同样可以将错误日志上报给服务器
      logErrorToMyService(error, errorInfo);
    }
  
    render() {
      if (this.state.hasError) {
        // 你可以自定义降级后的 UI 并渲染
        return <h1>Something went wrong.</h1>;
      }
  
      return this.props.children; 
    }
  }
  // 使用
  <ErrorBoundary>
    <MyWidget />
  </ErrorBoundary>
  ```

- - [ ] Refs转发 

  Ref 转发是一项将 ref自动地通过组件传递到其一子组件的技巧

  ```typescript
  const FancyButton = React.forwardRef((props, ref) => (
    <button ref={ref} className="FancyButton">
      {props.children}
    </button>
  ));
  
  // 你可以直接获取 DOM button 的 ref：
  const ref = React.createRef();
  <FancyButton ref={ref}>Click me!</FancyButton>;
  ```

- Fragments

  Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点,可设置key

- - [ ] 高阶组件(HOC)

  它是一种基于 React 的组合特性而形成的设计模式
  
  具体而言，**高阶组件是参数为组件，返回值为新组件的函数**
  
- 与第三方库协同
  
  在React中使用其他的插件`jQuery`等
  
  ```javascript
  class SomePlugin extends React.Component {
    componentDidMount() {
      this.$el = $(this.el);
      this.$el.somePlugin();
    }
  
    componentWillUnmount() {
      this.$el.somePlugin('destroy');
    }
  
    render() {
      return <div ref={el => this.el = el} />;
    }
  }
  ```
  
  

## 2. 进阶用法







# Next.js