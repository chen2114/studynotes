---
title: 【学习笔记】之echart
tags: [学习笔记, echart]
categories: [学习笔记]
---
## 使用
### 方法一
``` js
import Vue from 'vue'
import echarts from 'echarts'

Vue.prototype.$echarts = echarts
```
``` vue
<template>
  <div ref="pieChart"></div>
</template>
<script>
export default {
  data () {
    return {
      myChart: null
    }
  },
  methods: {
    initChart () {
      const that = this
      this.myChart = this.$echarts.init(this.$refs.pieChart)
      let option = {
        ...
      }
      this.myChart.setOption(option)
    }
  }
}
</script>
```
### 方法二
``` js
import Vue from 'vue'
// 安装 vue-echarts-v3
import IEcharts from 'vue-echarts-v3/src/lite.js'
// 安装 echarts
import 'echarts/lib/chart/bar'
import 'echarts/lib/chart/pie'

Vue.component('v-chart', IEcharts)
```
``` vue
<template>
  <v-chart :option="option" />
</template>
<script>
export default {
  data () {
    return {
      option: {
        ...
      }
    }
  }
}
</script>
```
## 配置 option
### title
标题组件，包含主标题和副标题
#### __title__.textStyle __| *__
##### __title__.__textStyle__.rich __| Object__
在`rich`里面，可以自定义富文本样式。利用富文本样式，可以在标签中做出非常丰富的效果。

例如：
``` js
label: {
  // 在文本中，可以对部分文本采用 rich 中定义样式。
  // 这里需要在文本中使用标记符号：
  // `{styleName|text content text content}` 标记样式名。
  // 注意，换行仍是使用 '\n'。
  formatter: [
    '{a|这段文本采用样式a}',
    '{b|这段文本采用样式b}这段用默认样式{x|这段用样式x}'
  ].join('\n'),

  rich: {
    a: {
      color: 'red',
      lineHeight: 10
    },
    b: {
      backgroundColor: {
        image: 'xxx/xxx.jpg'
      },
      height: 40,
      align: 'center'
    },
    x: {
      fontSize: 18,
      fontFamily: 'Microsoft YaHei',
      borderColor: '#449933',
      borderRadius: 4
    },
    ...
  }
}
```
### legend
图例组件
#### __legend__.type __| string__
图例的类型。可选值：
- `plain`：普通图例。缺省就是普通图例。
- `scroll`：可滚动翻页的图例。当图例数量较多时可以使用。
### grid
直角坐标系内绘图网格，单个 grid 内最多可以放置上下两个 X 轴，左右两个 Y 轴。可以在网格上绘制折线图，柱状图，散点图（气泡图）。
#### __grid__.show __| boolean__
[ default: false ]

是否显示直角坐标系网格
#### __grid__.containLabel __| boolean__
[ default: false ]

grid 区域是否包含坐标轴的刻度标签，这常用于『防止标签溢出』的场景
### xAxis
直角坐标系 grid 中的 x 轴
#### __xAxis__.type __| string__
[ default: 'category' ]

坐标轴类型。

可选：

- `value`: 数值轴，适用于连续数据。
- `category`: 类目轴，适用于离散的类目数据，为该类型时必须通过`data`设置类目数据。
- `time`: 时间轴，适用于连续的时序数据，与数值轴相比时间轴带有时间的格式化，在刻度计算上也有所不同，例如会根据跨度的范围来决定使用月，星期，日还是小时范围的刻度。
- `log`: 对数轴。适用于对数数据。
#### __xAxis__.data[i] __| Object__
类目数据，在类目轴（type: 'category'）中有效。

示例：
``` js
// 所有类目名称列表
data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
// 每一项也可以是具体的配置项，此时取配置项中的 `value` 为类目名
data: [{
  value: '周一',
  // 突出周一
  textStyle: {
    fontSize: 20,
    color: 'red'
  }
}, '周二', '周三', '周四', '周五', '周六', '周日']
```
### yAxis
直角坐标系 grid 中的 y 轴
### polar
极坐标系，可以用于散点图和折线图
### radiusAxis
极坐标系的径向轴
### angleAxis
极坐标系的角度轴
### radar
雷达图坐标系组件，只适用于雷达图
### dataZoom[i]
用于区域缩放
### visualMap[i]
视觉映射组件，用于进行『视觉编码』
### tooltip
提示框组件，可以设置在多种地方：
全局 tooltip，坐标系 grid.tooltip、polar.tooltip、single.tooltip，系列 series.tooltip，每个数据项 series.data.tooltip
#### __tooltip__.trigger __| string__
[ default: 'item' ]

触发类型。

可选：
- `item`: 数据项图形触发，主要在散点图，饼图等无类目轴的图表中使用。
- `axis`: 坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。
- `none`: 什么都不触发。
#### __tooltip__.triggerOn __| string__
[ default: 'mousemove|click' ]

提示框触发的条件，可选：
- `mousemove`: 鼠标移动时触发。
- `click`: 鼠标点击时触发。
- `mousemove|click`: 同时鼠标移动和点击时触发。
- `none`: 不在`mousemove`或`click`时触发，用户可以通过`action.tooltip.showTip`和`action.tooltip.hideTip`来手动触发和隐藏。也可以通过`axisPointer.handle`来触发或隐藏。
#### __tooltip__.formatter __| string, Function__
提示框浮层内容格式器，支持字符串模板和回调函数两种形式。
1. 字符串模板
- 折线（区域）图、柱状（条形）图、K线图: `{a}`（系列名称），`{b}`（类目值），`{c}`（数值）, `{d}`（无）
- 散点图（气泡）图: `{a}`（系列名称），`{b}`（数据名称），`{c}`（数值数组）, `{d}`（无）
- 地图: `{a}`（系列名称），`{b}`（区域名称），`{c}`（合并数值）, `{d}`（无）
- 饼图、仪表盘、漏斗图: `{a}`（系列名称），`{b}`（数据项名称），`{c}`（数值）, `{d}`（百分比）
### axisPointer
这是坐标轴指示器（axisPointer）的全局公用设置，效果：鼠标悬浮到图上，可以出现标线和刻度文本
#### __axisPointer__.triggerOn __| string__
[ default: 'mousemove|click' ]

提示框触发的条件，可选：
- `mousemove`: 鼠标移动时触发。
- `click`: 鼠标点击时触发。
- `none`: 不在 'mousemove' 或 'click' 时触发。
### toolbox
工具栏。内置有导出图片，数据视图，动态类型切换，数据区域缩放，重置五个工具
### brush
区域选择组件，用户可以选择图中一部分数据，从而便于向用户展示被选中数据，或者他们的一些统计计算结果
### geo
地理坐标系组件，用于地图的绘制，支持在地理坐标系上绘制散点图，线集
### parallel
平行坐标系
### parallelAxis
平行坐标系中的坐标轴
### singleAxis
单轴。可以被应用到散点图中展现一维数据
### timeline
提供了在多个 ECharts option 间进行切换、播放等操作的功能
### graphic
原生图形元素组件。可以支持的图形元素包括：image, text, circle, sector, ring, polygon, polyline, rect, line, bezierCurve, arc, group
### calendar
日历坐标系组件
### dataset
数据集（dataset）组件用于单独的数据集声明，从而数据可以单独管理，被多个组件复用，并且可以自由指定数据到视觉的映射
### aria
自动智能生成关于图表的描述
### series[i]
系列列表。每个系列通过 type 决定自己的图表类型
#### series[i]-line
折线/面积图
##### __series[i]-line__.emphasis __| Object__
图形的高亮样式。
##### __series[i]-line__.data[i] __| Object__
系列中的数据内容数组。数组项通常为具体的数据项。

当需要对个别数据进行个性化定义时：
``` js
[
  12,
  34,
  {
    value: 56,
    //自定义标签样式，仅对该数据项有效
    label: {},
    //自定义特殊 itemStyle，仅对该数据项有效
    itemStyle: {}
  },
  10
]
```
#### series[i]-bar
柱状/条形图
##### __series[i]-bar__.barGap __| string__
[ default: 30% ]

不同系列的柱间距离，为百分比（如 '30%'，表示柱子宽度的 30%）。

如果想要两个系列的柱子重叠，可以设置 barGap 为 '-100%'。这在用柱子做背景的时候有用。
##### __series[i]-bar__.barCategoryGap __| string__
[ default: '20%' ]

同一系列的柱间距离，默认为类目间距的20%，可设固定值
#### series[i]-pie
饼图
##### __series[i]-pie__.center __| Array__
[ default: ['50%', '50%'] ]

调整饼图在容器中的位置，默认为容器中心，设置成百分比时第一项是相对于容器宽度，第二项是相对于容器高度。
##### __series[i]-pie__.radius __| number, string, Array__
[ default: [0, '75%'] ]

饼图的半径。可以为如下类型：
- `number`: 直接指定外半径值。
- `string`: 例如，'20%'，表示外半径为容器高宽中较小一项的 20% 长度。
- `Array.<number|string>`: 数组的第一项是内半径，第二项是外半径。每一项遵从上述 number string 的描述。

可以将内半径设大显示成圆环图（Donut chart）。
##### __series[i]-pie__.data[i] __| Object__
需要对个别内容指定进行个性化定义时：
``` js
[{
  name: '数据1',
  value: 10
}, {
  // 数据项名称
  name: '数据2',
  value : 56,
  //自定义特殊 tooltip，仅对该数据项有效
  tooltip:{},
  //自定义特殊itemStyle，仅对该item有效
  itemStyle:{}
}]
```
### color
调色盘颜色列表。如果系列没有设置颜色，则会依次循环从该列表中取颜色作为系列颜色。<br>
默认为：

['#c23531', '#2f4554', '#61a0a8', '#d48265', '#91c7ae', '#749f83', '#ca8622', '#bda29a','#6e7074', '#546570', '#c4ccd3']
### backgroundColor
背景色，默认无背景
### textStyle
全局的字体样式
### animation
是否开启动画
### animationThreshold
是否开启动画的阈值，当单个系列显示的图形数量大于这个阈值时会关闭动画
### animationDuration
初始动画的时长，支持回调函数，可以通过每个数据返回不同的 delay 时间实现更戏剧的初始动画效果：
``` js
animationDuration: function (idx) {
  // 越往后的数据延迟越大
  return idx * 100;
}
```
### animationEasing
初始动画的缓动效果
### animationDelay
初始动画的延迟，支持回调函数，可以通过每个数据返回不同的 delay 时间实现更戏剧的初始动画效果：
``` js
animationDelay: function (idx) {
  // 越往后的数据延迟越大
  return idx * 100;
}
```
### animationDurationUpdate
数据更新动画的时长。<br>
支持回调函数，可以通过每个数据返回不同的 delay 时间实现更戏剧的更新动画效果：
``` js
animationDurationUpdate: function (idx) {
  // 越往后的数据延迟越大
  return idx * 100;
}
```
### animationEasingUpdate
数据更新动画的缓动效果
### animationDelayUpdate
数据更新动画的延迟，支持回调函数，可以通过每个数据返回不同的 delay 时间实现更戏剧的更新动画效果：
``` js
animationDelayUpdate: function (idx) {
  // 越往后的数据延迟越大
  return idx * 100;
}
```
### blendMode
图形的混合模式，默认为`source-over`。 支持每个系列单独设置。<br>
`lighter`也是比较常见的一种混合模式，该模式下图形数量集中的区域会颜色叠加成高亮度的颜色（白色）。常常能起到突出该区域的效果。
### hoverLayerThreshold
图形数量阈值，决定是否开启单独的 hover 层，在整个图表的图形数量大于该阈值时开启单独的 hover 层。<br>
单独的 hover 层主要是为了在高亮图形的时候不需要重绘整个图表，只需要把高亮的图形放入单独的一个 canvas 层进行绘制，防止在图形数量很多的时候因为高亮重绘所有图形导致卡顿。
### useUTC
是否使用 UTC 时间。
- `true`: 表示`axis.type`为`time`时，依据 UTC 时间确定 tick 位置，并且`axisLabel`和`tooltip`默认展示的是 UTC 时间
- `false`: 表示`axis.type`为`time`时，依据本地时间确定 tick 位置，并且`axisLabel`和`tooltip`默认展示的是本地时间
## API
### echarts
全局 echarts 对象，在 script 标签引入 echarts.js 文件后获得，或者在 AMD 环境中通过 require('echarts') 获得。
### echartsInstance
通过 `echarts.init` 创建的实例。
### action
ECharts 中支持的图表行为，通过 dispatchAction 触发。
### events
在 ECharts 中主要通过 on 方法添加事件处理函数。