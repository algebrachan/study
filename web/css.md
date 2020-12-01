# css

## 1.css层叠样式

### 1.1 简介

- 内联样式 <标签名 style="属性:属性值">content</标签名>

- 内部样式 

```html
<head>
    <style type="text/CSS">
        选择器 {
            属性1:属性值1;
            属性2:属性值2;
        }
    </style>
</head>
```

- 外部样式表，外链式：<script type="text/javascript" src="../../xx.css"></script>
- 三大特性：
  - css层叠性
  - css继承性 text-、font-、line-、color可继承
  - **css优先级**：行内样式style>id选择器>类选择器>标签选择器
  - 权重叠加
- css属性书写顺序
  - 布局定义属性：display position float clear visibility overflow
  - 自身属性：width height margin padding border background
  - 文本属性：color font text
  - 其他属性：content border-radius box-shadow
### 1.2 选择器

- 普通选择器

  ```css
  /* 标签选择器 */
  a {}
  /* 类选择器 */
  .classname {}
  /* id选择器 */
  #id {}
  /* 通配符选择器 */
  * {}
  ```
  
- 复合选择器

  ```css
  /* 后代选择器 用空格隔开*/
  div h {}
  /* 子元素选择器 用> 只选最近的一级 */
  div>strong 
  /* 交集选择器 既又关系 */
  p.red 
  /* 并集选择器 */
  p,span.red{}
  /* 伪类选择器 */
  a:link //未访问的链接
  a:visited //已访问的链接
  a:hover //鼠标移动到链接上
  a:active //选定的链接
  ```
### 1.3 基础样式

|    属性    | 内容                                                         | 实例                                                         |
| :--------: | :----------------------------------------------------------- | :----------------------------------------------------------- |
|    字体    | font-size：px<br />font-family：字体<br />font-weight：字体粗细 bold 100-900 (没有单位)<br />font-style：字体风格 | font: italic 700 20px "微软雅黑"                             |
|  外观属性  | color：文本颜色 rgb/#16进制/颜色英文<br />text-align：文本水平对齐<br />line-height：行间距<br />text-indent：首行缩进<br />text-decoration：none\|underline 用于a标签去掉下划线 | #000 黑色 #fff 白色<br />FSCapture<br />**行高等于盒子的高度就能水平居中** |
|  display   | block、inline、inline-block                                  | 块级元素、行内元素、行内块元素                               |
| background | -color<br />-image：url(...)<br />-repeat：默认平铺<br />-position：x坐标 y坐标<br />-attachment:fiexd 背景固定滚动<br />rgab(0,0,0,.3) 背景半透明 | background：#ccc url(images/sms.jpg) no-repeat fixed center top; |

### 1.4 盒子模型

|     属性      | 内容                                                         | 实例                                                         |
| :-----------: | ------------------------------------------------------------ | ------------------------------------------------------------ |
|  边框border   | -width：px<br />-style：solid实现、dashed虚线、dotted点线<br />-color：<br />-collapse：边框合并<br />-radius：圆角边框 50%高度的一半 |                                                              |
| 外边距margin  | 块级盒子水平居中<br />margin：auto<br />margin：0 auto <br />text-align：center；除了让文字居中对齐，还能让行内元素和行内块元素居中对齐<br /> | 清除内外边距*{margin：0；padding：0；}<br />取消列表样式li{list-style：none;} |
| 内边距padding | 设置内边距更改了盒子的大小                                   |                                                              |
|  box-shadow   | 水平阴影 垂直阴影 模糊距离 阴影尺寸 阴影颜色 内/外阴影       | box-shadow: 2px 2px 2px 2px #000                             |

- 外边距的合并

  - 发生塌陷的时候，给父盒子指定一个上边框，或者内边距

  - 子盒子浮动就解决了外边框塌陷的问题

  - 给父元素添加 overflow:hidden
- 盒子模型布局稳定性：width>padding>margin

### 1.5 浮动

- 标准流：块级元素、行内元素，定位，浮动

- 浮动：

  浮：float：left

  漏：浮动之后，不占有原来的位置

  特性：加了float的元素默认把display改成行内块

  如果父级宽度装不下这些浮动的盒子，多出来的盒子会另起一行

- 浮动的应用

  大盒子嵌套小盒子，需要一个标准流的父级，子级用浮动

  布局，需要划分：靠左、靠右

- 连接 a 要用li包起来，给a大小

- 清除浮动：父盒子不方便给高度、清除浮动的本质是为了解决父级内部高度为0的问题

  什么时候清除浮动？

  ​	父级没有高度

  ​	子盒子浮动了

  ​	影响下面布局了，我们就应该清除浮动

  清除浮动的方法

  - 额外标签、在最后加一个标签，clear：both

  - 父级添加overflow:hidden 可能会隐藏超出部分

  - 使用after伪元素清除浮动

    ```css
    .clearfix:after {
        content:"";
        display:block;
        height:0;
        visibility:hidden;
        clear:both;
    }
    ```

  - 使用双伪元素清除浮动

    ```css
    .clearfix:before,
    .clearfix:after {
        content:"";
        display:table;
    }
    .clearfix:after {
        clear:both;
    }
    ```
### 1.6 定位

- 特点：标准流盒子在底层，浮动的盒子在中层，定位的盒子在上层
- 定位 = 定位模式 + 边偏移
- 定位模式：position
  - 静态定位 static(默认是标准流) 基本不用
  - 相对定位 relative(相对于原来的位置) 原来的位置还继续占有
  - 绝对定位 absolute() 以带有定位的父级元素来移动位置，父级没有定位就以浏览器为准，不保留原来的位置
  - 固定定位 fixed() 完全脱标(完全不占位置) 以屏幕为准，只认浏览器的可视区域
- 堆叠顺序 z-index 数值越大越靠上
- 注意事项
  - 绝对定位 左右margin无效	left:50% margin-left:-自己宽度的一半
  - 块级元素怎么转换：display属性、浮动、绝对定位
  - 圆角矩形可以分为四个角来分别设置
  - 多用并集选择器

### 1.7 页面制作

- 布局流程
  - 必须确定页面的版心
  - 先分析行模块，再分析其中的列模块
  - 制作html结构，先有结构，后有样式
  - 行高子类可以继承父类
- 父盒子没有高度，之后的盒子会跑到底下去
- 三种常用布局方式
  - 标准流：上下排列
  - 浮动：左右排列
  - 定位：层叠概念

### 1.8 高级技巧

- 隐藏方式

  - display显示：不保留原来的位置

    display：none/block

  - visibility可见性：保留原来的位置

    hidden/visible

  - overflow溢出：hidden 溢出隐藏

- 鼠标样式：pointer小手/move移动/text文本/not-allowed禁止

- vertical-align 垂直居中对齐：只针对行内元素和行内块元素，实现图片和文字的对齐

  - baseline：基线对齐 默认
  - middle：中线对齐
  - top：顶线对齐

- 溢出文字省略号显示

  - 强制一行内显示文本 white-space：nowrap
  - 超出部分隐藏 overflow：hidden
  - 文字用省略号替代超出的部分 text-overflow：ellipsis

- css精灵技术

  - 为了减少图片访问的次数，把需要用到的背景图片放在同一张图中，用的时候规定盒子大小以及图片的坐标

- 滑动门技术

  ```css
  <a><span></span></a>
  .nav a {
      display:inline-block;
      height:33px;
      background:url(images/to.png) no-repeat;
      padding-left:15px;
  }
  .nav a span {
      display:inline-block;
      height:33px;
      background:url(images/to.png) no-repeat right;
      padding-right:15px;
  }
  ```

- 边框合并，margin：-1px

  - 边框可以 div:hover 边框用相对定位来显示不同的颜色
  - 边框css可以模拟三角

### 1.9 其他

- ico图标：[png转ico](http://www.bitbug.net/)
- 搜索引擎优化
  - title：网站名-网站介绍
  - description：<meta name="description" content="搜索关键字">
  - keyword
- 字体图标：[iconfont](https://www.iconfont.cn/)



## 2.CSS3

### 2.1 新增选择器

- 属性选择器
  |    选择符     |                 简介                  |
  | :-----------: | :-----------------------------------: |
  |    E[att]     |        选择具有add属性的E元素         |
  | E[att="val"]  | 选择具有att属性且属性值等于val的E元素 |
  | E[att^="val"] | 匹配具有att属性、且值以val开头的E元素 |
  | E[att$="val"] | 匹配具有att属性、且值以val结尾的E元素 |
  | E[att*="val"] | 匹配具有att属性、且值中含有val的E元素 |

- 结构伪类选择器

  |      选择符      |                       简介                       |
  | :--------------: | :----------------------------------------------:|
  |  E:first-child   |           匹配父元素中的第一个子元素E            |
  |   E:last-child   |            匹配父元素中最后一个E元素             |
  |  E:nth-child(n)  | 匹配父元素中的第n个子元素E<br />n可以是odd和even |
  | E:frist-of-type  |                指定类型E的第一个                 |
  |  E:last-of-type  |               指定类型E的最后一个                |
  | E:nth-of-type(n) |                 指定类型E的第n个                 |

- 伪元素选择器
  |  选择符  |           简介           |
  | :------: | :----------------------: |
  | ::before | 在元素内部的前面插入内容 |
  | ::after  | 在元素内部的后面插入内容 |
  
- 注意

  - before和after必须有content属性
  
  - before在内容前，after在内容后
  
  - before和after创建一个元素，但是属于行内元素
  
  - 因为在dom里面看不见刚才创建的元素，所以我们成为伪元素
  
  - 伪元素和标签选择器一样，权重为1
  
### 2.2 2D转换

- transform:

  - translate(x,y) x、y轴上移动的距离 translate不会改变元素在页面中的位置

  - translateX(xx%) 百分比是盒子自身的百分比

  - rotate(xxdeg)

    ```css
    /*水平垂直居中*/
    p { 
        position:absolute;
        top:50%;
        left:50%;
        width:200px;
        height:200px;
        transform:translate(-50%,50%);
    }
    /*旋转*/
    img {
        width:150px;
        border-radius:50%;
        border:5px solid pink;
        /*过渡写到本身上，谁做动画给谁加*/
    }
    /* 综合写法 将位移放前，旋转之后会改变坐标 缩放 */
    transform:translate() rotate() scale() 
    ```

- animation动画

  - 先定义动画keyframes，再使用动画 

  - 动画简写：animation:动画名称 持续时间 运动曲线 何时开始 播放次数 是否反方向 动画起始或者结束状态

    |           属性            | 描述                                                 |
    | :-----------------------: | :--------------------------------------------------- |
    |        @keyframes         | 规定动画                                             |
    |         animation         | 所有动画属性的简写属性，除了animation-play-state属性 |
    |      animation-name       | 规定keyframes的名称                                  |
    |    animation-duration     | 规定动画完成一个周期的时间 默认0                     |
    | animation-timing-function | 速度曲线 ease                                        |
    |      animation-delay      | 何时开始 0                                           |
    | animation-iteration-count | 播放次数 1 infinite                                  |
    |    animation-direction    | 逆向播放 normal                                      |
    |   animation-play-state    | 是否正在运行或暂停                                   |
    |    animation-fill-mode    | 结束后状态 forwards、 backwards                      |

### 2.3 3D转换
- 坐标系
  - x轴：水平向右
  - y轴：垂直向下
  - z轴：垂直屏幕
- 内容
  - 3D位移：translate3(x,y,z)
  - 3D旋转：rotate(x,y,z,deg)
  - 透视：perspective d视距，z轴，物体到屏幕距离 z值越大看到的图片越大
  - 3D呈现：transform-style：perserve-3d 开启3D空间，给父空间(案例，反转盒子)
- 私有前缀
  - -moz-：代表firefox浏览器私有属性
  - -ms-：代表ie浏览器私有属性
  - -webkit-：代表safari、chrome私有属性
  - -o-：代表Opera私有属性



## 3 流式布局flex

