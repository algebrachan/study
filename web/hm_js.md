JavaScript

## 1.基础语法

### 1.1 初识

- 浏览器分成两个部分：渲染引擎和JS引擎，js引擎逐行运行脚本语言
- js组成：ECMAScript语法、DOM、BOM
  - console.log() prompt() alert()
- 数据类型：js的变量数据类型，只有程序运行过程中根据等号右边的值来确定
  - 简答数据类型：
  - Number
    - 八进制：var 010 = 8
    - 十六进制：0x
    - 最大最小值：MAX_VALUE  MIN_VALUE
    - infinity 无穷 NaN不是数字 isNan()
  - String：
    - 字符转义符号
  - Boolean：
    - true的运算为 1 false为 0
  - Typeof：检测是什么数据类型
  - 数据类型转换：
    - 转字符串：toString()、String()、加号拼接字符串
    - 转数字型：parseInt ...

### 1.2 运算符

- 算术运算符：+、-、*、/ 、% 浮点数不要直接计算
- 前置递增、后置递增：++i、i++
- ==会进行类型转换，===全等
- && 0 '' null undefined NaN 为 false其余为真：表达式1&&表达式2，若1为真则返回2，若1为假则返回1



### 1.3 流程控制

- if语句、switch case、三目表达式
- for：for(初始化变量；条件表达式；操作表达式){ 循环体 }
- while：while(条件表达式){ 循环体 }
- do while：do{循环体}while(条件表达式){ 进一步循环，再判断 }
- continue，退出本次循环
- break，跳出整个循环



### 1.4 数组

- 创建数组的方式：new；字面量；var arr = new Array()；var arr = []
- 数组扩容：
  - 修改length的长度
  - 新增数组元素
- [ ] 各种排序算法？



### 1.5 函数

- 封装公用代码

- 形参、实参；形参不传参数默认 undefined

- 函数没有return 则返回 undefined

- arguments的使用，传递这个arguments代表所有的实参

  ```javascript
  // 翻转数组
  function reverse(arr){
      var newArr = []
      for(var i = arr.length-1;i>=0;i--){
          newArr[newArr.length] = arr[i];
          return newArr;
      }
  }
  // 冒泡排序
  function sort(arr){
      for(var i=0;i<arr.length-1;i++){
          for(var j=0;j<arr.length-i;j++){
              if(arr[j]>arr[j+1]){
                  //交换
                  var temp = arr[j];
                  arr[j] = arr[j+1];
                  arr[j+1] = temp;
              }
          }
      }
  }
  ```

- 函数声明方式：function fn(){} ,var 变量名 = function(){}

### 1.6 作用域

- 全局作用域：整个script标签，或者是一个单独的js文件
- 局部作用域：在函数内部
- 块级作用域：{}
- 全局变量：在全局作用域下的变量 在全局下都可以使用，在函数内部未声明的变量
- 局部变量：在局部作用域下的变量，比较节约内存资源
- 作用域链：内部函数 访问外部函数的变量，采取的是链式查找方式决定取哪个值
- **预解析：**
  - 预解析会把js里面所有的var还有function提升到当前作用域的最前面，只提升变量声明，不提升赋值
  - 变量提升，函数提升，把声明提升到当前作用域的前面，其他不调用

### 1.7 JavaScript自定义对象

- 创建对象的方法：直接赋值

  ```javascript
  //
  var obj = {
  	uname:'',
  	age:20,
  	sex:'男',
  	sayHi:function(){console.log('123')}
  }
  调用对象
  obj.uname;
  obj['age'];
  obj.sayHi();
  
  var obj = new Object();
  obj.uname = 'wc';
  追加属性
  构造函数：封装对象
  function 构造函数名(){
  	this.属性 = 值;
  	this.方法 = function(){}
  }
  new 构造函数名();
  构造函数名需要大写
  ```

- 使用构造函数 新建对象 称为对象的实例化

  - 在内存中创建一个空对象
  - this指向这个对象/ this指向调用他的对象
  - 执行构造函数里的代码，添加属性和方法
  - 返回这个对象

- for in 遍历对象

  ```javascript
  for(var k in obj){
    console.log(k) // k变量输出 得到属性名
    console.log(obj[k]);// obj[k] 输出得到 属性值
  }
  ```

### 1.8 JavaScript内置对象

- [MDN说明文档](https://developer.mozilla.org/zh-CN/)

- Math

- Date是构造函数，必须new

  ```javascript
  var date = +new Date()
  Date.now()
  ```

- Array()

  | 方法                    | 说明                 |
  | ----------------------- | -------------------- |
  | instanceof              | 用来检测是否为数组   |
  | push                    | 在末尾添加元素       |
  | unshift                 | 在开头添加元素       |
  | pop                     | 删除数组最后一个元素 |
  | shift                   | 删除数组第一个元素   |
  | arr.reverse()           | 反转数组             |
  | arr.sort()              | 数组排序             |
  | indexOf();lastIndexOf() |                      |
  | concat()                | 连接多个数组         |
  | slice()                 | 截取                 |

  - 数组去重：遍历旧数组，放到新数组中去重

    ```javascript
    function unique(arr){
    	var newArr =[];
    	for(var i=0;i<arr.length;i++){
    		if(newArr.indexOf(arr[i]===-1)){
    			newArr.push(arr[i]);
    		}
    	}
    	return newArr;
    }
    // 数组转字符串
    toString();
    arr.join('分隔符');
    ```

- String：基本包装类型

  - 字符串不可变，每次操作都会返回一个新字符串
  - indexOf()  str.charAt(index)  replace()  split('')

- 简单数据类型：存放在栈里面，直接开辟一个存储空间 存放的是值

- 复杂数据类型：在栈里存地址，真正的对象实例在堆空间

- JavaScript浏览器对象

### 1.9 web Apis和js基础关联性

- DOM 和 BOM
  - 文档：document
  - 元素：<body/>
  - 节点：文本
- 获取元素
- 事件基础：
  - 获取事件源
  - 绑定事件，注册事件
  - 添加事件处理程序
    - element.innerText 不识别html标签
    - element.innerHTML 显示标签，保存空格换行
    - document.body.style.backgroundImage
  - 操作元素
    - element.属性 获取属性值 获取内置属性  element.属性 = '值'
    - element.getAttribute('属性') 获取自定义属性 element.setAttribute('属性','值')
  - H5自定义属性规范，data-index，h5获新属性的方法 div.dataset.index

- 节点操作
  - 元素节点，文本节点
    - parentNode.childNodes 获取子节点
    - parentNode.childern 获取子元素
    - appendChild(child) 创建元素节点，添加节点
    - xx.removeChild(el)
    - node.cloneNode() 克隆节点
  - 三种动态创建元素的区别
    - document.write()
    - document.innerHTML 用数组形式拼接
    - document.createElement()

### 1.10 事件

- 注册事件：绑定事件

- 删除事件：解绑事件

- DOM事件流：事件发生时 在元素节点之间按照待定的顺序传播，这个传播过程即DOM事件流

  - addEventListener(type,listener,[userCapture]) 第三个参数为true 捕获，false则为冒泡
  - js代码中只能执行捕获或冒泡一个阶段
  - 捕获：doc -> html -> body -> father -> son (大--小)
  - 冒泡：son -> father -> body -> html -> doc (小--大)

- 事件对象：事件发生之后，跟事件相关的一系列信息数据的集合都放到了事件对象里面

  | 事件属性            | 说明                           |
  | ------------------- | ------------------------------ |
  | e.target            | 返回触发事件的对象             |
  | e.srcElement        | 返回触发事件的对象 非标准      |
  | e.type              | 返回事件类型 click mouseover   |
  | e.cancelBubble      | 阻止冒泡                       |
  | e.returnValue       | 阻止默认事件 非标准            |
  | e.preventDefault()  | 阻止默认事件 标准 不让链接跳转 |
  | e.stopPropagation() | 阻止冒泡 标准                  |

- 事件委托，灵活利用冒泡，把公用事件注册给父级

- 常用鼠标事件

  | 事件函数                                                     | 说明             |
  | ------------------------------------------------------------ | ---------------- |
  | document.addEventListener('***contextmenu***',function(e){e.preventDefault();}) | 禁止鼠标右键菜单 |
  | document.addEventListener('***selectstart***',function(e){e.preventDefault();}) | 禁止鼠标选中     |

- 常用键盘事件

  | 键盘事件   | 触发条件                                               |
  | ---------- | ------------------------------------------------------ |
  | onkeyup    | 某个键盘按键被松开时触发，该事件文字已经落入文本框里面 |
  | onkeydown  | 某个键盘按键被按下时触发                               |
  | onkeypress | 某个键盘按键被按下时触发                               |
  | keyCode    | 返回该键的ASCII值                                      |



### 1.11 BOM浏览器对象模型

- window

  | 事件函数                        | 说明                   |
  | ------------------------------- | ---------------------- |
  | onresize                        | 调整窗口大小事件       |
  | setTimeout() / clearTimeout()   | 定时器  清除定时器     |
  | serInterval() / clearInterval() | 持续调用  清除循环事件 |

- js的执行机制 单线程

  - 先执行 执行栈中的同步任务，再去执行任务队列中的异步任务，再把异步任务放入执行栈执行
  - 回调函数是异步任务在消息队列中
  - 事件循环 event loop

- location对象

  | 属性     | 说明                           |
  | -------- | ------------------------------ |
  | protocol | 通信协议 http ftp maito        |
  | host     | 主机                           |
  | port     | 端口                           |
  | path     | 路劲 /....                     |
  | query    | 参数 键值对 & 区分             |
  | fragment | 片段 #后面内容 常见于链接 锚点 |

  - 属性： href、pathname、search、hash
  - 方法：assign() 跳转页面、replace() 替换当前页面 不能后退、reload() ==F5

- navigator对象：包含浏览器的信息

- history对象： back() forward() 前进 go()

### 1.12 网页特效

- offset：偏移量，动态的得到该元素的偏移、大小 offset Top Left Parent Height Width
  
  - 只读属性，获取元素使用offset，更改元素属性使用style.
  - offsetparent和parentNode，前一个是返回有定位的父级、后一个是返回最近一级的父级
  - 获取图片在盒子中的坐标
  - mousemove事件    
  
- client：与offset一样，但是不包含边框

- 立即执行函数：不需要调用，立马能够自己执行

  ```javascript
  // 写法
  (function(){})();
  (function(){}());
  ```

- scroll：滚动距离 

  - overflow:auto 显示滚动条

- 总结：

  - offset经常用于获得元素位置	offsetLeft offsetTop
  
  - client用于获取元素大小  	clientWidth clientHeight
  
  - scroll经常用于获取滚动距离 	scrollTop scrollLeft
  
  - 页面滚动的距离通过 window.pageXOffset 获得
  
    
  
- 动画使用 setInterval

- 缓动动画公式:(目标值-现在的位置)/10

  ```javascript
  function animate(obj,target){
  	//先清除以前的定时器，只保留当前的一个定时器执行
  	clearInterval(obj.timer);
  	obj.timer = setInterval(function(){
  		//步长值写到定时器的里面
  		vat step = (target-obj.offsetLeft)/10;
  		if(obj.offsetLeft>=target){
  			//停止动画 本质是停止定时器
  			clearInterval(obj.timer);
  		}
  		// 把每次加1 这个步长值改为一个慢慢变小的值 步长公式
  		obj.style.left = obj.offsetLeft + 1 +'px';
  	},30)
  }
  
  animate(span,500)
  ```

- 案例

  - 轮播图
  - 节流阀，等前一张图片加载了再继续加载
  - 返回顶部
  - 筋斗云



- 移动端网页特效
  - 触屏事件
  - 移动端拖动元素
  - [Swiper插件](https://swiper.com.cn)



- 本地储存

  | sessionStorage           | localStorage                   |
  | ------------------------ | ------------------------------ |
  | 生命周期为关闭浏览器窗口 | 生命周期永久生效，除非手动删除 |
  | 同一个窗口下数据可以共享 | 可以多窗口共享                 |
  | 以键值对的形式存储使用   | 以键值对的形式存储使用         |

  保存用户名，存在localStorage中



## 2. jQuery

### 2.1 jQuery概述

- 优点：

  - 轻量级，跨浏览器兼容
  - 链式编程、隐式迭代
  - 对事件、样式、动画支持，简化DOM操作
  - 支持插件扩展开发
  - 免费开源

- [下载](https://jquery.com)

  ```html
  // 标签引入
  <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
  ```

### 2.2 基本使用

- 入口函数

  ```javascript
  // $是jQuery的别称 顶级对象
  //等dom加载完毕执行js 代码
  $(function(){
  	$('div').hide()
  })
  // DOM对象转化为jQuery对象
  $(DOM对象)
  // JQuery对象转化为 DOM对象
  $('div')[0]
  ```

- 常用API

  ```javascript
  // 选择器 同css选择器
  $("选择器")
  // 鼠标经过 隐式迭代
  $(".nav>li").mouseover(function(){
      $(this).children("ul").show();
  })
  $(".nav>li").mouseover(function(){
      $(this).children("ul").hide();
  })
  // 排他思想
  $("button").click(function(){
      // 点击当前button样式
      $(this).css("background","pink");
      // 兄弟button样式
      $(this).siblings("button").css("background","")
  })
  // 添加类
  $(this).addClass("current");
  .removeClass()
  .toggleClass()
  ```

- 动画效果

  - 显示隐藏：show()	hide()	toggle()
  - 滑动：slideDown()   slideUp()  slideToggle()
  - 淡入淡出：fadeIn()  fadeOut()  fadeToggle()  fadeTo()
  - 自定义动画：animate()
  
- 属性操作

  ```javascript
// 获取固有属性
  prop("属性")
prop("属性","属性值")
  // 获取自定义属性
  attr("属性")
  attr("属性","属性值")
  // 数据缓存 data()
  data("name","value")
  // 获取元素内容 相当于原生innerHTML
  html()
  html("设置的内容")
  // 获取文本内容
  text()
  text("设置的文本")
  // 兄弟选择器
  siblings(".p-price") 
  ```
  
- 元素操作

  ```javascript
  // 遍历元素
  $("div").each(function(index,domEle){
  	$(domEle).css("color","blue")
  })
  // 内部添加
  .append()
  .prepend()
  // 外部添加
  .after()
  .before()
  // 添加类名
  .addClass("classname")
  .removeClass("classname")
  ```


### 2.3 事件

- 事件注册

  ```javascript
  // 1.单个事件注册
  element.事件(function(){})
  // click mouseover mouseout blur focus change keyup resize scroll
  
  // 2.事件处理on 可绑定动态事件
  $("div").on({
      mouseenter:function(){
          $(this).css("background","skyblue");
      }
      click:funciton(){
  		$(this).css("background","purple");    
  	}
  	mouseleave:function(){}
  })
  // 解绑事件 off()
  $("div").off("click")
  // 只触发一次
  $("p").one("click",function(){alert("h1")})
  // 自动触发
  $("div").click()
  $("div").trigger("click")
  $("div").triggerHandler("click")
  ```

- 其他方法

  ```javascript
  // 对象拷贝 deep为true深拷贝，target拷贝目标 object1待拷贝到第一对象的对象
  $.extend([deep],target,object1,[objectN])
  // 多库共存 $符号冲突
  // $ 换成 jQuery
  // 自定义
  var diy = $.noConflict();
  diy("span") // diy相当于$
  ```

### 2.4 插件

[jQuery插件库](https://www.jq22.com/)

[jQuery之家](http://www.htmleaf.com/)



## 3.JavaScript高级

### 3.1 面向对象编程

封装性、继承性、多态性

```javascript
// 1.创建类
class Star{
    constructor(uname){
        this.uname = uname
    }
    sing(song){
		console.log(this.uname + song)
    }
}
// 2.利用类创建对象
var ldh = new Star('刘德华')
console.log(ldh.uname)
ldh.sing('忘情水')

// 类的继承
class Father{
    constructor(x,y){
        this.x = x;
        this.y = y;
    }
    sum(){
        console.log(this.x+this.y);
    }
    say(){
        return '我是爸爸'
    }
}
class Son extends Father{
    constructor(x,y){
        super(x,y);// 调用了父类中的构造函数
    }
    say(){
        console.log(super.say()+'的儿子')
    }
}
var son = new Son(1,2);
son.sum();

// 实例成员只能通过实例化对象访问，静态成员只能通过构造函数访问
```

- 原型
  - prototype原型对象
  - 作用是共享方法，节约内存

```javascript
Star.prototype.sing() // 原型对象
ldh = new Star()
ldh.__proto__ //对象原型

Star.prototype = {
    constructor:Star,
    sing:function(){
        console.log('唱歌')
    },
    movie:function(){
        console.log('电影')
    }
}
// 扩展内置对象
Array.prototype.sum = function(){
	var sum =0;
    for(var i=0;i<this.length;i++){
		sum += this[i];
    }
    return sum;
}
var arr = [1,2,3]
arr.sum()
```

- call()

```javascript
function fn(x,y){
	console.log('我想喝手磨咖啡')
    console.log(this)
}
var o = {name:'andy'}
fn.call() // 1. 可以调用函数
fn.call(o,1,2) // 2. 改变这个函数的this指向

// 借用构造函数继承父类属性
function Father(uname,age){
    this.uname = uname;
    this.age = age;
}
Father.prototype.money = function(){
    console.log(10000)
}
function Son(uname,age){
    Father.call(this,uname,age);// 继承Father构造函数
}
Son.prototype = new Father();
Son.prototype.constructor = Son;
var son = new Son('刘德华',18);
console.log(son)
```

- 数组方法(ES5新增)

```javascript
//遍历
array.forEach(function(currentValue,index,arr))
//迭代过滤 返回一个 新数组
array.filter(function(currentValue,index,arr)) 
//some 查找数组中是否有满足条件的元素 返回bool值
array.some(function(currentValue,index,arr))

array.map
```

- 字符串方法(ES5新增)

```javascript
// 删除空白字符
str.trim()
```

- 对象方法(ES5新增)

```javascript
// 用于获取对象所有属性
var arr = Object.keys(obj)
// 定义对象中新属性或修改原有的属性
Object.defineProperty(obj,prop,descriptor)
// descriptor: value、writable、enumerable、configurable

```

### 3.2 函数进阶

- 函数定义与调用

  ```javascript
  // 1.命名函数
  function fn(){}
  // 2.匿名函数
  var fun = function(){}
  // 3.new Function
  var fn = new Function('param1','param2',...,'函数体')
  
  // 函数调用
  // 1.普通函数
  function fn(){
      console.log('人生的巅峰')
  }
  fn()
  fn.call()
  // 2.对象方法
  var o = {
      sayHi:function(){
          console.log('人生的巅峰')
      }
  }
  o.sayHi()
  // 3.构造函数
  function Star(){};
  new Star();
  // 4.绑定事件函数
  btn.onclick = function(){};
  // 5.定时器函数
  setInterval(function(){},1000) // 定时1s调用函数
  // 6.立即执行函数
  (function(){
  	console.log('人生的巅峰')
  })();
  ```
  
- this指向

  | 调用方式     | this指向       |
  | ------------ | -------------- |
  | 普通函数调用 | window         |
  | 构造函数调用 | 实例对象       |
  | 对象方法调用 | 该方法所属对象 |
  | 事件绑定调用 | 绑定事件对象   |
  | 定时器函数   | window         |
  | 立即执行函数 | window         |

- 改变函数内部this指向

  - call(o)
  - apply(o,['pink'])
    - var max = Math.max.apply(Math,arr)
  - bind(this)

- 严格模式：在严格模式下，全局作用域中函数中的this是undefined

- 高阶函数：接收函数作为参数或者将函数作为返回值输出

  ```javascript
  // 闭包函数
  function fn(){
  	var num = 10;
      function fun(){
          console.log(num)
      }
      return fun;
  }
  var f = fn() // 函数声明
  f() // 调用
  
  // 递归函数
  var num = 1;
  function fn(){
      console.log('我要打印6句话')
      if(num == 6){
          return;
      }
      num++;
      fn()
  }
  fn();
  ```
  
- 拷贝

  ```javascript
  // 浅拷贝
  var obj = {
      id:1,
      name:'andy',
      msg:{
          age:18
      }
  }
  var o = {}
  for (var k in obj){
  	o[k] = obj[k];
  }
  Object.assign(o,obj)
  
  // 深拷贝
  function deepCopy(newobj,oldobj){
      for(var k in oldobj){
          var item = oldobj[k];
          if(item instanceof Array){
              newobj[k]=[];
              deepCopy(newobj[k],item)
          }else if(item instanceof Object){
              newobj[k]={};
              deepCopy(newobj[k],item);
          }else{
              newobj[k]=item;
          }
      }
  }
  ```
### 3.3 正则表达式

- 在js中使用

  ```javascript
  // 利用字面量
  var rg = /123/;
  rg.test(123) // return bool
  
  ```

  | 边界符 | 说明                       |
  | ------ | -------------------------- |
  | ^      | 表示匹配的文本以谁开始     |
  | $      | 表示匹配的文本以谁结束     |
  | []     | 表示匹配其中一个           |
  | *      | 重复零次或更多次           |
  | +      | 重复一次或更多次           |
  | ？     | 重复零次或一次             |
  | {n}    | 重复n次                    |
  | {n,}   | 重复n次或更多次            |
  | {n,m}  | 重复n到m次                 |
  | \d     | 匹配0-9                    |
  | \D     | 匹配0-9以外的字符          |
  | \w     | 匹配任意字母、数字、下划线 |
  | \W     | 除去字母、数字、下划线     |
  | \s     | 匹配空格                   |
  | \S     | 匹配非空格                 |

### 3.4 ES6

- 新增语法

  ```javascript
  let   // 临时变量
  const // 静态变量 必须赋值
  // 展开语法
  let arr = [1,2,3]
  ...arr // 1,2,3
  // Array.from() 将类数组或可遍历对象转换为真正的数组
  // Array.find() 找到一个符合条件的组成员
  // Array.findIndex() 找出第一个符合条件的数组成员的位置， 如果没有返回 -1
  let ary = [10,20,50]
  let index ary.findIndex(item=>item>15);
  ary.includes(10)
  str.startsWith()
  str.endsWith()
  "y".repeat(5)
  // Set数据结构
  const s5 = new Set(['a','b','c'])
  ```
  
  

## 4.前后端交互

### 4.1 node.js

- 模块化开发

  - exports 对象导出
  - require 导入

  ```javascript
  let a = require('./b.js')
  module.exports.version = version; // 导出成员对象
  module.exports = {
  	name:'wc',
  }
  // fs模块
  const fs = require('fs')
  fs.readFile('文件路径/文件名称','文件编码',callback);
  fs.writeFile('文件路径/名称','数据',callback);
  
  // path模块
  const path = require('path');
  const finalPath = path.join('public','uploads','avatar');
  
  // 使用 nodemon替代node执行文件
  npm install nodemon -g
  // 切换模块源文件
  npm install nrm -g
  nrm use xxx 
  ```
  
- 一些web开发相关的包

  ```javascript
  const http = require('http')
  const getRouter = require('router')
  // 引入模板引擎
  const template = require('art-template')
  const path = require('path')
  const querystring = require('querystring')
  // 引入静态资源
  const serveStatic = require('serve-static')
  // 引入处理日期时间的模块
  const dateformat = require('dateformat')
  // 获取路由对象
  const router = getRouter()
  // 实现静态资源访问服务
  const serve = serveStatic(path.join(__dirname,'public'))
  
  // 配置模板的根目录
  template.defaults.root = path.join(__dirname,'views');
  template.defaults.imports.dateformat = dateformat;
  
  // 返回页面
  router.get('/add',(req,res)=>{
      Let html = template('index.art',{});
      res.end(html)
  })
  ```

  

### 4.2 Gulp

基于node平台开发的前端构建工具

[官方文档](https://www.gulpjs.com.cn/docs/getting-started/quick-start/)

- gulp使用

  - 使用 npm install gulp 下载
  - 在项目根目录下建立gulpfile.js文件
  - 重构项目的文件夹结构src目录放置源代码文件dist目录放置构建后文件
  - 在gulpfile.js文件中编写任务
  - 在命令行工具中执行gulp任务 npm install gulp-cli

  ```javascript
  gulp.src()		// 获取任务要处理的文件
  gulp.dest()		// 输出文件
  gulp.task()		// 建立gulp任务
  gulp.watch()	// 监控文件的变化
  
  const gulp = require('gulp')
  // 1.名称 2.callback
  gulp.task('first',()=>{
      gulp.src('./src/css/base.css')
      .pipe(gulp.dest('./dist/css'));
  })
  ```

- gulp插件，使用的时候查询官网

  - gulp-htmlmin  html文件压缩
  - gulp-csso  压缩css
  - gulp-babel  JavaScript语法转化
  - gulp-less  less语法转化
  - gulp-uglify  压缩混淆js
  - gulp-file-include  公共文件包含
  - browsersync  浏览器实时同步

- package.json文件

  - npm init -y：初始化依赖文件
  - npm install --production

- nodejs中的加载机制

  - require方法根据模块路径查找模块，如果是完整路径，直接引入模块
  - 如果模块后缀省略，先找同名js文件再找同名js文件夹
  - 如果找到了同名文件夹，找文件夹中的index.js
  - 如果没有index.js 就会去文件夹中的package.json文件中查找main选项中的入口文件
  - 若入口文件不存在，则模块没有被找到

- Promise: 解决回调地狱

  ```javascript
  let promise = new Promise((resolve,reject)=>{
      setTimeout(()=>{
  		if (true){
              resolve({name:'wc'});
          }else{
              reject('fail')
          }
      },2000)
  })
  promise.then(result=>console.log(result)) // 成功的resolve回调
  	.catch(error=>console.log(error)})  // 失败的reject回调
  // 链式编程promise 每一次都return一个Promise
  .then
  // 普通函数前面加 async 变成异步函数，异步函数默认的返回值是promise对象
  // 异步函数才能用await 
  const fs = require('fs')
  const promisify = require('util').promisify
  const readFile = promisify(fs.readFile)
  async function run(){
      let r1 = await readFile('./1.txt','utf-8');
      let r2 = await readFile('./2.txt','utf-8');
      let r3 = await readFile('./3.txt','utf-8');
      console.log(r1)
      console.log(r2)
      console.log(r3)
  }
  run()
  
  Promise.then()
  .catch()
  .finally()
  Promise.all([p1,p2,p3]).then(res=>console.log())// 返回数组，依次为p1 p2 p3的结果
  ```


### 4.3 MongoDB

- 数据库概念
  - database 数据库
  - collection 集合
  - document 文档 相当于js中的对象
  - field 字段
  
- 操作：和数据库操作相关的都是异步

  ```javascript
  // net stop mongodb  停止服务
  // net start mongodb  开启服务
  const mongoose = require('mongoose')
  // 连接mongodb数据库
  mongoose.connect('mongodb://localhost/playground', { useNewUrlParser: true })
      .then(() => console.log('suc'))
      .catch(err => console.log(err, 'err'))
  
  // 创建集合
  const courseSchema = new mongoose.Schema({
      name:String,
      author:String,
      isPublished:Boolean
  });
  // 创建集合并应用规则
  const Course = mongoose.model('Course',courseSchema); //courses
  
  // 创建文档
  const course = new Course({
      name: 'wangchen',
      author: 'wc',
      isPublished: true
  });
  course.save();
  
  // 导入数据
  // mongoimport -d 数据库名称 -c 集合名称 --file 要导入的数据文件
  
  Course.find().then(result => console.log(result))
  Course.findOne({name:'wangchen'}).then(res=>console.log(res))
  
  // $gt 大于 $lt 小于
  User.find({age:{$gt:20,$lt:40}}).then(result=>console.log(result))
  User.find({hobbies:{$in:['敲代码']}}).then(res=>console.log(res))
  User.find().select('name email').then(res=>console.log(res))
  User.find().sort('-age').then(res=>console.log(res))
  // skip 跳过多少条数据 limit 限制多少数量
  
  // 查到第一条文档并删除
  // 返回删除的文档
  Course.findOneAndDelete({_id:'607849a96e96364bd4d63808'}).then(res=>console.log(res))
  
  // 更改文档
  Course.updateOne()
  Course.updateMany()
  ```

- 验证规则

  ```javascript
  const mongoose = require('mongoose')
  
  mongoose.connect('mongodb://localhost/playground', { useNewUrlParser: true })
      .then(() => console.log('suc'))
      .catch(err => console.log(err, 'err'))
  const postSchema = new mongoose.Schema({
      title: {
          type: String,
          required: [true, '请传入文章标题'],
          minlength: [2, '最小为2'],
          maxlength: [5, '最大为5'],
          trim: true,
      },
      age: {
          type: Number,
          min: 18,
          max: 100
      },
      publishDate: {
          type: Date,
          default: Date.now
      },
      category: {
          type: String,
          enum: ['html', 'css', 'java', 'js', 'node']
      },
      author: {
          type: String,
          validate: {
              validator: v => {
                  return v && v.length > 4
              },
              message: '传入的值不符合验证规则'
          }
      }
  });
  const Post = mongoose.model('Post', postSchema);
  
  Post.create({ title: 'ac', age: 20, category: 'js', author: '12345' }).then(res => console.log(res))
  	catch(error=>{
        const err = error.errors;
          for(var attr in err){
              console.log(err[attr]['message'])
          }
  })
  ```
  
- 关联查询

  ```javascript
  author: {
          type: mongoose.Schema.Types.ObjectId,
          ref: 'User',
      }
  Post.find().populate('author').then(res => console.log(res))
  ```

### 4.4 Express

- 简易框架

  ```javascript
  // 引入express框架
  const express = require('express');
  // 创建网站服务器
  const app = express();
  
  app.get('/', (req, res) => {
      console.log('req', req)
      res.send('Hello. Express')
  })
  
  app.get('/list', (req, res) => {
      res.send({ name: 'wc', age: 27 })
  })
  // 监听端口
  app.listen(3500)
  console.log('网站启动成功')
  ```

- 中间件

  ```javascript
  // next方法将请求的控制权交给下一个中间件
  app.use((req,res,next)=>{
      console.log('请求走了app.use()中间件');
      next();
  })
  app.use('/request',(req,res,next)=>{
      console.log('请求走了app.use /request 中间件');
      next();
  })
  app.get('/request',(req,res,next)=>{
      req.name='wc';
      next();
  })
  app.get('/request',(req,res)=>{
      res.send(req.name);
  })
  // 异常处理
  app.get('/index', (req, res, next) => {
      // throw new Error('未知异常');
      // res.send('正常执行')
      fs.readFile('./12.txt', 'utf8', (err, result) => {
          if (err) {
              next(err);
          } else {
              res.send(result);
          }
      })
  })
  // 错误处理中间件
  app.use((err, req, res, next) => {
      res.status(500).send(err.message);
      next()
  })
  ```

- 中间件应用

  - 路由保护，判断用户登录状态，未登录直接响应
  - 网站维护公告
  - 自定义404页面

- 路由模块化

  ```javascript
  const express = require('express');
  const app = express();
  const home = express.Router();
  app.use('/home',home);
  //创建二级路由
  home.get('/index',(req,res)=>{
      res.send('欢迎来到90年代');
  })
  module.exports = home; // 导出模块
  
  // GET请求获取
  req.query
  // POST请求获取
  const bodyParser = require('body-parser');
  app.use(bodyParser.urlencoded({extended:false}))
  app.post('/add',(req,res)=>{
      res.send(req.body);
  })
  
  // 实现静态资源访问功能
  app.use('/static',express.static(__dirname))
  ```


### 4.5 Ajax

- ajax封装

  ```javascript
  function ajax(options){
      // 默认值
      var defaults = {
          type:'get',
          url:'',
          data:{},
          header:{
              'Content-Type':'application/json'
          },
          success:function(){},
          error:function(){}
      }
      //使用options 覆盖default的属性
      Object.assign(defaults,options);
      var xhr = XMLHttpRequest();
      var params = '';
      for(var attr in defaults.data){
          params +=attr+'='+defaults.data[attr]+'&';
      }
      params = params.substr(0,params.length-1);// 删除最后的&
      if(defaults.type == 'get'){
          defaults.url = defaults.url+'?'+params;
      }
      xhr.open(defaults.type,defaults.url);
      if(defaults.type == 'post'){
          // xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
          var contentType = defaults.header['Content-Type'];
          xhr.setRequestHeader('Content-Type',contentType);
          if(contentType == 'application/json'){
              xhr.send(JSON.stringify(defaults.data))
          }else{
              xhr.send(params);
          }
      }else{
          xhr.send();
      }
      xhr.onload=function(){
          if(xhr.status == 200){
              defaults.success(xhr.responseText,xhr);
          }else{
              defaults.error(xhr.responseText,xhr);
          }
          
      }
  }
  ajax({
      type:'get',
      urlL:'http://localhost:3000/first',
      data:{
          name:'wc',
          age:20
      },
      header:{
          'Content-Type':'application/json'
      }
      success:function(data){console.log(data)},
      error:function(data,hxr){
          console.log(data);
          console.log(xhr);
      }
  })
  ```


### 4.6 模板引擎

- [art-template地址](http://aui.github.io/art-template/zh-cn/index.html)
- 常用前后端分离，前端使用框架，故模板引擎方法只做介绍



## 5 Vue基础

### 5.1 模板语法

- [vue](https://cn.vuejs.org) 渐进式JavaScript框架

- 指令：自定义属性，格式v-开头

- v-text填充纯文本、v-html填充html片段、v-pre:提供原始信息

- 双向数据绑定 v-model

- MVVM设计思想

- 事件绑定：

  ```javascript
  <div id='app'>
      <div>{{num}}</div>
  	<button v-on:click='num++'></button>
  	<button @click='handle'></button> //handle()
  </div>
  var vm = new Vue({
      el:'#app',
      data:{
          num:0
      },
      methods:{
  		handle:function(){
  			this.num++;
          }
      }
  })
  // 事件修饰符
  v-on:click.stop // 阻止冒泡
  v-on:click.prevent // 阻止默认行为
  
  // 属性绑定
  <a v-bind:href='url'></a>
  <a :href='url'></a>
  // 样式绑定
  v-bind:class="{...}"
  v-bind:style='objStyles'
  
  // 分支结构
  v-if // 元素是否渲染到页面
  v-show // 元素已渲染，是否显示
  v-else
  v-else-if
  v-show
  
  // 循环结构
  <li :key='item.id' v-for='(item,index) in list'>{{item}}+ '---'{{index}}</li>
  
  ```

- 常用特性

  ```javascript
  // 表单区域修饰符
  number: //转化为数值
  trim:
  lazy: // 将input转化为change
  
  // 计算属性 有缓存  方法没有缓存
  computed:{
      reverseString:function(){
          return this.msg.split('').reverse().join('')
      }
  }
  
  // 侦听器 侦听数据变化
  watch:{
  	firstName:function(val){
          this.fullName=val+' '+this.lastName
      }
  }
  // 过滤器 自定义过滤器
  Vue.filter('upper',function(value){
      // 过滤器业务逻辑
  })
  filters:{// 局部过滤器
      capitalize:function(){}
  }
  // 过滤器使用
  <div>{{msg | upper}}</div>
  
  // 生命周期
  // 挂载
  beforeCreate
  created
  beforeMount
  mounted
  // 更新
  beforeUpdate
  updated
  // 销毁
  beforeDestroy
  destroyed
  ```


### 5.2 组件化开发

- 组件

  ```javascript
  // 全局注册语法
  //Vue.component(组件名称,{
  //    data:组件数据,
  //    template:组件模板内容
  //})
  Vue.component('button-counter',{
      data:function(){
          return{
              count:0
          }
      },
      template:'<button @click="handle">点击了{{count}}</button>',
      methods:{
          handle:function(){
              this.count++;
          }
      }
  })
  // 使用
  <button-counter></button-counter>
  // 组件命名，小写字母-的形式
  
  // 局部组件注册
  var ComponentA = {/*  */}
  var ComponentB = {/*  */}
  var ComponentC = {/*  */}
  new Vue({
      el:'#app',
      components:{
          'component-a':ComponentA,
          'component-b':ComponentB,
          'component-c':ComponentC,
      }
  })
  ```

- 数据交互

  ```javascript
  // 父传子
  Vue.component('menu-item',{
      props:['title'],
      template:'<div>{{title}}</div>'
  })
  <menu-item title="来自父组件的数据"></menu-item>
  <menu-item :title="title"></menu-item>
  
  // 子传父
  // 子
  @click='$emit("enlarge-text",5)'
  // 父
  <menu-item @enlarge-text='handle($event)'></menu-item>
  var vm = new Vue({
      el:'#app',
      data:{
          fontSize:10
      },
      methods:{
          handle:function(val){
              this.fontSize +=5;
          }
      }
  })
  // 监听事件
  var hub = new Vue();
  methods:{
      handle:function(){// 触发
          hub.$emit('tom-event',2)
      }
  }
  mounted:function(){ // 监听
  	hub.$on('jerry-event',(val)=>{
      this.num += val;
      })
  },
      
  // 插槽
  Vue.component('alert-box',{
      template:`
  		<div>
  			<slot></slot> // 插槽预留
  		</div>
  		`
  })
  <alert-box>something happened</alert-box> // 插槽属性
  ```

  

### 5.3 axios

  - 支持浏览器和nodejs

  - 支持promise

  - 能拦截请求和响应

  - 自动转换JSON

    ```javascript
    // fetch用法
    fetch('/abc',{
        method:'post',
        body:JSON.stringify({
            uname:'wc',
            pwd:'123'
        }),
        headers:{
            'Content-Type':'application/json'
        }
    }).then(data=>{
        return data.text();
        //return data.json();
    }).then(ret=>{
        console.log(ret)// 这里得到最终数据
    })
    
    // axios
    axios.get('/url',{
        params:{
            id:123
        }
    })
    .then(ret=>{
        console.log(ret)
    })
    axios.post('/url',{
    	uname:'wc',
        pwd:'123'
    }).then(ret=>{
        console.log(ret)
    })
    // 响应主要内容 data headers status statusText
    
    
    // axios请求拦截器
    axios.interceptors.request.use(function(config){
        // 在请求发出之前进行一些信息设置、
        console.log(config)
        config.headers.mytoken = 'nihao';
        return config;
    },function(err){
        // 处理响应的错误信息
    })
    // axios响应拦截器
    axios.interceptors.response.use(function(res){
    	//对返回的数据进行处理
        return res;
    },function(err){
    	// 处理响应的错误信息
    })
    ```
    

### 5.4 前端路由

- 简易实现前端路由

  ```javascript
  <div id="app">
      <a href="#/zhuye">主页</a>
  	<a href="#/keji">科技</a>
  	
  	<component :is="comName"></component>
  </div>
  
  // 定义组件
  cosnt zhuye = {
      template:'<h1>主页</h1>'
  }
  cosnt keji = {
      template:'<h1>科技</h1>'
  }
  const vm = new Vue({
      el:'#app',
      data:{comName},
      components:[
          zhuye,
          keji
      ]
  })
  window.onhashchange=function(){
      // 通过location.hash 获取最新的hash值
      switch(loaction.hash.slice(1)){
          case '/zhuye':
              vm.comName='zhuye'
              break;
          case '/keji':
              vm.comName='keji'
              break;
          default:break;
      }
  }
  ```

- vue-router

  ```javascript
  // 添加路由链接
  <router-link to="/user">User</router-link>
  <router-link to="{name:'user',params:{id:123}}">User</router-link>
  <router-link to="/register">Register</router-link>
  router.push({name:'user',params:{id:123}})
  
  // 添加路由填充位
  <router-view></router-view>
  
  // 定义路由组件
  var User = {
      props:['id'],
      template:'<div>User{{$route.params.id}}</div>'
  } // 获取动态路由匹配数据id
  var Register = {template:'<div>Register</div>'}
  // 创建路由实例对象
  var router = new VueRouter({
      routes:[
          {
              path:'/user:id',
              component:User,
              name:'user',//用于匿名路由
              props:true
          }, // props可以直接传对象
        {
              path:'/register',
              component:Register，
              children:[ // 使用children实现嵌套路由
              	{path:'/register/tab1',component:Tab1},
          		{path:'/register/tab2',component:Tab2},
              ]
          },
          {path:'/',redirect:'/user'},// 重定向
      ]
  })
  
  const vm = new Vue({
      el:'#app',
      data:{},
      router:router //挂载路由实例对象
  })
  
  // 编程式导航
  this.$router.push('hash地址')
  this.$router.go(n)
  router.push({path:'/home',query:{uname:'lisi'},params:{userId:123}})
  ```
  





## 6 前端工程化

### 6.1ES6模块 

​	每个js文件都是一个独立的模块

​	导入模块成员使用**import**关键字

​	暴露模块成员使用**export**关键字

- Node.js中通过babel体验ES6模块化

  - npm install --save-dev @babel/core @babel/cli @babel/preset-env @babel/node
  - npm install --save @babel/polyfill
  - 项目跟目录创建文件 babel.config.js
  - bebel.config.js 

  ```javascript
  const presets = [
  	["@babel/env",{
          targets:{
              edge:"17",
              firefox:"60",
              chrome:"67",
              safari:"11.1"
          }
      }]
  ]
  module.exports = {presets}
  // npx babel-node index.js 执行代码
  ```

- 默认导出 导入

  export default {  //默认导出成员}

  import 接收名称 from '模块标识符'

- 按需导出 导入

  export let s1 = 10
  
  import { s1 } from '模块标识符'
  
- 直接导入并执行模块代码

  import './m2.js'



### 6.2 webpack

前端项目打包工具，提供友好的模块化支持、代码压缩混淆、处理js兼容性问题、性能优化

- 在项目中安装和配置webpack

  - 运行 npm install webpack webpack-cli -D 

  - 项目根目录创建 webpack.config.js 配置文件

    ```javascript
    // webpack.config.js 配置文件内容
    module.exports = {
        mode:'development' // mode 用来指定构建模式
    }
    ```

  - 在package.json的scripts节点下，新增dev脚本如下

    ```json
    "scripts":{
        "dev":"webpack" // script 节点下的脚本 可以通过 npm run 执行
    }
    ```

  - 终端运行 npm run dev 命令，启动 webpack进行项目打包

- 入口与出口

  ```javascript
  const path = require('path')
  
  module.exports = {
  	mode:'development',
  	entry:path.join(__dirname,'./src/index.js'), // 入口
  	output:{ // 出口
  		path:path.join(__dirname,'./dist'),
  		filename:'bundle.js'
  	}
  }
  ```

- 配置自动打包

  - 运行 npm install webpack-dev-server -D

  - 修改 package.json -> scripts 中的dev命令如下

    ```json
    "scripts":{
        "dev":"webpack-dev-server --open --host 127.0.0.1 --port 8888" // script 节点下的脚本 可以通过 npm run 执行
    }
    ```

  - 将src -> index.html 中 script脚本的引用路径，修改为"./buldle.js"

  - 运行 npm run dev命令

  - 浏览器访问 http://localhost:8080 查看自动打包效果

- 配置html-webpack-plugin 生成预览页面

  - 运行 npm install html-webpack-plugin -D

  - 修改webpack.config.js 文件头部区域，添加如下配置

    ```javascript
    // 导入插件
    const HtmlWebpackPlugin = require('html-webpack-plugin')
    const htmlPlugin = new HtmlWebpackPlugin({
        template:'./src/index.html',//指定要用到的模板文件
        filename:'index.html' // 指定生成的文件的名称，该文件存在于内存中 在目录中不显示
    })
    ....
    
    module.exports = {
        plugins:[htmlPlugin] // plugins数组是webpack打包期间会用到的一些插件列表
    }
    ```

- 通过loader打包非js模块

  - css、less、sass

    `npm i style-loader css-loader -D`

    `npm i less-loader less -D`

    `npm i sass-loader node-sass -D`

    ```json
    module:{
        rules:[
            {test:/\.css$/,use:['style-loader','css-loader']},
            {test:/\.less$/,use:['style-loader','css-loader','less-loader']}
            {test:/\.sass$/,use:['style-loader','css-loader','sass-loader']}
        ]
    }
    ```

  - 配置postCSS自动添加css的兼容前缀

    `npm i postcss-loader autoprefixer -D`

    ```javascript
    // 项目根目录创建 postcss配置文件 postcss.config.js 
    const autoprefixer = require('autoprefixer')
    module.exports = {
        plugins:[autoprefixer]
    }
    
    // webpack.config.js中 module -> rules 数组中修改css 
    module:{
        rules:[
            {test:/\.css$/,use:['style-loader','css-loader','postcss-loader']}
        ]
    }
    ```

  - 打包样式表中的图片和字体文件

    `npm i url-loader file-loader -D`

    ```javascript
    module:{
        rules:[
            {
                test:/\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/,
                use:'url-loader?limit=16940' // 小于limit 才会被转成base64 单位是byte
            }
        ]
    }
    ```

  - 打包处理js中的高级语法

    安装babel转换器相关包 `npm i babel-loader @babel/core @babel/runtime -D`  

    安装babel语法插件相关的包 `npm i @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -d`

    ```javascript
    // 项目根目录创建 babel配置文件 babel.config.js
    module.exports = {
        presets:['@babel/preset-env'],
        plugins:['@babel/plugin-transform-runtime','@babel/plugin-proposal-class-properties']
    }
    
    // webpack.config.js 中
    module:{
        rules:[
            {test:/\.js$/,use:'babel-loader',exclude:/node_modules/}
        ]
    }
    ```

- webpack 打包发布

  ```json
  // 在package.json 文件中配置 webpack打包命令
  // 该命令默认加载项目根目录中的 webpack.config.js配置文件
  "script":{
      // 用于打包的命令
      "build":"webpack -p",
      // 用于开发调试的命令
      "dev":"webpack-dev-server --open --host 127.0.0.1 --prot 3000"
  }
  ```

  

### 6.3 Vue单文件组件

单文件组件的组成结构

- template 组件的模块区域
- script 业务逻辑区域
- style 样式区域

#### webpack中配置vue组件的加载器

运行 `npm i vue-loader vue-template-compiler -D`

在webpack.config.js配置文件中，添加 vue-loader 的配置项如下:

```javascript
const VueLoaderPlugin = require('vue-loader/lib/plugin')
module.exports = {
    module:{
        rules:[
            // ... 其他规则
            {test:/\.vue$/,loader:'vue-loader'}
        ]
    },
    plugins:[
        // ... 其他插件
        new VueLoaderPlugin() // 请确保引入这个插件！
    ]
}
```

#### 在项目中使用vue

`npm i vue -s`

```javascript
import Vue from 'vue'
import App from './components/App.vue'

const vm = new Vue({
    el:'#app',
    render:h=>h(App)
})
```



### 6.4 Vue脚手架

官网地址： https://cli.vuejs.org/zh/

安装 3.x版本的Vue脚手架:

```shell
npm install -g @vue/cli

#  创建vue项目
vue create my-project
vue ui
# 2.x版本
npm install -g @vue/cli-init
vue init webpack my-project

```















##  其他用法（临时）

### websocket

```javascript
 ws = new WebSocket('ws://127.0.0.1:8065/ws');
 ws.onmessage = (e) => { //接受数据回调
 	console.log('ws', e);
 }
 ws.send("123");
```



## 7.前端问题总结

- 前端解析对象为字符串，并作自定换行方案：
  div中设定样式 white-space: pre-wrap;
  let str = JSON.stringify(obj, null, 2)

- 复制方案
  import Clipboard from 'clipboard';
  <button class="btn" onClick={()=>this.copy()}></button>
  copy = () =>{
    const { res } = this.state;
    let clipboard = new Clipboard('.btn', {
        text: () => res
    });
    clipboard.on('success', function (e) {
        alert('复制成功')
        clipboard.destroy()
    })
  }

- 强制安装依赖
  npm install --legacy-peer-deps