---
title: css图片间距过宽问题
date: 2016-12-15 19:13:42
tags: css
---

今天在做网页的动态效果时，遇见一个图片间距过宽问题。

![img](img/spaceImg_01.png)

好好一个盾牌变成分裂的地砖了....


之后明白是因为代码中的换行字符硬生生把这个缝撑开了

```javascript

 <div class="shield">
            <img src="./images/shield_1.png" alt=""/>
            <img src="./images/shield_2.png" alt=""/>
            <img src="./images/shield_3.png" alt=""/>
            <img src="./images/shield_4.png" alt=""/>
            <img src="./images/shield_5.png" alt=""/>
            <img src="./images/shield_6.png" alt=""/>
            <img src="./images/shield_7.png" alt=""/>
            <img src="./images/shield_8.png" alt=""/>
            <img src="./images/shield_9.png" alt=""/>
        </div>

```

搞清楚了为什么，之后就是怎么解决它。

一般来讲，有三种方法

### 将元素改为块元素

display：block

### 将元素浮动

float：left

### 将font-size设为0

这个具体为啥我也没整明白，只是看到了就记下来。事实证明，他也是好用的。

不过css嘛` 总是有一些逻辑上解释不出来但是确实实际可行的方法。

以上三种方法实测都是可以的，只是前两种需要在css文件中单独对图片进行设置，而第三种方法只需要在父元素上增加一行代码就好了。

最后，地砖变成盾牌了~

![img](img/spaceImg_02.png)