## 尚硅谷vue全家桶 (vue2+vue3)

### 1.vue基础

> vue是什么

一套用于构建用户界面的渐进式JavaScript框架

特点：

- 采用**组件化**模式，提高代码复用率、且让代码更好维护
- 声明式编程，让编程人员无需直接操作DOM，提高开发效率
- 使用**虚拟DOM**+优秀的**Diff算法**，尽量复用DOM节点

```html
<div id="root">
    <h1>
        hello {{name}}
    </h1>
</div>
<script>
new Vue({
    el: '#root',
    data: {
        name: 'wc',
        url: 'http://www.baidu.com'
    }
})
const vm = new Vue({
    //data(){}
    data:function(){
        return{
        	name: 'wc',
        	url: 'http://www.baidu.com'
        }
    }
})
v.$mount('#root') //先创建，再渲染
// data的两种写法： 对象式、函数式
</script>
<!-- data的两种写法 -->
```



> MVVM模型

![image-20220211133017558](sgg_vue.assets\image-20220211133017558.png)

- M:模型 Model: 对应 data 中的数据
- V:视图 View: 模板
- VM:视图模型 ViewModel： Vue实例对象

#### 模板语法

- 插值语法:
  - 功能：用于解析标签体内容
  - 写法： **{{xxx}}** xxx是js表达式，且可以直接读取data中的所有属性
- 指令语法
  - 功能：用于解析标签 (包括： 标签属性、标签体内容、绑定事件)
  - 举例：**v-bind:href="xxx"** 或简写 :href="xxx",xxx同样要js表达式，且可以读取到data中的所有属性
  - 备注： Vue有很多指令，且形式都是 v-??? 

#### 数据绑定

- 单向绑定 (v-bind) : 数据只能从data流向页面
- 双向绑定（v-model）:数据不仅能从data流向页面，也能从页面流向data
  - 双向绑定一般都应用在表单类元素上 input、select
  - v-model:value 可以简写为 v-model 默认收集的就是value的值

#### 数据代理

通过一个对象代理对另一个对象中属性的操作

数据代理吧data中的数据 放到了 vue中

```javascript
let number = 18
let person = {
    name:'zhangsan',
    sex:'男'
}
// 数据代理的核心
Object.defineProperty(person,'age',{
    get(){
        return number
    },
    set(value){
        number = value
    }
    
})
```

#### 事件处理

```html
<!-- @click等价于v-on: -->
<button @click="showInfo($event,66)">点击</button>
<script>
	const vm = new Vue({
        el:'#root',
        data:{
            name:'wv'
        },
        methods:{
            showInfo(event,number){
			}
        }
    })
</script>
```

事件修饰符 .xx

- prevent：阻止默认事件
- stop：阻止事件冒泡
- once：事件只触发一次
- capture：使用事件的捕获模式
- self：只有event.target是当前操作的元素才触发事件
- passive：事件的默认行为立即执行，无需等待事件回调执行完毕

冒泡：从底往外

捕获：从外往里

@keyup按键抬起事件

#### 计算属性

```javascript
const vm = new Vue({
	el:'#root',
	data:{
		firstname:'wv',
        lastname:'王'
	},
	methods:{},
    computed:{
        fullName:{
            // get 当有人读取fullName时，get被调用，返回值就作为fullName的值
            // get什么时候被执行，初次调用fullName，所依赖的数据发生变化
            // 计算属性最终会出现在vm上，直接读取就可以使用
            // 计算属性要被修改，必须写set函数去响应
            get(){
                console.log('get被调用')
                return this.firstname + '-' + this.lastname
            }
            set(){}
        }
    }
})
```





### 2.vue-cli

### 3.vue-router

### 4.vuex

### 5.element-ui

### 6.vue3

