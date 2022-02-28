## Nodejs技术栈公众号随笔

```ini
说明: 记录公众号的关键知识与内容，拓宽自己的知识面

```



### *2022-01-18*前端领域架构模式：“干净架构”

> 什么是干净架构

干净架构是一种根据应用程序的领域（`domain`）的相似程度来拆分职责和功能的方法。通常分为三层，

![image-20220118092726852](nodejs技术栈日记.assets\image-20220118092726852.png)

- 领域层

  改变某些用例的时候不会变的那一部分，

  会描述应用程序主题区域的实体和数据，以及转换该数据的代码

  例：领域就是产品、订单、用户、购物车以及更新这些数据的方法

- 应用层

  用例：向服务器发送请求、执行领域转换、使用响应的数据更新UI

  端口被认为是一个现实世界和应用程序之间的"缓冲区"

- 适配器层

  可以降低代码和外部第三方服务的耦合

  驱动型、被动型

  离中心越远，代码的功能就越 “面向服务”

- 依赖规则

  三层架构有一个依赖规则：只有外层可以依赖内层

  领域必须独立

  应用层可以依赖领域

  最外层可以依赖任何东西



> 干净架构优势

- 独立领域

  所有应用的核心功能都被拆分并统一维护在一个地方—领域

- 独立用例

  用例的代码也是扁平的，并且容易测试，扩展性强

- 可替换的第三方服务

  只要我们不改接口，那么实现这个接口的是哪个第三方服务都没关系

> 干净架构成本

需要更多时间

有时显得多余

上手更困难

代码量增加

> 如何降低成本

抽象领域，遵守依赖原则，尝试写适配器

![image-20220118103509642](nodejs技术栈日记.assets\image-20220118103509642.png)

项目目录结构举例

![image-20220118103640787](nodejs技术栈日记.assets\image-20220118103640787.png)



#### 实现细节

> 共享内核

在实践中，共享内核可以这样解释：我们用到 `TypeScript`，使用它的标准类型库，但我们不会把它们看作是一个依赖项。这是因为使用它们的模块互相不会产生影响并且可以保持解耦。



### *2022-02-25*TypeScript 中 interface 和 type 的区别，你真的懂了吗？

#### 类型别名 type

类型别名用来给一个类型起个新名字，使用 `type` 创建类型别名，类型别名不仅可以用来表示基本类型，还可以用来表示对象类型、联合类型、元组和交集。

```typescript
type userName = string; // 基本类型
type userId = string | number;
type arr = number[];

// 对象类型
type Person = {
  id: userId;
  name: userName;
  age: number;
  gender: string;
  isWebDev: boolean;
};
// 范型
type Tree<T> = { value: T };

const user: Person = {
  id: "901",
  name: "春",
  age: 22,
  gender: "女",
  isWebDev: false,
};

const numbers: arr = [1, 2, 9];
```

#### 接口 interface

接口是命名数据结构（例如对象）的另一种方式；与`type` 不同，`interface`仅限于描述对象类型。

```typescript
interface Person {
    id: userId;
    name: userName;
    age: number;
    gender: string;
    isWebDev: boolean;
}
```

#### interface和type的相似之处

```typescript
// 都可以描述 Object和Function

type Point = {
  x: number;
  y: number;
};

type SetPoint = (x: number, y: number) => void;

interface Point {
  x: number;
  y: number;
}

interface SetPoint {
  (x: number, y: number): void;
}

// 二者都可以被继承
interface Person{
    name:string
}
type Person{
    name:string
}
// interface继承 type和interface
interface Student extends Person { stuNo: number }
// type 继承 type和interface
type Student = Person & { stuNo: number }

// 实现implements
interface ICat{
    setName(name:string): void;
}
// type 
type ICat = {
    setName(name:string): void;
}

class Cat implements ICat{
    setName(name:string):void{
        // todo
    }
}


// 类无法实现联合类型
type Person = { name: string; } | { setName(name:string): void };

// 无法对联合类型Person进行实现
// error: A class can only implement an object type or intersection of object types with statically known members.
class Student implements Person {
  name= "张三";
  setName(name:string):void{
        // todo
    }
}

```

#### 二者区别

- type可以定义基本类型别名
- type可以申明联合类型
- type可以声明 **元组类型**
- interface可以声明合并
- 索引签名问题

```typescript
type userName = string
// 联合类型
type Student = {stuNo: number} | {classId: number}
// 元组类型
type Data = [number, string];
// 声明合并
interface Person { name: string }
interface Person { age: number }

let user: Person = {
    name: "Tolu",
    age: 0,
};

```

### *2022-02-28*前端性能优化 - React.memo 解决函数组件重复渲染

使用 React Hooks 时函数组件应用的比较多，当遇到组件重复渲染问题不像类组件可以使用生命周期函数 `shouldComponentUpdate` 或 `extends React.PureComponent` 解决重复渲染问题。

函数组件中的解决方案是使用 React.memo() 函数，将需要优化的函数组件传入即可。

```javascript
import React, { useEffect, useState } from "react";

// 未使用 memo：const List = ({ dataList }) => {
const List = React.memo(({ dataList }) => {
  console.log("List 渲染");

  return (
    <div>
      {dataList.map((item) => (
        <h2 key={item.id}> {item.title} </h2>
      ))}
    </div>
  );
});

const Home = () => {
  const [count, setCount] = useState(0);
  const [dataList, setDataList] = useState([]);

  useEffect(() => {
    const list = [
      { title: "React 性能优化", id: 1 },
      { title: "Node.js 性能优化", id: 2 },
    ];
    setDataList(list);
  }, []);

  return (
    <div>
      <button type="button" onClick={() => setCount(count + 1)}>
        count: {count}
      </button>
      <List dataList={dataList} />
    </div>
  );
};

export default Home;

// React.memo() 的 TypeScript 类型描述
// 函数React.memo() 还提供了第二个参数 propsAreEqual，用来自定义控制对比过程
function memo<T extends ComponentType<any>>(
  Component: T,
  propsAreEqual?: (
    prevProps: Readonly<ComponentProps<T>>,
    nextProps: Readonly<ComponentProps<T>>
  ) => boolean
): MemoExoticComponent<T>;
```

> React.memo 无效情况

**一是 React.memo 对普通的引用类型是无效**的。例如，在 List 组件增加 user 属性，即使使用了 React.memo() ，每次点击 count， List 组件还会重复渲染。

```javascript
const Home = () => {
  const user = {name: '哈哈'};
  return (
    <div>
      <List dataList={dataList} user={user} />
    </div>
  );
};
const user = useMemo(() => ({ name: "哈哈" }), []);
const [user] = useState({ name: "哈哈" });
```

还有一种情况是**函数组中包括了一些 Hooks 例如 useState、useContext，当上下文发生变化时，组件也同样会重新渲染，React.memo 在这里仅比较 props**。上面例子中，如果把 button 组件放到 List 组件里，每次点击，List 也还是会被重新渲染。

```javascript
const List = React.memo(({ dataList }) => {
  console.log("List 渲染");
  const [count, setCount] = useState(0);

  return (
    <div>
      <button type="button" onClick={() => setCount(count + 1)}>
        List count: {count}
      </button>
      {dataList.map((item) => (
        <h2 key={item.id}> {item.title} </h2>
      ))}
    </div>
  );
});
```

> 总结

React.memo() 是一个高阶组件，接收一个组件并返回一个新组件。它会记忆组件上次的 Props，同下次需要更新的 Props 做 “浅对比”，如果相同就不做更新，只有在不同时才会重新渲染。如果你的组件存在一些耗时的计算，每次重新渲染对页面性能显然是糟糕的，这时 React.memo() 对你来说也许是一个好的选择。并不是所有的组件都要引入 React.memo()，自身浅对比这个过程也会有一些消耗，如果没有特殊需求，也不一定非要使用。