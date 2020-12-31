# Ant View

```init
更新时间：2020年12月14日11:03:44
说明：antv开发过程中的api问题记录
```



## 1. [G2Plot](https://g2plot.antv.vision/zh/docs/manual/introduction)

### 1.1 安装

```shell
npm install @antv/g2plot --save
```

### 1.2 实例

```javascript
<div id="container"></div>
const data = [
  { year: '1991', value: 3 },
  { year: '1992', value: 4 }
]
const line = new Line('container', {
  data,
  xField: 'year',
  yField: 'value',
});
line.render();// 初始化的时候就render()

line.update();// 数据更新的时候update

```

### 1.3 个性化设置

```javascript
const line = new Line('container', {
  data,
  xField: 'year',
  yField: 'value',
  // 自定义折线颜色
  color: '#FE740C',
  // 更改辅助数据点大小及样式
  point: {
    size: 5,
    shape: 'diamond',
    style: {
      stroke: '#FE740C',
      lineWidth: 2,
      fillOpacity: 0.6,
    },
  },
  yAxis: {
    // 格式化 y 轴标签加单位，自定义 labal 样式
    label: {
      formatter: (v) => {
        return v + 'k';
      },
      style: {
        fill: '#FE740C',
      },
    },
  },
  // 添加label
  label: {
    fill: '#FE740C',
  },
  // 添加辅助文本、辅助线
  annotations: [
    {
      type: 'text',
      position: ['min', 'median'],
      content: '辅助标记',
      offsetY: -4,
      style: {
        textBaseline: 'bottom',
      },
    },
    {
      type: 'line',
      start: ['min', 'median'],
      end: ['max', 'median'],
      style: {
        stroke: 'red',
        lineDash: [2, 2],
      },
    },
  ],
});

// element 添加点击事件
line.on('element:click', (e) => {
  console.log(e);
});

// annotation 添加点击事件
line.on('annotation:click', (e) => {
  console.log(e);
});

// axis-label 添加点击事件
line.on('axis-label:click', (e) => {
  console.log(e);
});
line.render();
```

### 1.4 通用API

```javascript
// 创建图表实例
import {Line} from '@antv/g2plot';
const plot = new Line('container',options);
// container:HTMLElement:图表渲染的DOM容器
// options:PlotOptions:图表当前的所有全量配置项options

//1. 渲染到DOM
plot.render();
//2. 增量的更新图表配置
plot.update(options:Partial<PlotOptions>);
//3. 修改图表数据，自动重新渲染
plot.changeData(data:object[] | number);
//4. 手动指定图表的大小 图表配置autoFit = true 
plot.changeSize(width:number,height:number);
//5. 完全销毁画布
plot.destroy();
//6. 多次监听某一个图表事件
plot.on(event:string,callback:Function);
//7. 监听一次 触发之后销毁
plot.once(event:string,callback:Function);
//8. 解除事件的监听 不传参数，解绑所有事件监听
plot.off(event?:string,callback?:Function);
//9. 设定状态
plot.setState(state?: 'active' | 'inactive' | 'selected', condition?: Function, status: boolean = true);
//10. 获取状态
plot.getStates();
```

- #### 配置图形样式：lineStyle、columnStyle、label.style、axis.line.style等

  | 属性名        | 类型            | 介绍                                                         |
  | ------------- | --------------- | ------------------------------------------------------------ |
  | fill          | string          | 图形的填充色                                                 |
  | fillOpacity   | number          | 图形的填充透明度                                             |
  | stroke        | string          | 图形的描边                                                   |
  | lineWidth     | number          | 图形描边的宽度                                               |
  | lineDash      | [number,number] | 描边虚线配置，第一个值为虚线分段长度，第二个值为分段间隔距离 |
  | lineOpacity   | number          | 描边透明度                                                   |
  | opacity       | number          | 图形的整体透明度                                             |
  | shadowColor   | string          | 图形阴影颜色                                                 |
  | shadowBlur    | number          | 图形阴影的高斯模糊系数                                       |
  | shadowOffsetX | number          | 设置阴影距图形的水平距离                                     |
  | shadowOffsetY | number          | 设置阴影距图形的垂直距离                                     |
  | cursor        | string          | 鼠标样式。同 css 的鼠标样式，默认 'default'。                |

  示例

  ```json
    columnStyle: {
      fill: 'red',
      fillOpacity: 0.5,
      stroke: 'black',
      lineWidth: 1,
      lineDash: [4,5],
      strokeOpacity: 0.7,
      shadowColor: 'black',
      shadowBlur: 10,
      shadowOffsetX: 5,
      shadowOffsetY: 5,
      cursor: 'pointer'
    }
  ```

- #### 配置线的样式

  | 属性名        | 类型            | 介绍                     |
  | ------------- | --------------- | ------------------------ |
  | stroke        | string          | 线的颜色                 |
  | lineWidth     | number          | 线宽                     |
  | lineDash      | [number,number] | 虚线配置                 |
  | opacity       | number          | 透明度                   |
  | shadowColor   | string          | 阴影颜色                 |
  | shadowBlur    | number          | 高斯模糊系数             |
  | shadowOffsetX | number          | 设置阴影距图形的水平距离 |
  | shadowOffsetY | number          | 设置阴影距图形的垂直距离 |
  | cursor        | string          | 鼠标样式                 |

  示例

  ```json
  lineStyle: {
    stroke: 'black',
    lineWidth: 2,
    lineDash: [4,5],
    strokeOpacity: 0.7,
    shadowColor: 'black',
    shadowBlur: 10,
    shadowOffsetX: 5,
    shadowOffsetY: 5,
    cursor: 'pointer'
  }
  ```

- #### 配置文字样式

  | 属性名        | 类型            | 介绍                     |
  | ------------- | --------------- | ------------------------ |
  | fontSize      | number          | 文字大小                 |
  | fontFamily    | string          | 文字字体                 |
  | fontWeight    | number          | 文字粗细                 |
  | lineHeight    | number          | 文字行高                 |
  | textAlign     | string          | 设置文本内容对其方式     |
  | textBaseline  | string          | 当前文本基线，top        |
  | fill          | string          | 文字填充色               |
  | fillOpacity   | number          | 文字填充透明度           |
  | stroke        | string          | 文字描边                 |
  | lineWidth     | number          | 描边宽度                 |
  | lineDash      | [number,number] | 描边的虚线配置           |
  | lineOpacity   | number          | 描边透明度               |
  | opacity       | number          | 文字整体透明度           |
  | shadowColor   | string          | 文字阴影颜色             |
  | shadowBlur    | number          | 文字阴影的高斯模糊系数   |
  | shadowOffsetX | number          | 设置阴影距文字的水平距离 |
  | shadowOffsetY | number          | 设置阴影距文字的垂直距离 |
  | cursor        | string          | string                   |

  示例

  ```json
  statistic: {
    style:{
      fontSize: 80,
      fontWeight: 300,
      textAlign: 'center',
      textBaseline: 'middle',
      shadowColor: 'white',
      shadowBlur: 10,
    }
  }
  ```


### 1.5 问题总结

- 图表tooltip中padding样式变形问题？

  1. 分析原因：div组件中设置了子孙元素选择器，ul
  2. 解决方案：css样式中所有下一级的ul样式都 固定为子元素选择器 >ul>li
- 图表legend中修改item文字样式问题？
  1. legend: {itemName:{style:{fill:'color'}}}
2. 文字颜色统一使用fill属性
  
  

