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
    methods:{
    	this.$set(this.name,'')
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
  - v-model.number 控制输入为数字
  - v-model.lazy 输入框失去焦点再变换数字
  - v-model.trim 去掉前后空格

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

计算属性必须是已经存在的数据去计算

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
            // 简写 只读取 不修改
        fullName(){
    		return this.firstname + '-' + this.lastname
		}
    }
})
```

#### 监视属性

```javascript
const vm = new Vue({
	el:'#root',
	data:{
		isHot:true,
        number:{
			a:1,
            b:2
        }
	},
	methods:{},
    computed:{},
    watch:{
		isHot:{// 监视isHot属性
           	handler(newValue,oldValue){
				console.log('isHot被修改了',newValue,oldValue)
            }
        },
        'number.a':{
            handler(){console.log('a改变了')}
        },
        number:{
            immediate:true,// 一开始就调用一次
            deep:true,// 深度监视 监视多级数据,需要手动开启
            handler(){}
        }
        // 监视简写
        isHot(newValue,oldValue){
    		console.log('isHot被修改了',newValue,oldValue)
		}
    }
})
```

watch和computed对比：

- computed能完成的功能，watch都可以完成
- watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作

注意：

- 被vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象
- 所有不被vue管理的函数（定时器的回调函数，ajax的回调函数等），最好写成箭头函数，这样this的指向才是vm或组件实例

#### 样式

```html
<div class="demo" :class="mod"> 动态指定样式:class   
</div>
```

- class样式
  - 写法：class="xxx" xxx可以是字符串、对象、数组
  - 字符串适用于：类名不确定、要动态获取
  - 对象写法适用于：绑定多个样式、个数不确定、名字不确定
  - 数组写法适用于：绑定多个样式，个数确定、名字也确定，但不确定用不用
- style样式
  - :style="{fontSize:xxx}" xxx是动态值
  - :style="[a,b]" 其中a、b是样式对象



#### 条件渲染、列表渲染

```vue
<div v-if></div> 
<div v-show></div>
<ul>
    <li v-for="p in parent" :key="p.id">
    	{{p.name}}-{{p.age}}
    </li>
</ul>
```

- v-if： 删除 or 新增，适用于切换频率低的场景
- v-show: 渲染好了，只是隐藏了，适用于切换频率高的场景
- v-for: 可以遍历列表 (item,key)、 可以遍历对象 (value,key)

> key的作用和原理

虚拟DOM上必须要有key，真实DOM上没有key，

使用index为key，对数据进行破话顺序的操作，diff算法会出错

- 虚拟DOM中key的作用：
  - key是虚拟DOM对象标识，当数据发生变化时，vue会根据 [新数据] 生成 新 [虚拟DOM ] 
  - 随后vue进行 [新虚拟DOM] 与 [旧虚拟DOM] 的差异比较
- 对比规则：
  - 旧虚拟DOM中找到与新虚拟DOM相同的key
    - 若虚拟DOM中的内容不变，直接使用之前的真实DOM
    - 若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中的真实DOM
  - 旧虚拟DOM 中未找到与新虚拟DOM相同的key
    - 创建新的真实DOM, 随后渲染到页面
- 用index作为key，可能引发的问题：
  - 若对数据进行：逆序添加、逆序删除等破坏顺序操作，会产生没有必要的真实DOM更新 ==> 页面效果没问题，但效率低
  - 如果结构中还包含输入类的DOM：会产生错误DOM更新 ==> 页面有问题
- 开发中如何选择key
  - 最好使用唯一标识，id等
  - 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key没有问题

> 列表过滤

建议使用computed属性



#### Vue监测改变数据的原理

![image-20220215113507106](sgg_vue.assets\image-20220215113507106.png)



#### 过滤器

定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）

语法：注册过滤器： Vue.filter(name,callback) 或  new Vue(filters:{})

​			使用过滤器：{{ xxx | 过滤器名 }} 或 v-bind:属性 = “xxx | 过滤器名”

备注： 过滤器也可以读取额外参数，多个过滤器可串联

​			并不改变原有数据，是产生新的对应的数据

 

```html
<h3>{{time | timeFormater | mySlice}}</h3> // 管道
<script>
Vue.filter('mySlice',function(value){
    return value.slice(0,4)
})    
    
    
new Vue({
    el:'#root',
    data:{
        time:12132314224,
    },
    filters:{
        timeFormater(value){
            return dayjs(value).format('YYYY-MM-DD HH:mm:ss')
        },
        mySlice(value){
            return value.slice(0,4)
        }
    }
})
</script>

```

#### 内置指令

- v-bind： 单向绑定
- v-on： 绑定事件监听，可简写@
- v-model：数据双向绑定
- v-for：遍历数组/对象/字符串
- v-if：条件渲染，是否存在
- v-else：条件渲染
- v-show：条件渲染，是否显示
- v-text：向其所在的节点中渲染文本内容，与插值语法区别：v-text会替换节点内容，{{xx}}则不会
- v-html：x向指定节点中渲染包含html结构的内容；有安全性问题，容易导致XSS攻击
- v-clock：没有值，本质是一个特殊属性，可以解决网速慢时页面显示出{{xxx}}的问题
- v-once：所在节点初次动态渲染后，视为静态内容，以后数据的改变不会引起v-once所在节点的更新，用于优化性能
- v-pre：跳过其所在节点的编译过程，利用这个跳过没有指令语法、没有使用插值语法的节点，会加快编译



#### 自定义指令

```html
<script>
const vm = new Vue({
	el:'#root',
	data:{
		n:1,
	},
	directives:{ // 函数式
		big(element,binding){
			element.innerText = binding.value * 10
		},
		fbind(element,binding){ // 对象式
			bind(element,binding){}, // 指令与元素成功绑定时
			inserted(element,binding){}, // 指令所在元素被插入页面时
			update(element,binding){} // 指令所在 的模板被重新解析时
		}
	}
})
</script>
```

#### 生命周期

![lifecycle](\sgg_vue.assets\lifecycle.png)

- mounted：Vue完成模板的解析并把初始的真实DOM元素放入页面 后（挂载完毕）调用mounted

  - 初始化过程结束，一般在此进行：开启定时器、发送网络请求、订阅消息、绑定自定义事件

- beforeDestroy：销毁之前，关闭定时器，取消订阅消息，解绑自定义事件等收尾工作

  

### 2.Vue组件化编程

> 组件化

当应用中的功能是多组件的 方式来编写的，那这个应用就是一个组件化的应用

创建组件 -> 注册组件 -> 编写组件标签

组件嵌套

- 定义组件
  - Vue.extend(options)创建，其中options的el不写，data必须写成函数，template可以配置组件结构
- 注册组件
  - 局部注册： new Vue的时候传入 components选项
  - 全局注册：Vue.component('组件名'，组件)
- 使用组件
  - <school> <school/>

> VueComponent

- 组件本质是一个名为VueComponent的构造函数，是Vue.extend生成的
- 只需要写< school >标签，Vue解析时会创建school组件的实例对象 执行 new VueComponent(options)
- 每次调用 Vue.extend 返回的都是一个全新的VueComponent ！！！
- this的指向
  - 组件配置中：data函数、methods函数、watch函数、computed函数 this均是VueCOmponent实例对象
  - new Vue() 配置中：以上函数，this均是 Vue实例对象
- VueComponent的实例对象，组件实例对象 简称vc ，vue实例对象 简称vm



#### 单文件文件

```vue
<!--  -->
<template>
  <div></div>
</template>

<script>
export default {
  data() {
    return {};
  },
  //生命周期 - 创建完成（访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（访问DOM元素）
  mounted() {},
};
</script>
<style scoped>
/* @import url(); 引入css类 */
</style>
```

#### 组件自定义事件

```vue
<template>
	<!-- 通过父组件给子组件传递函数类型的props实现 子传父 -->
	<School :getSchoolName="getSchollName"/>
	<!-- 通过父组件给子组件一个自定义事件实现 子传父 使用@-->
	<Student v-on:atguigu="getStudentName"/>
	
	<!-- 通过父组件给子组件一个自定义事件实现 子传父 使用ref
	组件绑定原生事件需要 写成 @click.native
-->
	<Student ref="student" @click.native="show"/> 
	
</template>
<script>
	export default{
        name:'App',
        component:{School,Student},
        data(){},
        methods:{
            getSchollName(name){
                
			},
            getStudentName(name){
                
            }
        },
        mounted(){
            this.$refs.student.$on('atguigu',getStudentName)
            // 触发一次 用 $once
        }
        
    }
</script>

<Student>
    <script>
    this.$emit('atguigu',this.name) // 触发自定义事件
    this.$off(['atguigu','demo']) // 解绑多个自定义事件
    // 
    this.$destroy() // 销毁了当前Student组件的实例，销毁后所有Student的自定义事件都不奏效
    </script>
</Student>
```

- 适用于 子 ==> 父
- 使用场景： A是父，B是子，B=>A，那么就要在A中给B绑定自定义事件 （事件回调在A中）
- 注意： 通过 `this.$refs.xxx.$on('atguigu',callback)` 绑定自定义事件时，回调要么配置在methods中，要么使用箭头函数 否则this指向会有问题



#### 全局事件总线

可以实现任意组件间通信

```javascript
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  beforeCreate(){
      Vue.prototype.$bus = this // 安装全局事件总线
  }
}).$mount('#app')

// 使用
// 1.接收数据：
methods(){
    demo(data){...}
}
...
mounted(){
    this.$bus.$on('xxxx',this.demo)
}
// 2.提供数据
this.$bus.$emit('xxxx',数据)
    
// 最好在beforeDestroy钩子中，用$off去解绑当前组件所用到的事件
```

#### 发布订阅

可以实现任意组件间通信

`npm i pubsub-js --save`

```javascript
import pubsub from 'pubsub-js'

methods(){
    demo(data){...}
}
mounted(){
    this.pid = pubsub.subscribe('xxx',this.demo)//订阅消息
}
    
// 发布消息
pubsub.publish('xxx',数据)

// 取消订阅
beforeDestroy(){
	pubsub.unsubscribe(pid)
}
```



##### $nextTick

- 语法： this.$nextTick(callback)
- 作用：在下一次DOM更新结束后执行其指定的回调
- 什么时候用：当改变数据后，要基于更新后的DOM 进行某些操作时，要nextTick所指定的回调函数中执行



#### Vue封装的过渡与动画

作用：在插入、更新或移除DOM元素时，在合适的时候给元素添加样式类名

第三方样式库： [animate.css](https://www.npmjs.com/package/animate.css)

![Transition Diagram](sgg_vue.assets\transition.png)

写法：

- 元素进入样式：

  - v-enter：进入起点
  - v-enter-active：进入过程中
  - v-enter-to：进入终点

- 元素离开样式

  - v-leave：离开起点
  - v-leave-active：离开过程中
  - v-leave-to：离开终点

- 使用

  ```vue
  <transition name="hello">
  	<h1 v-show="isShow">hello</h1>
  </transition>
  ```

- 备注: 有多个元素需要过渡，则需要使用  <transition-group> 且每个元素都要指定key 



#### 插槽

作用：让父组件可以向子组件指定位置插入html结构，也是一种组件间通信的方式，适用于 父组件 ===>子组件

分类：默认插槽、具名插槽、作用域插槽

使用方式：

- 默认插槽

```vue
父组件中：
    <Category>
        <div>html结构1</div>
    </Category>
子组件中：
<template>
	<div>
        <!--定义插槽-->
        <slot>插槽默认内容</slot>
    </div>
</template>
```

- 具名插槽

```vue
父组件中：
    <Category>
        <template slot="center">
             <div>html结构1</div>
        </template>  
        <template v-slot:footer>
             <div>html结构2</div>
        </template>
    </Category>
子组件中：
<template>
	<div>
        <!--定义插槽-->
        <slot name="center">插槽默认内容</slot>
        <slot name="footer">插槽默认内容</slot>
    </div>
</template>
```

- 作用域插槽
  - 数据在组件自身，但根据数据生成的结构需要组件的使用者来决定 （games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定）

```vue
父组件中：
    <Category>
        <template scope="scopeData">
             <!-- 生成的是ul列表 -->
			<ul>
                <li v-for="g in scopeData.games" :key="g">{{g}}</li>
            </ul>
        </template>  
    </Category>
    <Category>
        <template scope="scopeData">
             <!-- 生成的是h4标题 -->
			<h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
        </template>  
    </Category>
子组件中：
    <template>
        <div>
            <!--定义插槽-->
            <slot :games="games"></slot>
        </div>
    </template>
	<script>
		export default {
            name:'Category',
            props:['title'],
            //数据在子组件自身
            data(){
                return{
                    games:['1','2','3','4']
                }
            }
        }
	</script>

```







### 3.vue-cli

#### 安装

`npm i -g @vue/cli`

`vue create xxx`

`npm run serve` 

`vue inspect > output.js` 暴露配置文件的配置 

```javascript
// ./src/main.js 是项目入口文件

import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')

// 为什么是 render
/*
 使用了精简版的vue（模板解析器的vue），不使用完整版的vue，所以用render渲染

*/

```

```vue
<!-- ./src/App.vue 所有组件的父组件 --> 
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

vue.config.js文件可以调整脚手架配置 https://cli.vuejs.org/config/#pages

```js
module.exports = {
  pages: {
    index: {
      // entry for the page
      entry: 'src/main.js',
      // the source template
      template: 'public/index.html',
      // output as dist/index.html
      filename: 'index.html',
      // when using title option,
      // template title tag needs to be <title><%= htmlWebpackPlugin.options.title %></title>
      title: 'Index Page',
      // chunks to include on this page, by default includes
      // extracted common chunks and vendor chunks.
      chunks: ['chunk-vendors', 'chunk-common', 'index']
    },
    // when using the entry-only string format,
    // template is inferred to be `public/subpage.html`
    // and falls back to `public/index.html` if not found.
    // Output filename is inferred to be `subpage.html`.
    subpage: 'src/main.js'
  }
}
```

#### ref属性

被用来给元素或子组件注册引用信息

应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象 （vc）

使用方式

- 打标识： < h1 ref="xxx"> ...</h1> 或 <School ref="xxx"></School>
- 获取：this.$refs.xxx

#### props配置

```javascript
// props功能： 让组件接收外部传过来的数据
/*
1.传递数据：
<Demo name="xxx" />

2.接收数据
	vc实例中的props属性
	(1) props:['name']
	(2) 限制类型
        props:{
            name:Number
        }
    (3) 限制类型、必要性、默认值
    	props:{
    		name:{
    			type:String,
    			required:true,
    			default:'wang'
    		}
    	}
3.props是只读的， Vue底层监测对props的修改，如果进行了修改，就会发出警告，若要修改，则复制props的内容到data中，再修改data的数据

*/


```

#### 混入

```javascript

// mixin混入 
/*
mixin功能：可以把多个组件公用的配置提取成一个混入对象
使用方式：
1. 定义混合
	export const xxx = {
		data(){...}
		methods:{...}
		...
	}
2.混入
	import {xxx} from '../mixin'
    Vue.mixin(xxx)
    mixins:['xxx']
*/
```

#### 插件

```javascript
// 功能：用于增强Vue
// 本质： 包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据

/*
定义插件：
对象.install = functino(Vue,options){
	1.添加全局过滤器
	Vue.filter(...)
	2.添加全局指令
	Vue.directive(...)
	3.配置全局混入
	Vue.minxin(...)
	4.添加实例方法
	Vue.prototype.$myMethod = function(){}
	Vue.prototype.$myProperty = xxxx
}

使用插件： Vue.use()
*/
```

- scoped：样式局部生效，防止冲突

```vue
<style scoped lang="less"> 
    
</style>
```

#### 常规编程

- 组件化编程流程
  - 拆分静态组件：组件按照功能点拆分，命名不要与html元素冲突
  - 实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用
    - 一个组件用：放在组件自身
    - 一些组件用：放在他们共同的父组件上（状态提升）
  - 实现交互
- props适用于：
  - 父组件 ==> 子组件 通信
  - 子组件 ==> 父组件 通信，要求父给子一个函数
- 使用v-model：绑定的值不能是props传过来的值，因为props不能修改
- props传过来的是对象类型，修改一般不会报错，但不推荐这样操作

##### 本地存储

> webStorage

1.存储内容大小一般支持5MB左右

2.浏览器通过 window.sessionStorage和window.localStorage 属性来实现本地存储机制

3.相关API：

- xxxxStorage.setItem('key','value')
- xxxxStorage.getItem('key')
- xxxxStorage.removeItem('key')
- xxxxStorage.clear()

4.备注

sessionStorage存储的内容会随着浏览器窗口的关闭而小时

localStorage存储的内容，需要手动清除才会消失

获取不到值返回null

JSON.parse(null)的结果依然是null

#### 代理

方法一

```javascript
// 在vue.config.js 中添加如下配置：
devServer:{
    proxy:"http://localhost:5000"
}
// 优点：配置简单，请求资源直接发给前端(8080)即可
// 缺点：不能配置多个代理，不能灵活的控制请求是否走代理

```

方法二

```javascript
module.exports = {
    devServer:{
        proxy:{
            '/api1':{ // 匹配所有 以 /api1 开头的路径
                target:'http://localhost:5000', // 代理目标的基础路径
                changeOrigin:true,
                pathRewrite:{'^/api1':''}
            },
             '/api2':{ // 匹配所有 以 /api2 开头的路径
                target:'http://localhost:5000', // 代理目标的基础路径
                changeOrigin:true,
                pathRewrite:{'^/api2':''}
            }
        }
    }
    
}
```

### 4.vuex

> vuex

概念：专门在Vue中实现集中式状态管理的一个Vue插件，对vue应用中多个组件的 **共享状态** 进行集中式的管理（读/写）适用于任意组件中的通信

![vuex](sgg_vue.assets\vuex.png)

安装

npm i vuex@3 vue2中只能使用vuex3

```javascript
// 创建文件 src/store/index.js
// 引入Vue核心库
import Vue from 'vue'
// 引入Vuex
import Vuex from 'vuex'
// 应用插件
Vue.use(Vuex)
// 准备actions对象——响应组件中用户的动作
const actions = {
    jia(context,value){
        context.commit('JIA',value)
    }
}
// 准备mutations对象 —— 修改state中的数据
const mutations = {
    JIA(state,value){
        state.sum += value
    }
}
// 准备state对象——保存具体的数据
const state = {
    sum:0
}
// 准备getters——用于将state中的数据进行加工
const getters = {
    bigSum(state){
        return state.sum*10
    }
}

// 创建并暴露store
export default new Vuex.Store({
    actions,
    mutations,
    state,
    getters
})


// main.js 中引入
import store from './store'

new Vue({
	el:'#app',
    render:h => h(App),
    store,   
})

// Count.vue 中
<template>
  <div>
    <div>{{ $store.state.sum }}</div>
    <div>{{ $store.getters.bigSum }}</div>
    <button @click="increment">加</button>
  </div>
</template>

<script>
export default {
  name: "Count",
  data() {
    return {};
  },
  methods: {
    increment() {
      this.$store.dispatch("jia", 1);
    },
  },
  //生命周期 - 创建完成（访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（访问DOM元素）
  mounted() {},
};
</script>
<style scoped>
/* @import url(); 引入css类 */
</style>
```

#### mapState与mapGetters

```vue
<script>
import {mapState,mapGetters} from 'vuex'
export default {
    name:'Count',
    data(){},
    computed:{
        ...mapState({he:'sum'}), // 映射state中的数据 对象写法
        ...mapState(['sum','school']), // 数组写法
        ...mapGetters(['bigSum'])
    }
}
</script>

```

#### mapActions与mapMutations

```vue
<script>
import {mapActions,mapMutations} from 'vuex'
export default {
    name:'Count',
    data(){},
    methods:{
        ...mapActions({increment:'jia'}), // action映射 $store.dispatch(xxx)
        ...mapMutations({increment:'JIA'}), // mutations映射 $store.commit(xxx)
    }
}
</script>
```

#### vuex模块化

```javascript
// src/store/index.js
// 引入Vue核心库
import Vue from 'vue'
// 引入Vuex
import Vuex from 'vuex'
// 应用插件
Vue.use(Vuex)
// 模块化
const countOptions = {
  namespaced: true, // 开启命名空间
  actions: {
    jia(context, value) {
      context.commit('JIA', value)
    }
  },
  mutations: {
    JIA(state, value) {
      state.sum += value
    }
  },
  state: {
    sum: 0
  },
  getters: {}
}

// 创建并暴露store
export default new Vuex.Store({
  modules: {
    countAbout: countOptions
  }
})
```

```vue
<template>
  <div>
    <div>{{ sum }}</div>
    <button @click="increment(1)">加</button>
    <div>{{ count }}</div>
    <button @click="increment2">加</button>
  </div>
</template>

<script>
import { mapState, mapActions } from "vuex";
export default {
  name: "Count",
  data() {
    return {
      count: 0,
    };
  },
  computed: {
    ...mapState("countAbout", ["sum"]),
  },
  methods: {
    increment2() {
      this.count++;
    },
    ...mapActions("countAbout", { increment: "jia" }),
  },
  //生命周期 - 创建完成（访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（访问DOM元素）
  mounted() {},
};
</script>
```

开启命名空间

```javascript
// 组件读取state
this.$store.countAbout.list
...mapState('countAbout',['sum','school','subject'])

// 读取getters
this.$store.getters['countAbout/bigSum']
...mapGetters('countAbout',['bigSum'])

// 调用dispatch
this.$store.dispatch('countAbout/add',person)
...mapActions('countAbout',{incrementOdd:'jiaOdd'})

// 调用commit
this.$store.commit('countAbout/ADD',person)
...mapMutations('countAbout',{increment:'JIA'})
```



### 5.vue-router

### 6.element-ui

### 7.vue3

