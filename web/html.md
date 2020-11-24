# HTML

## 1. 元素种类分类

​	行内元素（display：inline）、块级元素（display：block）、行内块元素（display：inline-block）

### 1.1 行内元素

- 不会自动换行，当充满容器后，就回开始位置继续叠加显示

- 不能设置  `width` `height` 属性 可以指line-height 宽度随内容变化
- `margin` 水平有效，垂直无效
- `padding` 水平有效，垂直方向有特殊效果，影响视觉

|   标签   |          含义          |
| :------: | :--------------------: |
|    a     |          锚点          |
|   abbr   |          缩写          |
| acronym  |          首字          |
|    b     |      粗体(不推荐)      |
|   big    |         大字体         |
|    br    |          换行          |
|   cite   |          引用          |
|   code   |          代码          |
|   dfn    |        定义字段        |
|    em    |          强调          |
|   font   |        字体设定        |
|    i     |          斜体          |
|   img    |          图片          |
|  input   |         输入框         |
|   kbd    |      定义键盘文本      |
|  label   |        表格标签        |
|    q     |         短引用         |
|    s     |         中划线         |
|   samp   |   定义范例计算机代码   |
|  select  |        项目选择        |
|  small   |       小字体文本       |
|   span   | 定义文本内区块（常用） |
|  strike  |         中划线         |
|  strong  |        粗体强调        |
|   sub    |          下标          |
|   sup    |          上标          |
| textarea |     多行文本输入框     |
|    tt    |        电传文本        |
|    u     |         下划线         |



### 1.2 块级元素

- 独占一行，使用多个块级标签时自动换行
- 可以设置 `width`、`height`、`margin`、`padding`等属性
- 宽度默认为 `100%`

|    标签    |          含义          |
| :--------: | :--------------------: |
|  address   |          地址          |
| blockquote |         块引用         |
|   center   |       居中对齐块       |
|    dir     |        目录列表        |
|    div     |      常用块级元素      |
|     dl     |        定义列表        |
|  fieldset  |       form控制组       |
|    form    |        交互表单        |
|   h1~h6    |        各级标题        |
|     hr     |       水平分隔线       |
|    menu    |        菜单列表        |
|  noframes  |     frames可选内容     |
|  noscript  |      可选脚本内容      |
|     ol     |        有序表单        |
|     p      |          段落          |
|    pre     |       格式化文本       |
|   table    | 表格（display：table） |
|     ul     |        无序列表        |



### 1.3 行内块元素

- 元素排在一行，会有空白缝隙
- 可以设置 `width`、`height`、`margin`、`padding`等属性
- 默认宽度随内容变化

|   标签   |    含义    |
| :------: | :--------: |
|  button  |    按钮    |
|  input   |   输入框   |
|   img    |    图片    |
|  iframe  |    框架    |
| textarea | 多行文本框 |
|  select  |    选项    |



三种元素都可以通过css改变display属性来切换

