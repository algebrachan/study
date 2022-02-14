# React.js
```init
更新时间：2021年1月11日10:40:23
说明：react官方文档分析
```



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

- State & <span id="period">生命周期</span>

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

- <span id="lazy">**代码分割**</span>

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

- <span id="ref">**Refs转发**</span>

  Ref 转发是一项将 ref自动地通过组件传递到其一子组件的技巧

  不能在函数式组件上使用，因为没有实例

  ```typescript
  const FancyButton = React.forwardRef((props, ref) => (
    <button ref={ref} className="FancyButton">
      {props.children}
    </button>
  ));
  
  // 你可以直接获取 DOM button 的 ref：
  const ref = React.createRef();
  <FancyButton ref={ref}>Click me!</FancyButton>;
  // 对该节点的引用可以在current属性中被访问
  const node = this.ref.current;
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

  backbone的使用

  ```typescript
  function connectToBackboneModel(WrappedComponent) {
    return class BackboneComponent extends React.Component {
      constructor(props) {
        super(props);
        this.state = Object.assign({}, props.model.attributes);
        this.handleChange = this.handleChange.bind(this);
      }
  
      componentDidMount() {
        this.props.model.on('change', this.handleChange);
      }
  
      componentWillReceiveProps(nextProps) {
        this.setState(Object.assign({}, nextProps.model.attributes));
        if (nextProps.model !== this.props.model) {
          this.props.model.off('change', this.handleChange);
          nextProps.model.on('change', this.handleChange);
        }
      }
  
      componentWillUnmount() {
        this.props.model.off('change', this.handleChange);
      }
  
      handleChange(model) {
        this.setState(model.changedAttributes());
      }
  
      render() {
        const propsExceptModel = Object.assign({}, this.props);
        delete propsExceptModel.model;
        return <WrappedComponent {...propsExceptModel} {...this.state} />;
      }
    }
  }
  
  function NameInput(props) {
    return (
      <p>
        <input value={props.firstName} onChange={props.handleChange} />
        <br />
        My name is {props.firstName}.
      </p>
    );
  }
  
  const BackboneNameInput = connectToBackboneModel(NameInput);
  
  function Example(props) {
    function handleChange(e) {
      props.model.set('firstName', e.target.value);
    }
  
    return (
      <BackboneNameInput
        model={props.model}
        handleChange={handleChange}
      />
    );
  }
  
  const model = new Backbone.Model({ firstName: 'Frodo' });
  ReactDOM.render(
    <Example model={model} />,
    document.getElementById('root')
  );
  ```
  
- 性能优化
  
  最小化DOM操作，生产模式和开发模式的区分
  
  虚拟化长列表，[react-window](https://react-window.now.sh/]) [react-virtualized](https://bvaughn.github.io/react-virtualized/)
  
  避免调停：覆盖生命周期跳过渲染 shouldComponentUpdate(){return true} / 继承React.PureComponent，浅比较

- Protals、Profiler
  
  Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案
  
### 1.3 Api Reference

- React

  - React.Component 经常使用

  - React.PureComponent：浅层对比 prop 和 state 的方式来实现了函数shouldComponentUpdate()

  - React.memo (包函数)：

    ```typescript
    function MyComponent(props) {
      /* 使用 props 渲染 */
    }
    function areEqual(prevProps, nextProps) {
      /*
      如果把 nextProps 传入 render 方法的返回结果与
      将 prevProps 传入 render 方法的返回结果一致则返回 true，
      否则返回 false
      */
    }
    export default React.memo(MyComponent, areEqual);// 仅用来性能优化
    ```

  - createElement()

    ```typescript
    React.createElement(
      type, // 'div'
      [props],
      [...children]
    )
    React.cloneElement(
      element,
      [props],
      [...children]
    )
    React.createFactory(type)
    ```

  - createFactory()

  - cloneElement()

  - isValidElement()：判断是否为React元素，返回值为true、false

  - React.Children

    ```typescript
    React.Children.map(children, function[(thisArg)])
    React.Children.forEach(children, function[(thisArg)])
    React.Children.count(children)
    React.Children.only(children)
    React.Children.toArray(children) // 将children这个复杂的数据结构以数组的方式扁平展开并返回
    ```

  - React.Fragment 等价于<></> 

  - React.[createRef](#ref)

  - React.forwardRef

  - React.[lazy](#lazy)

  - React.Suspense

- React.Component 的子类中必须定义render()函数

  - [生命周期](#period)
  - setState() 修改state的值，是异步方法
  - forceUpdate() 强制重新渲染
  - defaultProps
  - displayName
  - props
  - state

- ReactDOM

  ```typescript
  ReactDOM.render(element, container[, callback])//渲染React元素到htmlDom
  ReactDOM.hydrate(element, container[, callback])
  ReactDOM.unmountComponentAtNode(container) // 卸载组件
  ReactDOM.findDOMNode(component) // 严格模式已弃用
  ReactDOM.createPortal(child, container) // 将子节点渲染到DOM节点中的方式
  ```

- ReactDOMServer 仅服务端可用

  ```typescript
  // ES modules
  import ReactDOMServer from 'react-dom/server';
  // CommonJS
  var ReactDOMServer = require('react-dom/server');
  ReactDOMServer.renderToString(element)
  ReactDOMServer.renderToStaticMarkup(element)
  ReactDOMServer.renderToNodeStream(element)
  ReactDOMServer.renderToStaticNodeStream(element)
  ```

- DOM元素：与ReactDOM元素的不同，例如：className、onChange等

- 合成事件：需要使用浏览器底层事件

  ```typescript
  // 剪贴板事件
  onCopy onCut onPaste
  // 复合事件
  onCompositionEnd onCompositionStart onCompositionUpdate
  // 键盘事件
  onKeyDown onKeyPress onKeyUp
  // 焦点事件
  onFocus onBlur
  // 表单事件
  onChange onInput onInvalid onReset onSubmit 
  // 通用事件
  onError onLoad
  // 鼠标事件
  onClick onContextMenu onDoubleClick onDrag onDragEnd onDragEnter onDragExit
  onDragLeave onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave
  onMouseMove onMouseOut onMouseOver onMouseUp
  // 指针事件
  onPointerDown onPointerMove onPointerUp onPointerCancel onGotPointerCapture
  onLostPointerCapture onPointerEnter onPointerLeave onPointerOver onPointerOut
  // 选择事件
  onSelect
  // 触摸事件
  onTouchCancel onTouchEnd onTouchMove onTouchStart
  // UI事件
  onScroll
  // 滚轮事件
  onWheel
  // 媒体事件
  onAbort onCanPlay onCanPlayThrough onDurationChange onEmptied onEncrypted
  onEnded onError onLoadedData onLoadedMetadata onLoadStart onPause onPlay
  onPlaying onProgress onRateChange onSeeked onSeeking onStalled onSuspend
  onTimeUpdate onVolumeChange onWaiting
  // 图像事件
  onLoad onError
  // 动画事件
  onAnimationStart onAnimationEnd onAnimationIteration
  // 过渡事件
  onTransitionEnd
  // 其他事件
  onToggle
  ```

- Test Utilities 测试

  ```typescript
  // 测试demo
  class Counter extends React.Component {
    constructor(props) {
      super(props);
      this.state = {count: 0};
      this.handleClick = this.handleClick.bind(this);
    }
    componentDidMount() {
      document.title = `You clicked ${this.state.count} times`;
    }
    componentDidUpdate() {
      document.title = `You clicked ${this.state.count} times`;
    }
    handleClick() {
      this.setState(state => ({
        count: state.count + 1,
      }));
    }
    render() {
      return (
        <div>
          <p>You clicked {this.state.count} times</p>
          <button onClick={this.handleClick}>
            Click me
          </button>
        </div>
      );
    }
  }
  // 测试代码
  import React from 'react';
  import ReactDOM from 'react-dom';
  import { act } from 'react-dom/test-utils';
  import Counter from './Counter';
  
  let container;
  
  beforeEach(() => {
    container = document.createElement('div');
    document.body.appendChild(container);
  });
  
  afterEach(() => {
    document.body.removeChild(container);
    container = null;
  });
  
  it('can render and update a counter', () => {
    // 首先测试 render 和 componentDidMount
    act(() => {
      ReactDOM.render(<Counter />, container);
    });
    const button = container.querySelector('button');
    const label = container.querySelector('p');
    expect(label.textContent).toBe('You clicked 0 times');
    expect(document.title).toBe('You clicked 0 times');
  
    // 再测试 render 和 componentDidUpdate
    act(() => {
      button.dispatchEvent(new MouseEvent('click', {bubbles: true}));
    });
    expect(label.textContent).toBe('You clicked 1 times');
    expect(document.title).toBe('You clicked 1 times');
  });
  ```

  

- Hook

  - useState
  - useEffect
  - useContext
  - useReducer
  - useCallback
  - useMemo
  - useRef
  - useImperativeHandle
  - useLayoutEffect
  - useDebugValue

### 1.4 HOOK

- 简介：Hook 使你在非 class 的情况下可以使用更多的 React 特性

- 概述：Hook 是一些可以让你在**函数组件**里“钩入” React state 及生命周期等特性的函数

- **useState**：

  ```typescript
  import React, { useState } from 'react';
  
  function Example() {
    // 声明一个叫 "count" 的 state 变量
    const [count, setCount] = useState(0);
  
    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>
          Click me
        </button>
      </div>
    );
  }
  // class写法
  class Example extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        count: 0
      };
    }
    render() {
      return (
        <div>
          <p>You clicked {this.state.count} times</p>
          <button onClick={() => this.setState({ count: this.state.count + 1 })}>
            Click me
          </button>
        </div>
      );
    }
  }
  ```

- useEffect

  可以将useEffect看做是componentDidMount，componentDidUpdate，componentWillUnmount的组合

  ```typescript
  // 需要清除effect
  useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
  
      ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
      };
  });
  // 不需要清除
   useEffect(() => {
      document.title = `You clicked ${count} times`;
    });
  
  // 通过跳过effect进行性能优化
  componentDidUpdate(prevProps, prevState) {
    if (prevState.count !== this.state.count) {
      document.title = `You clicked ${this.state.count} times`;
    }
  }
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]); // 仅在 count 更改时更新
  ```

- Hook规则：1.只在最顶层使用Hook 2.只在React函数中调用Hook

- 自定义Hook

  ```typescript
  // 提取自定义Hook
  import { useState, useEffect } from 'react';
  
  function useFriendStatus(friendID) {
    const [isOnline, setIsOnline] = useState(null);
  
    useEffect(() => {
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
  
      ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
      return () => {
        ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
      };
    });
  
    return isOnline;
  }
  // 使用自定义Hook
  function FriendStatus(props) {
    const isOnline = useFriendStatus(props.friend.id);
  
    if (isOnline === null) {
      return 'Loading...';
    }
    return isOnline ? 'Online' : 'Offline';
  }
  function FriendListItem(props) {
    const isOnline = useFriendStatus(props.friend.id);
  
    return (
      <li style={{ color: isOnline ? 'green' : 'black' }}>
        {props.friend.name}
      </li>
    );
  }
  ```
### 1.5 测试

- [Jest](https://jestjs.io/)
- 测试技巧
- 测试环境

## 2. 进阶用法

### 2.1 ES6语法

```typescript
// 不改变原有对象
function updateColorMap(colormap) {
  return Object.assign({}, colormap, {right: 'blue'});
}
// 更新不可变对象 对象扩展运算符
function updateColorMap(colormap) {
  return {...colormap, right: 'blue'};
}
// 数组扩展运算符
this.setState(state => ({
  words: [...state.words, 'marklar'],
}));
```

### 2.2 React测试

https://www.cnblogs.com/testopsfeng/p/14265218.html

执行 `npm test` 项目中所有 test.js为结尾的



## 3.遇到的问题

- react-router和react-router-dom的区别？

  ```javascript
  import {Swtich, Route, Router, HashHistory, Link} from 'react-router-dom';
  
  import {Switch, Route, Router} from 'react-router';
  import {HashHistory, Link} from 'react-router-dom';
  
  // react-router: 实现了路由的核心功能
  // react-router-dom: 基于react-router，加入了在浏览器运行环境下的一些功能，例如：Link组件，会渲染一个a标签，Link组件源码a标签行; BrowserRouter和HashRouter组件，前者使用pushState和popState事件构建路由，后者使用window.location.hash和hashchange事件构建路由。
  
  // 安装了react-router-dom，npm会解析并安装上述依赖包。可以看到，其中包括react-router。
  ```

  

# Next.js