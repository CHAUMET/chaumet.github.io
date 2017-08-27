---
title: ES6新功能
date: 2017-01-16 16:30:25
tags: ECMAscript6
---

## 块级作用域 Let

### 用 `let` 声明一个变量，那么该变量只能使用在定义它的块里面。

使用场景：for循环的计数器

在下面代码中如果使用var，最后输出的是10

```javascript

var a=[];
for(var i=0;i<10;i++){
    a[i]=function(){
        console.log(i);
    };
}

a[6](); //10

```

如果使用let，最后输出6

### 不存在变量提升

在代码块内，使用let命令声明变量之前，该变量都是不可用的。
这在语法上称为“暂时性死区”（temporal dead zone，简称TDZ）

### 不允许重复声明

let 不允许在相同作用域内，重复声明同一个变量，也不允许在函数内部重新声明参数。

### 使匿名函数不再必要

```javascript
//匿名函数
（function(){
    var temp = 123;
    ...
}()）;

// 块级作用域

{
    let temp = 123;
    ...
}

```

## 衡量 const

`const`也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变。

对常量重新赋值不会报错，只会默默失败

const的作用域与let命令相同：只在声明所在的块级作用域内有效。

const命令也不存在提升，只能在生命的位置后面使用。

同样的，const声明的变量不可重复声明。

### 将对象声明为常量

由于const命令只是指向变量所在的地址，所以将一个对象声明为常量必须非常小心

```javascript

const foo = {};
foo.prop = 123;

foo.prop // 123

foo = {} //不起作用

```

上面代码中，常量foo存储的是一个地址，这个地址指向一个对象，不可变得只是这个地址，
即不能把foo指向另外一个地址，但对象本身是可变的，所以依然可以为其添加新属性；

### 跨模块常量

因为const声明的常量只在当前代码块有效，如果想设置跨模块常量，可在const前添加 `export`;

### 全局对象的属性

全局对象是最顶层的对象，在浏览器环境指的是Windows对象，在Node.js指的是global对象。
在JavaScript语言中，所有全局变量都是全局对象的属性。

ES6规定，var命令和function命令生命的全局变量，属于全局对象的属性；
let命令、const命令。class命令生命的全局变量，不属于全局对象的属性。

## 数组的解构赋值

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为结构。

以前为变量赋值，只能直接指定值。ES6允许写成这样：

```javascript

let[a,b,c] = [1,2,3];

//从数组中提取值，按照对应位置，对变量赋值。

```

这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。

结构只能用于数组或对象，其他原始类型的值都可以转为相应的对象。
所以等号右边如果是原始类型的值，结构不会成功，变量的值等于undefined；
但undefined和null不能转为对象，因此包装错；
另一种情况下，等号左边只匹配一部分等号右边的数组，此时解构依然可以成功。

```javascript

// 等号右边为原始类型的值

var [foo] = [];
var [foo] = 1;
var [foo] = false;
var [foo] = NaN;
var [bar,foo] = [1];

//等号左边不完全匹配右边

lett[x,y] = [1,2,3];
x//1
y//2

```

解构赋值允许指定默认值

```javascript

var [foo = true] = [];
foo//true

[x,y='b'] = ['a'] // x='a',y='b'
[x,y='b'] = ['a',undefined] // x='a',y='b'

// ES6内部使用严格相等运算符（===），判断一个位置是否有值。所以如果一个数组成员不严格等于undefined，默认值是不会生效的。

var [x=1] = [undefined];
x//1

var [x=1] = [null];
x//null

```


## 对象的解构赋值

结构也可以用于对象。与数组不同的是，对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
```javascript

var {bar,foo} = {foo:'aaa',bar:'bbb'};
foo//'aaa'
bar//'bbb'

var {baz} = {foo:'aaa',bar:'bbb'};
baz//undefined
//没有对应的同名属性，导致取不到值，最后等于undefined。
```
如果变量名与属性名不一致，必须写成下面这样

```javascript

var {foo:baz} = {foo:'aaa',bar；'bbb'}
baz//'aaa'

```
和数组一样，解构也可以用于嵌套解构的对象，对象的解构也可以指定默认值。
默认值生效的条件是，对象的属性值严格等于undefined。

## 字符串的解构赋值

字符串被结构赋值时，被转换成了一个类似数组的对象。

```javascript

const[a,b,c,d,e] = 'hello';
a//'h'
b//'e'
c//'l'
d//'l'
e//'o'

```
类似数组的对象，都有一个length属性，因此还可以对这个属性解构赋值。

``javascript
 let{length:len} = 'hello'
 len//5

## 圆括号问题

因为圆括号有可能导致解构的歧义，所以尽量不要在模式中放置圆括号

###不能使用圆括号的情况

+ 变量声明语句中，模式不能带有圆括号

```javascript

//全部报错

var[(a)] = [1];
var{x:(c)} = {};
var{o:({p:p})} = {o:{p:2}} ;

```

+ 函数参数中，模式不能带有圆括号

```javascript

//报错
function f([(z)]){return z;}

```
+ 不能将整个模式。或嵌套模式中的一层，放在圆括号之中

```javascript
//全部报错

（{p:a}） = {p:42};
([a]) = [5];

[({p:a}),{x；c}] = [{},{}];

```

## 模板字符串

常规字符串的链接是用加号，ES6提供了 `${}` 模板实现更方便的语句书写

```javascript

let desert = 'cake',
    drink = 'tea';

//常规写法
let breakfirst = '今天的早餐是'+desert+'和'+dirnk+'。'；

//ES6模板字符串写法
let breakfirst = `今天的早餐是${desert}和${dirnk}。`；

```

注：用${}包裹完变量之后，要用反引号（`）包裹住整个句子；


