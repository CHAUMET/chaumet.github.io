---
title: Emmet常见语法
date: 2016-12-19 20:29:02
tags: Emmet
---

Emmet (前身为 Zen Coding) 是一个能大幅度提高前端开发效率的一个工具:

>基本上，大多数的文本编辑器都会允许你存储和重用一些代码块，我们称之为“片段”。虽然片段能很好地推动你得生产力，但大多数的实现都有这样一个缺点：你必须先定义你得代码片段，并且不能再运行时进行拓展。

Emmet把片段这个概念提高到了一个新的层次：你可以设置CSS形式的能够动态被解析的表达式，然后根据你所输入的缩写来得到相应的内容。Emmet是很成熟的并且非常适用于编写HTML/XML 和 CSS 代码的前端开发人员，但也可以用于编程语言。

在使用Emmet语法之前需要知道一个前提原则：Emmet语法在不同的使用区域，它的功能是不同的。

比如说，在css中书写 `w100` 按下tab键之后，会自动补全变成 `width:100px;` 但，这条命令如果在html中，是不会自动补全的

#### 自动联想

Emmet会在你输入字母之后，自动联想出相应的单词作为选项来提供，这时需要我们主观的去选择

## 常见基础语法

### ` . `  类名  ` # ` id
```javascript
- div.box => <div class="box"></div>
- div#box => <div id="box"></div>
- div.box#box => <div class="box" id="box"></div>
```
### ` [] ` 属性
```javascript
- div[class="a"] => <div class="box"></div>
- img[width="100"] => <img src="" alt="" width="100">
```
### ` > ` 子元素
```javascript
- ul>li =>  <ul>
                <li></li>
            </ul>
```
### ` + ` 同级元素
```javascript
- div+p => <div></div>
           <p></p>
```
### ` {} ` 内容
```javascript
- p{你好} => <p>你好</p>
```
### ` * ` 创建多个同一类型标签
```javascript
- p*2 => <p></p>
         <p></p>
```
### ` $ ` 索引值
```javascript
- p{$}*2 =><p>1</p>
           <p>2</p>
```

[更多Emmet语法查询](http://www.w3cplus.com/tools/emmet-cheat-sheet.html)


-