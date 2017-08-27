---
title: 各种居中方式
date: 2016-12-19 15:16:37
tags: 方法总结
---

## 方法一 使用flex

父盒子： display：flex；

子元素： margin：auto；

随意改变宽高，一直居中。较方便

## 方法二  使用定位

父盒子： position：relative；

子元素： position： absolute；
        left：50%；
        top：50%；
        margin: -高度的一半 0 0 -宽度的一半；

改变宽高子元素便不再居中，有局限性

改进方案： 将margin用transform替代

        transform： translate（-50% ， -50%）；

改进之后，居中的位置不受元素的宽高所影响

## 方法三 vertical-align

这一方法常针对图片居中使用

 初始结构如下：
```javascript

<div class="thumb">
    <img src="图片链接" >
<div>
```
 给盒子设置好样式之后开始添加属性，实现水平和垂直的居中

 1. 父盒子添加text-align：center
 2. 给img加一个兄弟元素span
```javascript

<div class="thumb">
    <img src="图片链接" >
    <span></span>
<div>
```
 3. span样式设置：

 ```javascript
 .thumb span{
    display：inline-block；
    height：100%；
 }
 ```

 4. 给img设置样式：
 ```javascript
 .thumb span{
    vertical-align:middle;
 }
```
此时无论怎样改变图片大小，图片一直是水平垂直居中的

##### 原理解释：vertical-align，是相对于兄弟元素的中线做垂直对齐，只有img和外面的盒子的时候是无法让img居中的

#### 不想用span也可以用伪类代替

```javascript

.thumb:after{
    content:'';
    display:inline-block;
    height:100%;
    vertical-align:middle;
}
```
## 第四种：使用div模拟表格结构

```javascript

<div class="table">
    <div class="td">
        <img src="图片链接" alt="">
    </div>
</div>
```

在css设置中 给 `.table` 添加 `display:table`;
           给 `.td` 添加 `display:table-cell; vertical-align:middle;text-align:center`






