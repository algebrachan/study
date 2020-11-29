# HTML

## 1.浏览器
### 1.1 浏览器内核
- chorme浏览器的内核是webkit，其js引擎是V8
- Blink是webkit的分支
- web标准：w3c
	- 结构：html
	- 表现：css
	- 行为：js
- JavaScript规范：ECMAScript 6 简称ES6
### 1.2 常用开发框架
- [react.js](https://react.docschina.org/)
  - 相关衍生社区
  - [antd](https://ant.design/index-cn)
  - [andv](https://antv.vision/)
  - [dva.js](https://dvajs.com/) 
  - [umi.js](https://umijs.org/)
- [vue.js](https://cn.vuejs.org/)
- [angular.js](https://angular.cn/)


## 2. 元素种类分类

​	行内元素（display：inline）、块级元素（display：block）、行内块元素（display：inline-block）

### 2.1 行内元素

- 不会自动换行，当充满容器后，就回开始位置继续叠加显示

- 不能设置  `width` `height` 属性 可以指line-height 宽度随内容变化
- `margin` 水平有效，垂直无效
- `padding` 水平有效，垂直方向有特殊效果，影响视觉

|    标签    |               含义                |
| :--------: | :-------------------------------: |
|   **a**    |               锚点                |
|    abbr    |               缩写                |
|  acronym   |               首字                |
|     b      |           粗体(不推荐)            |
|    big     |              大字体               |
|     br     |               换行                |
|    cite    |               引用                |
|    code    |               代码                |
|    dfn     |             定义字段              |
|   **em**   |               强调                |
|    del     |               删除                |
|    font    |             字体设定              |
|     i      |               斜体                |
|    img     |               图片                |
| **input**  | 输入框 type="" value="" name="id" |
|    kbd     |           定义键盘文本            |
| **label**  |         表格标签 for="nc"         |
|     q      |              短引用               |
|     s      |              中划线               |
|    samp    |        定义范例计算机代码         |
|   select   |             项目选择              |
|   small    |            小字体文本             |
|  **span**  |      定义文本内区块（常用）       |
|   strike   |              中划线               |
| **strong** |             粗体强调              |
|    sub     |               下标                |
|    sup     |               上标                |
|  textarea  |          多行文本输入框           |
|     tt     |             电传文本              |
|   u/ins    |              下划线               |



### 2.2 块级元素

- 独占一行，使用多个块级标签时自动换行
- 可以设置 `width`、`height`、`margin`、`padding`等属性
- 宽度默认为 `100%`

|         标签         |                    含义                    |
| :------------------: | :----------------------------------------: |
|       address        |                    地址                    |
|      blockquote      |                   块引用                   |
|        center        |                 居中对齐块                 |
|         dir          |                  目录列表                  |
|       **div**        |                常用块级元素                |
|          dl          |                  定义列表                  |
|       fieldset       |                 form控制组                 |
|         form         | 交互表单 表单域action:提交到哪个页面，meth |
|      **h1~h6**       |                  各级标题                  |
|        **hr**        |                 水平分隔线                 |
|         menu         |                 菜单列表2                  |
|       noframes       |               frames可选内容               |
|       noscript       |                可选脚本内容                |
|          ol          |                  有序表单                  |
|          p           |                    段落                    |
|         pre          |                 格式化文本                 |
|        table         |           表格（display：table）           |
| tr行/td单元格/th表头 |                caption标题                 |
|        **ul**        |                  无序列表                  |



### 2.3 行内块元素

- 元素排在一行，会有空白缝隙
- 可以设置 `width`、`height`、`margin`、`padding`等属性
- 默认宽度随内容变化

|   标签   |     含义     |
| :------: | :----------: |
|  button  |     按钮     |
|  input   |    输入框    |
|   img    |     图片     |
|  iframe  |     框架     |
| textarea |  多行文本框  |
|  select  | 选项<option> |



三种元素都可以通过css改变display属性来切换

### 1.4 常见使用示例

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8" />
        <script>...</script>
        <script type="text/javascript" src="..."></script>
        <link>...</link>
    	<title>...</title>
    </head>
    <body>
        <img src="/..." alt="图片不错在替代文字" title="鼠标移动显示文字">
        <a href="/" target="_blank">连接在新页面打开</a>
    </body>
</html>
```

## 3.HTML5

### 3.1 新增语义化标签

|   标签    |    含义    |
| :-------: | :--------: |
| <header>  |  头部标签  |
|   <nav>   |  导航标签  |
| <article> |  内容标签  |
| <section> |  块级标签  |
|  <aside>  | 侧边栏标签 |
| <footer>  |  尾部标签  |

### 3.2 新增多媒体标签

|  标签   | 含义 |                             说明                             |
| :-----: | :--: | :----------------------------------------------------------: |
| <audio> | 音频 | 支持：Ogg、mp3、Wav<br /><audio src="文件地址" controls="controls"><source></audio><br />属性：autoplay(谷歌禁用)、controls、loop、src |
| <video> | 视频 | 支持：Ogg、MPEG4、WebM<br /><video src="/.."></video><br />属性：autoplay、controls、loop(循环)、muted(静音)、poster |

### 3.3 新增input表单属性
- type = "email" "url" "date" "time" "month" "week" "number" "tel" "search" "color" "autofocus"

### 3.4 canvas画布

