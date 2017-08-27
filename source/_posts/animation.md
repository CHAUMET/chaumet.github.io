---
title: css3动画效果的实现
date: 2016-12-19 11:09:21
tags: CSS3
---

## 过渡

>过渡是CSS3中具有颠覆性的特征之一，可以实现元素不同状态间的平滑过渡（补间动画），经常用来制作动画效果。

在CSS3里使用 `transition` (n.过渡）可以实现补间动画（[更多补间动画知识](http://mux.alimama.com/posts/1009)）
并且当前元素只要有“属性”发生变化时即存在两种状态，就可以实现平滑的过渡

```javascript

transition ：property（过渡属性） duration（过渡时间） time-function（过渡速度） delay（过渡延时）
//           常用all             动画持续时间         不常用                   看情况写数值

```

## 2D转换

>转换是CSS3中具有颠覆性的特征之一，可以实现元素的位移、旋转、变形、缩放，甚至支持矩阵方式，配合过渡和即将学习的动画知识，可以取代大量之前只能靠Flash才可以实现的效果。

### 移动 translate(x, y)

(v.转化 需要和transition作对比 容易混淆 可以按照一个动词一个名词来区分)

可以改变元素的位置，x、y可为负值；
- 移动位置相当于自身原来位置
- y轴正方向朝下
-  除了可以像素值，也可以是百分比，相对于自身的宽度或高度

### 缩放 scale(x, y)

可以对元素进行水平和垂直方向的缩放，x、y的取值可为小数；

### 旋转 rotate(deg)

可以对元素进行旋转，正值为顺时针，负值为逆时针；

- 当元素旋转以后，坐标轴也跟着发生的转变
- 调整顺序可以解决，把旋转放到最后

### 倾斜 skew(deg, deg)

可以使元素按一定的角度进行倾斜，可为负值，第二个参数不写默认为0。

### 矩阵matrix()

把所有的2D转换组合到一起，需要6个参数

[关于矩阵的学习资料](http://www.zhangxinxu.com/wordpress/2012/06/css3-transform-matrix-%E7%9F%A9%E9%98%B5/comment-page-2/)

### transform-origin

可以调整元素转换的原点

##### 我们可以同时使用多个转换，其格式为：transform: translate() rotate() scale() ...等，其顺序会影响转换的效果。

### 易混淆单词

- transform v. 转换 用于多个转换同时使用
- transition n. 过渡 用于设置补间动画 一般transition和transform会同时存在
- translate v.转化 transform 中的一种转换属性

## 动画

>动画是CSS3中具有颠覆性的特征之一，可通过设置多个节点来精确控制一个或一组动画，常用来实现复杂的动画效果。

### 必要元素：

a、通过@keyframes指定动画序列；
b、通过百分比将动画序列分割成多个节点；
c、在各节点中分别定义各属性
d、通过animation将动画应用于相应元素；

```javascript

.box img{
    animation: move 2s infinete liner;
}
@keyframe move{

    0%{
        transform: rotate(0deg);
    }
    100%{
        transform: rotate(360deg);
    }
}

```
### 关键属性

a、animation-name设置动画序列名称
b、animation-duration动画持续时间
c、animation-delay动画延时时间
d、animation-timing-function动画执行速度，linear、ease等
e、animation-play-state动画播放状态，running、paused等
f、animation-direction动画逆播，alternate等
g、animation-fill-mode动画执行完毕后状态，forwards、backwards等
h、animation-iteration-count动画执行次数，inifinate等
i、steps(60) 表示动画分成60步完成

参数值的顺序：
关于几个值，除了名字，动画时间，延时有严格顺序要求其它随意
