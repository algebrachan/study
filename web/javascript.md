# JavaScript

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



## 3. 其他用法（临时）

### 3.1 websocket

```javascript
 ws = new WebSocket('ws://127.0.0.1:8065/ws');
 ws.onmessage = (e) => { //接受数据回调
 	console.log('ws', e);
 }
 ws.send("123");
```

