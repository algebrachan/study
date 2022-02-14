# vue.js

```init
更新时间:2021年4月29日09:07:18
说明:vue官方文档分析
```

## 1.基本使用

- [官方文档vue3.x](https://v3.cn.vuejs.org/)

### 1.1 安装

- CDN包

  ```html
  <script src="https://unpkg.com/vue@next"></script>
  ```

- npm

  ```bash
  npm install vue@next
  ```

- 命令行工具CLI

  ```bash
  npm install -g @vue/cli
  # 创建项目
  vue create <project-name>
  # 图形化界面
  vue ui
  # 启动
  npm run serve
  # 打包
  npm run build
  # 启动打包工程
  serve -s dist
  
  # 快速原型开发 需安装如下
  npm install -g @vue/cli-service-global
  serve [options] [entry]
  Options:
    -o, --open  #打开浏览器
    -c, --copy  #将本地 URL 复制到剪切板
    -h, --help  #输出用法信息
  
  Usage: build [options] [entry]
  
  # 在生产环境模式下零配置构建一个 .js 或 .vue 文件
  Options:
    -t, --target <target>  #构建目标 (app | lib | wc | wc-async, 默认值：app)
    -n, --name <name>      #库的名字或 Web Components 组件的名字 (默认值：入口文件名)
    -d, --dest <dir>       #输出目录 (默认值：dist)
    -h, --help             #输出用法信息
  ```

- Vite

  - [ ] 使用这个工具构建的时候一直报错，未解决

  ```bash
  npm init @vitejs/app <project-name>
  cd <project-name>
  npm install
  npm run dev # 启动报错
  ```

### 1.2 介绍

- 渐进式框架：一开始不需要完全掌握全部功能，后续逐步增加功能；对比react和angular都有入门门槛，vue即插即用

- 新建代码片段

  ```json
  # 首选项 -> 用户代码片段 -> (新建代码片段取名vue.json)
  {
    "Print to console": {
      "prefix": "vue",  
      "body": [
        "<!-- $1 -->",
        "<template>",
        "<div></div>",
        "</template>",
        "",
        "<script>",
        "export default {",
        "data() {",
        "return {",
        "",
        "}",
        "},",
        "//生命周期 - 创建完成（访问当前this实例）",
        "created() {",
        "",
        "},",
        "//生命周期 - 挂载完成（访问DOM元素）",
        "mounted() {",
        "",
        "}",
        "}",
        "</script>",
        "<style scoped>",
        "/* @import url(); 引入css类 */",
        "$4",
        "</style>"
      ],
      "description": "Log output to console"
    }
  }
  # vue 文件中使用 vue tab键使用
  ```


### 1.3 应用&组件实例

- 创建应用实例

  ```javascript
  // 注册全局组件
  const app = Vue.createApp({
      /*选项*/
  })
  app.component('SearchInput', SearchInputComponent)
  app.directive('focus', FocusDirective)
  app.use(LocalePlugin)
  // 允许链式
  Vue.createApp({})
    .component('SearchInput', SearchInputComponent)
    .directive('focus', FocusDirective)
    .use(LocalePlugin)
  // 挂载
  const RootComponent = { 
    data() {
      return { count: 4 }
    }
  }
  const app = Vue.createApp(RootComponent)
  const vm = app.mount('#app')
  ```

- 生命周期(常用)

  - created:实例创建之后被执行代码
  - mounted:挂载之后
  - updated:更新之后
  - unmounted:销毁之后

  ![image-20210429134653161](\vue.assets\image-20210429134653161.png)

### 1.4 模板语法

```html
<!-- 数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值 -->
<span v-once>Message: {{ msg }}</span>
<!-- v-once执行一次插值 -->
<!-- v-html输出真正的html 不加的话显示字符串 -->


<!-- v-bind绑定html 属性 -->
<a v-bind:href="url"> ... </a>
<a :href="url"> ... </a>
<!-- 动态绑定 -->
<a v-bind:[attributeName]="url"> ... </a>
<!-- 绑定事件 -->
<a v-on:click="doSomething"> ... </a> 

<form v-on:submit.prevent="onSubmit">...</form>

<!-- 完整语法 -->
<a v-bind:href="url"> ... </a>

<!-- 缩写 -->
<a :href="url"> ... </a>

<!-- 动态参数的缩写 -->
<a :[key]="url"> ... </a>

<!-- 完整语法 -->
<a v-on:click="doSomething"> ... </a>

<!-- 缩写 -->
<a @click="doSomething"> ... </a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```



### 1.5 Data Property和方法

```javascript
const app = Vue.createApp({
  data() {
    return { count: 4 }
  }
})
const vm = app.mount('#app')

console.log(vm.$data.count) // => 4
console.log(vm.count)       // => 4
```

- 防抖和节流

```javascript
app.component('save-button', {
  created() {
    // 用 Lodash 的防抖函数
    this.debouncedClick = _.debounce(this.click, 500)
  },
  unmounted() {
    // 移除组件时，取消定时器
    this.debouncedClick.cancel()
  },
  methods: {
    click() {
      // ... 响应点击 ...
    }
  },
  template: `
    <button @click="debouncedClick">
      Save
    </button>
  `
})
```

### 1.6 计算属性和侦听器

- 计算属性: computed

  ```html
  <div id="computed-basics">
    <p>Has published books:</p>
    <span>{{ publishedBooksMessage }}</span>
  </div>
  ```

  ```javascript
  Vue.createApp({
    data() {
      return {
        author: {
          name: 'John Doe',
          books: [
            'Vue 2 - Advanced Guide',
            'Vue 3 - Basic Guide',
            'Vue 4 - The Mystery'
          ]
        }
      }
    },
    computed: {
      // 计算属性的 getter
      publishedBooksMessage() {
        // `this` 指向 vm 实例
        return this.author.books.length > 0 ? 'Yes' : 'No'
      }
    }
  }).mount('#computed-basics')
  ```

- 计算属性与方法的区别：
  - 方法每次调用都是重新计算，浪费资源
  - 计算属性，只有在响应式依赖发生改变时才会重新求值
  
- 计算属性的Setter

  ```javascript
  computed: {
    fullName: {
      // getter
      get() {
        return this.firstName + ' ' + this.lastName
      },
      // setter
      set(newValue) {
        const names = newValue.split(' ')
        this.firstName = names[0]
        this.lastName = names[names.length - 1]
      }
    }
  }
  ```

- 侦听器:watch

  ```html
  <div id="watch-example">
    <p>
      Ask a yes/no question:
      <input v-model="question" />
    </p>
    <p>{{ answer }}</p>
  </div>
  ```

  ```javascript
  const watchExampleVM = Vue.createApp({
      data() {
        return {
          question: '',
          answer: 'Questions usually contain a question mark. ;-)'
        }
      },
      watch: {
        // whenever question changes, this function will run
        question(newQuestion, oldQuestion) {
          if (newQuestion.indexOf('?') > -1) {
            this.getAnswer()
          }
        }
      },
      methods: {
        getAnswer() {
          this.answer = 'Thinking...'
          axios
            .get('https://yesno.wtf/api')
            .then(response => {
              this.answer = response.data.answer
            })
            .catch(error => {
              this.answer = 'Error! Could not reach the API. ' + error
            })
        }
      }
    }).mount('#watch-example')
  ```

- 尽量使用侦听属性



### 1.7 Class与Style绑定

class和style都可以用v-bind处理他们

- 绑定HTML Class Style

  ```html
  <div :class="{ active: isActive }"></div>
  <div
    class="static"
    :class="{ active: isActive, 'text-danger': hasError }"
  ></div>
  
  <div :class="classObject"></div>
  
  <!-- 使用三元表达式 -->
  <div :class="[isActive ? activeClass : '', errorClass]"></div>
  
  <!-- 绑定内联样式 -->
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  ```

  ```javascript
  data() {
    return {
      isActive: true,
      hasError: false,
      error:null
      classObject: {
        active: true,
        'text-danger': false
      }
    }
  }
  // 也可使用计算属性
  computed: {
    classObject() {
      return {
        active: this.isActive && !this.error,
        'text-danger': this.error && this.error.type === 'fatal'
      }
    }
  }
  ```

### 1.8 条件渲染（列表）

- v-if、v-else、v-else-if

- v-show

  - 如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好

- v-for

  - 遍历list和object

  ```html
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
  
  <ul id="array-rendering">
    <li v-for="item in items">
      {{ item.message }}
    </li>
  </ul>
  ```

### 1.9 事件处理

- v-on、@
- 事件修饰符
  - .stop
  - .prevent
  - .capture
  - .self
  - .once
  - .passive
- 按键修饰符：@keyup
  - .enter
  - .tab
  - .delete
  - .esc
  - .space
  - .up
  - .down
  - .left
  - .right
  - .ctrl
  - .alt
  - .shift
  - .meta

### 1.10 表单输入绑定

- 基本用法

  ```html
  <!-- 文本 -->
  <input v-model="message" placeholder="edit me" />
  <p>Message is: {{ message }}</p>
  
  <!-- 多行文本 -->
  <span>Multiline message is:</span>
  <p style="white-space: pre-line;">{{ message }}</p>
  <br />
  <textarea v-model="message" placeholder="add multiple lines"></textarea>
  
  <!-- 复选框 -->
  <input type="checkbox" id="checkbox" v-model="checked" />
  <label for="checkbox">{{ checked }}</label>
  
  <!-- 单选框 -->
  div id="v-model-radiobutton">
    <input type="radio" id="one" value="One" v-model="picked" />
    <label for="one">One</label>
    <br />
    <input type="radio" id="two" value="Two" v-model="picked" />
    <label for="two">Two</label>
    <br />
    <span>Picked: {{ picked }}</span>
  </div>
  
  <!-- 选择框 -->
  <div id="v-model-select" class="demo">
    <select v-model="selected">
      <option disabled value="">Please select one</option>
      <option>A</option>
      <option>B</option>
      <option>C</option>
    </select>
    <span>Selected: {{ selected }}</span>
  </div>
  
  ```

- v-model修饰符

  - .lazy: 在change而非input时更新
  - .number: 输入转为数值
  - .trim: 过滤收尾空白字符串



## 2.深入组件

- 组件注册

  ```javascript
  const app = Vue.createApp({...})
  // 组件名建议使用 kebab-case(短横线分隔命名) 风格 
  // 全局注册                      
  app.component('my-component-name', {
    /* ... */
  })
  // 使用
  <my-component-name></my-component-name>
  
  // 局部注册
  const ComponentA = {
    /* ... */
  }
  const ComponentB = {
    /* ... */
  }
  const ComponentC = {
    /* ... */
  }
  const app = Vue.createApp({
    components: {
      'component-a': ComponentA,
      'component-b': ComponentB
    }
  })
  
  import ComponentA from './ComponentA'
  import ComponentC from './ComponentC'
  export default {
    components: {
      ComponentA,
      ComponentC
    }
    // ...
  }
  ```

- Props

  ```javascript
  app.component('my-component', {
    props: {
      // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
      propA: Number,
      // 多个可能的类型
      propB: [String, Number],
      // 必填的字符串
      propC: {
        type: String,
        required: true
      },
      // 带有默认值的数字
      propD: {
        type: Number,
        default: 100
      },
      // 带有默认值的对象
      propE: {
        type: Object,
        // 对象或数组默认值必须从一个工厂函数获取
        default: function() {
          return { message: 'hello' }
        }
      },
      // 自定义验证函数
      propF: {
        validator: function(value) {
          // 这个值必须匹配下列字符串中的一个
          return ['success', 'warning', 'danger'].indexOf(value) !== -1
        }
      },
      // 具有默认值的函数
      propG: {
        type: Function,
        // 与对象或数组默认值不同，这不是一个工厂函数 —— 这是一个用作默认值的函数
        default: function() {
          return 'Default function'
        }
      }
    }
  })
  ```

- 自定义事件

  ```javascript
  app.component('custom-form', {
    // 定义事件
    emits: {
      // 没有验证
      click: null,
  
      // 验证submit 事件
      submit: ({ email, password }) => {
        if (email && password) {
          return true
        } else {
          console.warn('Invalid submit event payload!')
          return false
        }
      }
    },
    methods: {
      submitForm() {
        this.$emit('submit', { email, password })
      }
    }
  })
  ```

- 插槽

  ```html
  <todo-button>
    Add todo
  </todo-button>
  <!-- todo-button 组件模板 -->
  <button class="btn-primary">
    <slot>Submit</slot>
  </button>
  <!-- 渲染 HTML -->
  <button class="btn-primary">
    Add todo
  </button>
  
  <!-- 具名插槽 -->
  <div class="container">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
  <!-- 使用 -->
  <base-layout>
    <template v-slot:header>
      <h1>Here might be a page title</h1>
    </template>
    <template v-slot:default>
      <p>A paragraph for the main content.</p>
      <p>And another one.</p>
    </template>
  
    <template v-slot:footer>
      <p>Here's some contact info</p>
    </template>
  </base-layout>
  
  <!-- 作用域插槽 -->
  <ul>
    <li v-for="( item, index ) in items">
      <slot :item="item" :index="index" :another-attribute="anotherAttribute"></slot>
    </li>
  </ul>
  <todo-list>
    <template v-slot:default="slotProps">
      <i class="fas fa-check"></i>
      <span class="green">{{ slotProps.item }}</span>
    </template>
  </todo-list>
  
  ```

- Provide/Inject: 子孙组件数据传递

  ```javascript
  const app = Vue.createApp({})
  
  app.component('todo-list', {
    data() {
      return {
        todos: ['Feed a cat', 'Buy tickets']
      }
    },
    provide: {
      user: 'John Doe'，
      todoLength: Vue.computed(() => this.todos.length)
    },
    template: `
      <div>
        {{ todos.length }}
        <!-- 模板的其余部分 -->
      </div>
    `
  })
  
  app.component('todo-list-statistics', {
    inject: ['user'],
    created() {
      console.log(`Injected property: ${this.user}`) // > 注入 property: John Doe
    }
  })
  //provide 一些组件的实例 不起作用
  
  ```
```
  
- 动态组件&异步组件

  keep-alive 切换的时候保留状态

  ```html
  <!-- 失活的组件将会被缓存！-->
  <keep-alive>
    <component :is="currentTabComponent"></component>
  </keep-alive>
```

  ```javascript
  // 异步组件 只在需要的时候才从服务器加载一个模块
  const { createApp, defineAsyncComponent } = Vue
  
  const app = createApp({})
  
  const AsyncComp = defineAsyncComponent(
    () =>
      new Promise((resolve, reject) => {
        resolve({
          template: '<div>I am async!</div>'
        })
      })
  )
  
  app.component('async-example', AsyncComp)
  // 异步组件可与Suspense一起使用
  ```

- 模板引用

  ```javascript
  // 直接访问dom 使用ref属性
  const app = Vue.createApp({})
  
  app.component('base-input', {
    template: `
      <input ref="input" />
    `,
    methods: {
      focusInput() {
        this.$refs.input.focus()
      }
    },
    mounted() {
      this.focusInput()
    }
  })
  ```

- 处理边界情况

  - 强制更新: $forceUpdate
  - 低静态组件与v-once：对只加载一次的组件进行缓存

  ```javascript
  app.component('terms-of-service', {
    template: `
      <div v-once>
        <h1>Terms of Service</h1>
        ... a lot of static content ...
      </div>
    `
  })
  ```



## 3.过渡&动画

### 3.1 概述

- 基于class的动画和过渡

  ```html
  <div id="demo">
    Push this button to do something you shouldn't be doing:<br />
  
    <div :class="{ shake: noActivated }">
      <button @click="noActivated = true">Click me</button>
      <span v-if="noActivated">Oh no!</span>
    </div>
  </div>
  ```

  ```css
  .shake {
    animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
    transform: translate3d(0, 0, 0);
    backface-visibility: hidden;
    perspective: 1000px;
  }
  
  @keyframes shake {
    10%,
    90% {
      transform: translate3d(-1px, 0, 0);
    }
  
    20%,
    80% {
      transform: translate3d(2px, 0, 0);
    }
  
    30%,
    50%,
    70% {
      transform: translate3d(-4px, 0, 0);
    }
  
    40%,
    60% {
      transform: translate3d(4px, 0, 0);
    }
  }
  ```

  ```javascript
  const Demo = {
    data() {
      return {
        noActivated: false
      }
    }
  }
  
  Vue.createApp(Demo).mount('#demo')
  ```

- 过渡与style绑定

  ```html
  <div id="demo">
    <div
      @mousemove="xCoordinate"
      :style="{ backgroundColor: `hsl(${x}, 80%, 50%)` }"
      class="movearea"
    >
      <h3>Move your mouse across the screen...</h3>
      <p>x: {{x}}</p>
    </div>
  </div>
  ```

  ```css
  .movearea {
    transition: 0.2s background-color ease;
  }
  ```

  ```javascript
  const Demo = {
    data() {
      return {
        x: 0
      }
    },
    methods: {
      xCoordinate(e) {
        this.x = e.clientX
      }
    }
  }
  
  Vue.createApp(Demo).mount('#demo')
  ```

- Easing

  ```css
  .button {
    background: #1b8f5a;
    /* 应用于初始状态，因此此转换将应用于返回状态 */
    transition: background 0.25s ease-in;
  }
  
  .button:hover {
    background: #3eaf7c;
    /* 应用于悬停状态，因此在触发悬停时将应用此过渡 */
    transition: background 0.35s ease-out;
  }
  ```

  

### 3.2 进入&离开

- 典例

  ```html
  <div id="demo">
    <button @click="show = !show">
      Toggle
    </button>
  
    <transition name="fade">
      <p v-if="show">hello</p>
    </transition>
  </div>
  ```

  ```javascript
  const Demo = {
    data() {
      return {
        show: true
      }
    }
  }
  
  Vue.createApp(Demo).mount('#demo')
  ```

  ```css
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.5s ease;
  }
  
  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
  ```

- 过渡class

  - `v-enter-from`: 进入开始
  - `v-enter-active`: 进入过渡生效时
  - `v-enter-to`: 进入结束
  - `v-leave-from`: 离开开始
  - `v-leave-active`: 离开过渡生效时
  - `v-leave-to`: 离开结束

- JavaScript钩子

  ```html
  <transition
    @before-enter="beforeEnter"
    @enter="enter"
    @after-enter="afterEnter"
    @enter-cancelled="enterCancelled"
    @before-leave="beforeLeave"
    @leave="leave"
    @after-leave="afterLeave"
    @leave-cancelled="leaveCancelled"
    :css="false"
  >
    <!-- ... -->
  </transition>
  ```

  ```javascript
  // ...
  methods: {
    // --------
    // ENTERING
    // --------
  
    beforeEnter(el) {
      // ...
    },
    // 当与 CSS 结合使用时
    // 回调函数 done 是可选的
    enter(el, done) {
      // ...
      done()
    },
    afterEnter(el) {
      // ...
    },
    enterCancelled(el) {
      // ...
    },
  
    // --------
    // 离开时
    // --------
  
    beforeLeave(el) {
      // ...
    },
    // 当与 CSS 结合使用时
    // 回调函数 done 是可选的
    leave(el, done) {
      // ...
      done()
    },
    afterLeave(el) {
      // ...
    },
    // leaveCancelled 只用于 v-show 中
    leaveCancelled(el) {
      // ...
    }
  }
  ```

- 多元素过渡

  ```html
  <transition>
    <button :key="docState">
      {{ buttonMessage }}
    </button>
  </transition>
  ```

  ```javascript
  // ...
  computed: {
    buttonMessage() {
      switch (this.docState) {
        case 'saved': return 'Edit'
        case 'edited': return 'Save'
        case 'editing': return 'Cancel'
      }
    }
  }
  ```

- 多个组件之间的过渡

  ```html
  <div id="demo">
    <input v-model="view" type="radio" value="v-a" id="a"><label for="a">A</label>
    <input v-model="view" type="radio" value="v-b" id="b"><label for="b">B</label>
    <transition name="component-fade" mode="out-in">
      <component :is="view"></component>
    </transition>
  </div>
  ```

  ```javascript
  const Demo = {
    data() {
      return {
        view: 'v-a'
      }
    },
    components: {
      'v-a': {
        template: '<div>Component A</div>'
      },
      'v-b': {
        template: '<div>Component B</div>'
      }
    }
  }
  
  Vue.createApp(Demo).mount('#demo')
  ```

  ```css
  .component-fade-enter-active,
  .component-fade-leave-active {
    transition: opacity 0.3s ease;
  }
  
  .component-fade-enter-from,
  .component-fade-leave-to {
    opacity: 0;
  }
  ```

### 3.3 列表过渡

- 典例 使用  <transition-group>

  ```html
  <div id="list-demo">
    <button @click="add">Add</button>
    <button @click="remove">Remove</button>
    <transition-group name="list" tag="p">
      <span v-for="item in items" :key="item" class="list-item">
        {{ item }}
      </span>
    </transition-group>
  </div>
  ```

  ```javascript
  const Demo = {
    data() {
      return {
        items: [1, 2, 3, 4, 5, 6, 7, 8, 9],
        nextNum: 10
      }
    },
    methods: {
      randomIndex() {
        return Math.floor(Math.random() * this.items.length)
      },
      add() {
        this.items.splice(this.randomIndex(), 0, this.nextNum++)
      },
      remove() {
        this.items.splice(this.randomIndex(), 1)
      }
    }
  }
  
  Vue.createApp(Demo).mount('#list-demo')
  ```

  ```css
  .list-item {
    display: inline-block;
    margin-right: 10px;
  }
  .list-enter-active,
  .list-leave-active {
    transition: all 1s ease;
  }
  .list-enter-from,
  .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }
  ```

- 列表移动过渡

  ```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.15/lodash.min.js"></script>
  
  <div id="flip-list-demo">
    <button @click="shuffle">Shuffle</button>
    <transition-group name="flip-list" tag="ul">
      <li v-for="item in items" :key="item">
        {{ item }}
      </li>
    </transition-group>
  </div>
  ```

  ```javascript
  const Demo = {
    data() {
      return {
        items: [1, 2, 3, 4, 5, 6, 7, 8, 9]
      }
    },
    methods: {
      shuffle() {
        this.items = _.shuffle(this.items)
      }
    }
  }
  
  Vue.createApp(Demo).mount('#flip-list-demo')
  ```

  ```css
  .flip-list-move {
    transition: transform 0.8s ease;
  }
  ```

- 列表交错过渡

  ```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.3.4/gsap.min.js"></script>
  
  <div id="demo">
    <input v-model="query" />
    <transition-group
      name="staggered-fade"
      tag="ul"
      :css="false"
      @before-enter="beforeEnter"
      @enter="enter"
      @leave="leave"
    >
      <li
        v-for="(item, index) in computedList"
        :key="item.msg"
        :data-index="index"
      >
        {{ item.msg }}
      </li>
    </transition-group>
  </div>
  ```

  ```javascript
  const Demo = {
    data() {
      return {
        query: '',
        list: [
          { msg: 'Bruce Lee' },
          { msg: 'Jackie Chan' },
          { msg: 'Chuck Norris' },
          { msg: 'Jet Li' },
          { msg: 'Kung Fury' }
        ]
      }
    },
    computed: {
      computedList() {
        var vm = this
        return this.list.filter(item => {
          return item.msg.toLowerCase().indexOf(vm.query.toLowerCase()) !== -1
        })
      }
    },
    methods: {
      beforeEnter(el) {
        el.style.opacity = 0
        el.style.height = 0
      },
      enter(el, done) {
        gsap.to(el, {
          opacity: 1,
          height: '1.6em',
          delay: el.dataset.index * 0.15,
          onComplete: done
        })
      },
      leave(el, done) {
        gsap.to(el, {
          opacity: 0,
          height: 0,
          delay: el.dataset.index * 0.15,
          onComplete: done
        })
      }
    }
  }
  
  Vue.createApp(Demo).mount('#demo')
  ```

### 3.4 状态过渡

- 状态动画与侦听器

  ```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.2.4/gsap.min.js"></script>
  
  <div id="animated-number-demo">
    <input v-model.number="number" type="number" step="20" />
    <p>{{ animatedNumber }}</p>
  </div>
  ```

  ```javascript
  const Demo = {
    data() {
      return {
        number: 0,
        tweenedNumber: 0
      }
    },
    computed: {
      animatedNumber() {
        return this.tweenedNumber.toFixed(0)
      }
    },
    watch: {
      number(newValue) {
        gsap.to(this.$data, { duration: 0.5, tweenedNumber: newValue })
      }
    }
  }
  
  Vue.createApp(Demo).mount('#animated-number-demo')
  ```

- 把过渡放到组件里

  ```html
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.2.4/gsap.min.js"></script>
  
  <div id="app">
    <input v-model.number="firstNumber" type="number" step="20" /> +
    <input v-model.number="secondNumber" type="number" step="20" /> = {{ result }}
    <p>
      <animated-integer :value="firstNumber"></animated-integer> +
      <animated-integer :value="secondNumber"></animated-integer> =
      <animated-integer :value="result"></animated-integer>
    </p>
  </div>
  ```

  ```javascript
  const app = Vue.createApp({
    data() {
      return {
        firstNumber: 20,
        secondNumber: 40
      }
    },
    computed: {
      result() {
        return this.firstNumber + this.secondNumber
      }
    }
  })
  
  app.component('animated-integer', {
    template: '<span>{{ fullValue }}</span>',
    props: {
      value: {
        type: Number,
        required: true
      }
    },
    data() {
      return {
        tweeningValue: 0
      }
    },
    computed: {
      fullValue() {
        return Math.floor(this.tweeningValue)
      }
    },
    methods: {
      tween(newValue, oldValue) {
        gsap.to(this.$data, {
          duration: 0.5,
          tweeningValue: newValue,
          ease: 'sine'
        })
      }
    },
    watch: {
      value(newValue, oldValue) {
        this.tween(newValue, oldValue)
      }
    },
    mounted() {
      this.tween(this.value, 0)
    }
  })
  
  app.mount('#app')
  ```



## 4.可复用&组合

### 4.1组合式API基础

- 典例

  ```javascript
  // src/components/UserRepositories.vue
  import { fetchUserRepositories } from '@/api/repositories'
  import { ref } from 'vue'
  
  export default {
    components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
    props: {
      user: {
        type: String,
        required: true
      }
    },
    setup (props) {
      // 使用 `toRefs` 创建对prop的 `user` property 的响应式引用
   	const { user } = toRefs(props)
      const repositories = ref([]) // 响应式 初始值为[]
      const getUserRepositories = async () => {
         // 更新 `prop.user` 到 `user.value` 访问引用值
        repositories.value = await fetchUserRepositories(user.value)
      }
  	onMounted(getUserRepositories) // 在 `mounted` 时调用 `getUserRepositories`
      // 在用户 prop 的响应式引用上设置一个侦听器
    	watch(user, getUserRepositories)
      return {
        repositories,
        getUserRepositories
      }
    },
    data () {
      return {
        filters: { ... }, // 3
        searchQuery: '' // 2
      }
    },
    computed: {
      filteredRepositories () { ... }, // 3
      repositoriesMatchingSearchQuery () { ... }, // 2
    },
    watch: {
      user: 'getUserRepositories' // 1
    },
    methods: {
      updateFilters () { ... }, // 3
    },
    mounted () {
      this.getUserRepositories() // 1
    }
  }
  ```

- setup

  - `props`
  - `context`

  ```javascript
  // setup中的props是响应式的，传入新的prop，它将被更新
  // 因为 props 是响应式的，你不能使用 ES6 解构，因为它会消除 prop 的响应性
  import { toRefs } from 'vue'
  export default {
      props:{
          title:String
      },
      setup(props){
          const { title } = toRefs(props，'title')// 解构使用 toRefs 若没有title则需要toRef替代它
          console.log(title.value)
      }
  }
  
  //context 是一个普通的 JavaScript 对象，它暴露三个组件的 property：
  export default {
    setup(props, context) {
      // Attribute (非响应式对象)
      console.log(context.attrs)
  
      // 插槽 (非响应式对象)
      console.log(context.slots)
  
      // 触发事件 (方法)
      console.log(context.emit)
    }
  }
  export default {
    setup(props, { attrs, slots, emit }) { // context非响应式，可以安全的解构 避免对子属性的解构
      ...
    }
  }
  // setup内部不能使用this
  ```

- 生命周期钩子：如何在setup()内部调用

  | 选项式 API        | Hook inside `setup` |
  | ----------------- | ------------------- |
  | `beforeCreate`    | Not needed*         |
  | `created`         | Not needed*         |
  | `beforeMount`     | `onBeforeMount`     |
  | `mounted`         | `onMounted`         |
  | `beforeUpdate`    | `onBeforeUpdate`    |
  | `updated`         | `onUpdated`         |
  | `beforeUnmount`   | `onBeforeUnmount`   |
  | `unmounted`       | `onUnmounted`       |
  | `errorCaptured`   | `onErrorCaptured`   |
  | `renderTracked`   | `onRenderTracked`   |
  | `renderTriggered` | `onRenderTriggered` |

- Provide/Inject

  ```vue
  <!-- 使用Provide -->
  <!-- src/components/MyMap.vue -->
  <template>
    <MyMarker />
  </template>
  
  <script>
  import { provide, reactive, readonly, ref } from 'vue'
  import MyMarker from './MyMarker.vue
  
  export default {
    components: {
      MyMarker
    },
    setup() {
      const location = ref('North Pole') // 响应式
      const geolocation = reactive({
        longitude: 90,
        latitude: 135
      })
  
      const updateLocation = () => { // inject 内部更改
        location.value = 'South Pole'
      }
  
      provide('location', readonly(location)) // inject只读
      provide('geolocation', readonly(geolocation))
      provide('updateLocation', updateLocation)
    }
    methods: { 
      updateLocation() { // 修改响应式 property
        this.location = 'South Pole'
      }
    }
  }
  </script>
  ```

  ```vue
  <!-- 使用Inject -->
  <!-- src/components/MyMarker.vue -->
  <script>
  import { inject } from 'vue'
  
  export default {
    setup() {
      const userLocation = inject('location', 'The Universe')
      const userGeolocation = inject('geolocation')
      const updateUserLocation = inject('updateLocation')
  
      return {
        userLocation,
        userGeolocation,
        updateUserLocation
      }
    }
  }
  </script>
  ```

  



