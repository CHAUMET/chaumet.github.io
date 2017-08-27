---
title: hexo图片问题
date: 2016-12-21 20:03:11
tags: image
---

按照叙事文的套路来讲，这个过程要分为：起因、经过、结果三个部分。

### 起因

MarkDown里插入图片用的好好的，在hexo博客首页看的好好的，单独在列表里点进去就消失了（歪头摊手）

这让人很不开心啊

然后，搜了一圈插入本地图片的方法，有人提到了这个问题

>注意:
 通过常规的 markdown 语法和相对路径来引用图片和其它资源可能会导致它们在存档页或者主页上显示不正确。在Hexo2时代，社区创建了很多插件来解决这个问题。但是，随着Hexo3的发布，许多新的标签插件被加入到了核心代码中。这使得你可以更简单地在文章中引用你的资源。

 于是就决定试试作者推荐的解决方案:使用CodeFalling/hexo-asset-image的插件来加载本地图片

 详细的过程及解释戳[原文链接](http://www.jianshu.com/p/c2ba9533088a)

 ### 经过

 1. 首先确认 `_config.yml` 中是否有 `post_asset_folder:true`
 2. 安装插件：在hexo的目录下执行 `npm install https://github.com/CodeFalling/hexo-asset-image --save`
 3. 完成安装后用hexo新建文章的时候会发现_posts目录下面会多出一个和文章名字一样的文件夹，图片就放在新建的这个文件夹中
 4. 路径就按照md的格式写相对路径就OK了！


### 结果

 经测试显示正常哈哈哈。


### 附赠bug一枚

在折腾完一遍全部的图片链接之后，hexo d -g 突然就报了一堆错

```gitbash
Fatal: TaskCanceledException encountered.
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': Invalid argument
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: Fatal: TaskCanceledException encountered.
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': Invalid argument

    at ChildProcess.<anonymous> (G:\blog\node_modules\hexo-util\lib\spawn.js:37:17)
    at emitTwo (events.js:106:13)
    at ChildProcess.emit (events.js:191:7)
    at ChildProcess.cp.emit (G:\blog\node_modules\cross-spawn\lib\enoent.js:40:29)
    at maybeClose (internal/child_process.js:877:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:226:5)
FATAL Fatal: TaskCanceledException encountered.
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': Invalid argument

Error: Fatal: TaskCanceledException encountered.
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': Invalid argument

    at ChildProcess.<anonymous> (G:\blog\node_modules\hexo-util\lib\spawn.js:37:17)
    at emitTwo (events.js:106:13)
    at ChildProcess.emit (events.js:191:7)
    at ChildProcess.cp.emit (G:\blog\node_modules\cross-spawn\lib\enoent.js:40:29)
    at maybeClose (internal/child_process.js:877:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:226:5)
```

之后又搜了一圈 **把repo地址从http改成ssh就可以了**

因缺思厅（手动doge脸）

