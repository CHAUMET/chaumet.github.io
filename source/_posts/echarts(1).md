---
title: 使用ECharts3制作温度折线表格
date: 2016-12-21 20:03:11
tags: 插件
---

本篇文章主要记录了在制作折线图过程中的实现方法及总结,更完整的使用方法请参考[ECharts文档](http://echarts.baidu.com/tutorial.html#ECharts%20%E7%89%B9%E6%80%A7%E4%BB%8B%E7%BB%8D)

### （一）获取ECharts
因为是开发环境，我选择了[源代码版本](http://echarts.baidu.com/dist/echarts.js),也可以从[官网下载界面](http://echarts.baidu.com/download.html)选择你需要的版本下载
根据开发者功能和体积上的需求，提供了不同打包的下载.

### (二) 引入 ECharts
ECharts 3 只需要像普通的 JavaScript 库一样用 script 标签引入。

````javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <!-- 引入 ECharts 文件 -->
    <script src="echarts.min.js"></script>
</head>
</html>
````
### (三) 为 ECharts 准备一个具备高宽的 DOM 容器

```javascript
<body>
    <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
    <div id="main" style="width: 600px;height:400px;"></div>
</body>
```
ECharts完善了移动端自适应的能力，如果是制作一个固定宽高的表格，只需给dom容器一个固定的宽高，
EChart在检测到屏幕变化之后会相应调整表格内各内容的大小。

在这里，因为我要制作的是一个移动端长条形的温度折现表格，表格内容在ui设计稿中是要超出屏幕宽度范围的，
所以不能使用数据区域缩放组件（dataZoom），需要强行制定表格的宽高。

```javascript
<div id="wrapper2">
    <div id="main" style="width: 35rem;height:5.64rem;"></div>
</div>
```
##### ECharts组件的定位和布局

大部分『组件』和『系列』会遵循两种定位方式：
+ left/right/top/bottom/width/height 定位方式：

这六个量中，每个量都可以是『绝对值』或者『百分比』或者『位置描述』。

   - 绝对值:单位是浏览器像素（px），用 number 形式书写（不写单位）。例如 {left: 23, height: 400}。
   - 百分比:表示占 DOM 容器高宽的百分之多少，用 string 形式书写。例如 {right: '30%', bottom: '40%'}。
   - 位置描述:可以设置 left: 'center'，表示水平居中。可以设置 top: 'middle'，表示垂直居中。

这六个量的概念，和 CSS 中六个量的概念类似：

left：距离 DOM 容器左边界的距离。
right：距离 DOM 容器右边界的距离。
top：距离 DOM 容器上边界的距离。
bottom：距离 DOM 容器下边界的距离。
width：宽度。
height：高度。

在横向，left、right、width 三个量中，只需两个量有值即可，因为任两个量可以决定组件的位置和大小，例如 left 和 right 或者 right 和 width 都可以决定组件的位置和大小。 纵向，top、bottom、height 三个量，和横向类同不赘述。

+ center / radius 定位方式：
   - center:是一个数组，表示 [x, y]，其中，x、y可以是『绝对值』或者『百分比』，含义和前述相同。
   - radius:是一个数组，表示 [内半径, 外半径]，其中，内外半径可以是『绝对值』或者『百分比』，含义和前述相同。

在自适应容器大小时，百分比设置是很有用的。

### (四) 绘制一个简单的图表

到这一步，官网给出的是一个简单的柱状图完整代码，其实这一步直接在ECharts的[官方示例](http://echarts.baidu.com/examples.html)或[Gallery](http://gallery.echartsjs.com/explore.html#sort=rank~timeframe=all~author=all)(Echarts用户制作并提交的表格展示页面)中，
寻找与你需要制作的表格类型、内容、样式相似的表格，点进去复制他的源代码进行修改，会很好的帮助新手学习如何使用ECharts制作表格。
```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ECharts</title>
    <!-- 引入 echarts.js -->
    <script src="echarts.min.js"></script>
</head>
<body>
    <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
        // 基于准备好的dom，初始化echarts实例
        var myChart = echarts.init(document.getElementById('main'));

        // 指定图表的配置项和数据
        var option = {
            title: {
                text: 'ECharts 入门示例'
            },
            tooltip: {},
            legend: {
                data:['销量']
            },
            xAxis: {
                data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
            },
            yAxis: {},
            series: [{
                name: '销量',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
        };

        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option);
    </script>
</body>
</html>
```
接下来就是各种配置项参数的调整。

+ title :标题组件，包含主标题和副标题。
    - 标题默认左上方显示，可设置主、副标题
    - 标题可设置对齐方式、超链接、内边距、两标题之间的距离、层级、组件外边距
    - 标题背景色默认透明，边框的颜色宽度、阴影的颜色、偏移、模糊都可以进行修改

+ tooltip：鼠标在移动或点击时在表格内部出现的，显示当前坐标所包含的具体信息的组件
    - 提示框组件包括提示框浮层和坐标轴指示器配置项
    - 组件是否限制在表格范围内可设置
    - 提示框浮层
        + 默认显示，触发类型需要根据表格类型选择，触发条件默认为鼠标移动时触发。
        + 可设置是否永远显示、显示隐藏浮层时的延迟时间、鼠标是否可以进入浮层、浮层的位置
        + 提供了一定的css配置项，也可以添加额外的css样式
    - 坐标轴指示器
        + 默认指示器类型为直线
        + 可添加动画效果
    - 支持对个别数据进行个性化定义

+ legend ：图例组件 （表格顶部标注不同图例的组件）
    - 图例组件展现了不同系列的标记(symbol)，颜色和名字。可以通过点击图例控制哪些系列不显示。ECharts 3 中单个 echarts 实例中可以存在多个图例组件，会方便多个图例的布局。
    - 修改层级、四周距离、宽高、布局朝向、对齐方式、图例选中和关闭时的颜色
    - 标题背景色、边框的颜色宽度、阴影的颜色、偏移、模糊都可以进行修改

+ xAxis/ yAxis ：坐标轴的相关配置
    - type：
        + 'value' 数值轴，适用于连续数据。
            - 一般结合min、max、interval来控制y轴坐标
        + 'category' 类目轴，适用于离散的类目数据，为该类型时必须通过 data 设置类目数据。
        + 'time' 时间轴，适用于连续的时序数据，与数值轴相比时间轴带有时间的格式化，在刻度计算上也有所不同，例如会根据跨度的范围来决定使用月，星期，日还是小时范围的刻度。
        + 'log' 对数轴。适用于对数数据。
    -  data:
        + 类目数据，在类目轴（type: 'category'）中有效。
        ```javascript
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
    - boundaryGap:坐标轴两边留白，默认true
    - splitLine：坐标轴在表格区域中的分隔线，默认数值轴显示，类目轴不显示。
        + interval：坐标轴分隔线的显示间隔，在类目轴中有效
            - 默认会采用标签不重叠的策略间隔显示标签。
            - 可以设置成 0 强制显示所有标签。
            - 如果设置为 1，表示『隔一个标签显示一个标签』，如果值为 2，表示隔两个标签显示一个标签，以此类推。
            - 可以用数值表示间隔的数据，也可以通过回调函数控制。
        + linestyle.color:分割线的颜色，可单色，也可设置成颜色数组，分隔线会按数组中颜色的顺序依次循环设置颜色。
        + type：'solid' 'dashed' 'dotted'
        + 阴影颜色、偏移、图形透明度
    - axisTick: 坐标轴刻度相关设置。
    - axisLine: 坐标轴轴线相关设置
        + 默认显示坐标轴轴线
        + onZero :X 轴或者 Y 轴的轴线是否在另一个轴的 0 刻度上，只有在另一个轴为数值轴且包含 0 刻度时有效。默认true。
        + linestyle.color:坐标轴线颜色 （会影响坐标轴刻度的显示）
            - 颜色可以使用 RGB 表示，比如 'rgb(128, 128, 128)'，如果想要加上 alpha 通道表示不透明度，可以使用 RGBA，比如 'rgba(128, 128, 128, 0.5)'，也可以使用十六进制格式，比如 '#ccc'。
            - 除了纯色之外颜色也支持渐变色和纹理填充
        ```javascript
        // 线性渐变，前四个参数分别是 x0, y0, x2, y2, 范围从 0 - 1，相当于在图形包围盒中的百分比，如果最后一个参数传 `true`，则该四个值是绝对的像素位置
         color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
             offset: 0, color: 'red' // 0% 处的颜色
             }, {
             offset: 1, color: 'blue' // 100% 处的颜色
             }], false)

         //径向渐变，前三个参数分别是圆心 x, y 和半径，取值同线性渐变
        color: new echarts.graphic.RadialGradient(0.5, 0.5, 0.5, [...], false)

        // 纹理填充
        color: new echarts.graphic.Pattern(
          imageDom, // 支持为 HTMLImageElement, HTMLCanvasElement，不支持路径字符串
          'repeat' // 是否平铺, 可以是 repeat-x, repeat-y, no-repeat
        )

          ```
    - axisLabel: 坐标轴刻度标签的相关设置。
        + show：默认显示坐标轴刻度，
        + inside：刻度标签是否朝内，默认朝外。
        + rotate：刻度标签旋转的角度，在类目轴的类目标签显示不下的时候可以通过旋转防止标签之间重叠。旋转的角度从 -90 度到 90 度。
        + margin：刻度标签与轴线之间的距离。
        + formatter：刻度标签的内容格式器，支持字符串模板和回调函数两种形式。
        ```javascript
        // 使用字符串模板，模板变量为刻度默认标签 {value}
        formatter: '{value} kg'

        // 使用函数模板，函数参数分别为刻度数值（类目），刻度的索引
        formatter: function (value, index) {
            // 格式化成月/日，只在第一个刻度显示年份
            var date = new Date(value);
            var texts = [(date.getMonth() + 1), date.getDate()];
            if (index === 0) {
                texts.unshift(date.getYear());
            }
            return texts.join('/');
        }
        ```
    - series:系列列表。每个系列通过 type 决定自己的图表类型
        + 这里主要用了两种列表 line 和 pictorialBar
        + line：折线/面积图
            - coordinateSystem：可选择二维直角坐标系或极坐标系
            - 可存在多个x、y轴，适用标记，阶梯线图，各种样式的修改
            - smooth：折现平滑显示 默认false
            - showSymbol：是否显示坐标点 默认true
            - data：系列中的数据内容数组。数组项通常为具体的数据项。
                    通常来说，数据用一个二维数组表示。如下，每一列被称为一个『维度』。
                 ```javascript
                 series: [{
                      data: [
                          // 维度X   维度Y   其他维度 ...
                          [  3.4,    4.5,   15,   43],
                          [  4.2,    2.3,   20,   91],
                          [  10.8,   9.5,   30,   18],
                          [  7.2,    8.8,   18,   57]
                      ]
                  }]
                 //特别地，当只有一个轴为类目轴（axis.type 为 'category'）的时候，数据可以简化用一个一维数组表示。例如：
                 xAxis: {
                         data: ['a', 'b', 'm', 'n']
                     },
                     series: [{
                         // 与 xAxis.data 一一对应。
                         data: [23,  44,  55,  19]
                         // 它其实是下面这种形式的简化：
                         // data: [[0, 23], [1, 44], [2, 55], [3, 19]]
                     }]
                 ```
            - areaStyle: 区域填充样式。
                + 支持rgba 线性渐变 径向渐变 填充纹理 阴影 透明度
            - itemStyle： 折线拐点标志的样式。
            - lineStyle： 线条样式。
                + 修改 lineStyle 中的颜色不会影响图例颜色，如果需要图例颜色和折线图颜色一致，需修改 itemStyle.normal.color，线条颜色默认也会取改颜色。
                + 与areaStyle一样 支持各种颜色填充
        + pictorialBar：象形柱图
            - 象形柱图是可以设置各种具象图形元素（如图片、SVG PathData 等）的柱状图。往往用在信息图中。用于有至少一个类目轴或时间轴的直角坐标系上。
            - barGap: 柱间距离，可设固定值（如 20）或者百分比（如 '30%'，表示柱子宽度的 30%）如果想要两个系列的柱子重叠，可以设置 barGap 为 '-100%'。这在用柱子做背景的时候有用。
            - symbolPosition：每个象形图形根据基准柱的定位，是通过 symbolPosition、symbolOffset 来调整其于基准柱的相对位置。
            ```javascript
             type: 'pictorialBar',                   //象形柱图是可以设置各种具象图形元素（如图片、SVG PathData 等）的柱状图。往往用在信息图中。用于有至少一个类目轴或时间轴的直角坐标系上。
                    barGap: '-100%',                        // 柱间距离，可设固定值（如 20）或者百分比（如 '30%'，表示柱子宽度的 30%）如果想要两个系列的柱子重叠，可以设置 barGap 为 '-100%'。这在用柱子做背景的时候有用。
                    symbolPosition: 'end',                  // 图形的定位位置
                    data: [{
                        value: 35,                            //确定图形在什么高度显示
                        symbol:'image://images/sunrise.png',  // 链接
                        symbolSize: [21, 28],                 //大小
                        symbolOffset: [360,'-120%']           //距离
                    }, {
                        value: 35,
                        symbol: 'image://images/sunset.png',
                        symbolSize: [21, 28],
                        symbolOffset: [1100,'-120%']
                    }, {
                        value: 35,
                        symbol:'image://images/rainicon.png',
                        symbolSize:[21, 28],
                        symbolOffset: [475,'-120%']
                    }]
            ```























