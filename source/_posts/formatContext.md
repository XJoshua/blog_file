---
title: formatContext
date: 2017-10-26 10:35:24
tags: [css, BFC, IFC]
---
[原文地址](http://layout.imweb.io/article/formatting-context.html)
## 格式化上下文

默认情况下，网页上的所有内容都是放在一个个矩形盒子里按照元素在HTML中的先后顺序从左至右自上而下一个接一个排列摆放的。如下图：
![page](http://coding.imweb.io/img/p3/vfm/vfm.png)
在图中可以看到，有些元素的盒子被渲染为完整的一行，如h1、p、div；而有一些元素的盒子则被渲染为水平排列，直到该行被占满然后换行，如span、a、strong

这主要是因为不同的盒子按照不同的格式化上下文来布局，每个格式化上下文都拥有一套不同的渲染规则，他决定了其子元素如何定位，以及和其他元素的关系和相互作用。就如同使用不同的容器来装水，容器不同，呈现出来水的形态也不一样：
![water](http://layout.imweb.io/img/2017-09-26-17-41-31.png)
最基本的两个格式化上下文就是：格式化上下文（BFC），行内格式化上下文（IFC）

<!--more-->

## BFC(之前有篇文章单独记录了，可以在目录中详细查看)

默认根元素(html元素)会创建一个BFC，除此之外，下面的任何一个条件也都可以创建一个新的BFC：

* 浮动元素（元素的float不是none）
* 绝对定位元素（元素具有position为absolute或fixed）
* 内联块（元素具有display: inline-block）
* 表格单元格（元素具有display: table-cell,HTML表格单元格默认属性）
* 表格标题（元素具有display: table-caption,HTML表格标题默认属性）
* 具有overflow且值不是visible的块元素
* display: flow-root
* column-span：all应当总是会创建一个新的格式化上下文，即便是具有column-span：all的元素并不被包裹在一个多列容器中
* flex item和grid item

BFC规定的是其块级子元素的排列方式，而不是创建BFC的元素本身。如下图：
![page](http://layout.imweb.io/img/2017-09-26-18-04-25.png)

在一个BFC中，其块级盒子元素按照如下规则进行渲染：

* 块级盒会在垂直方向，一个接一个地放置，每个盒子水平占满整个容器空间
* 块级盒的垂直方向距离由上下margin决定，同属于一个BFC中的两个或以上块级盒的相接margin会发生重叠
* BFC就是页面上的一个隔离的独立容器，容器里面的元素不会影响到外面的元素。反之亦然
* 计算BFC的高度时，浮动元素也参与计算

具体渲染效果可以参考Demo：[块格式化上下文](http://coding.imweb.io/demo/p3/vfm/bfc.html)

## IFC

当块容器盒不包括任何块级盒时，就会创建一个行内格式化上下文（IFC）,IFC规定的是行内级子元素的排列方式，其渲染规则比较多，简单罗列几点：

* 盒子一个接一个地在水平方向摆放，当容器宽度不够时就会换行；
* 每一行将会生成一个匿名行盒（line box），包括该行的所有行内级盒；
* 水平方向上，当所有盒的总宽度小于匿名行盒的宽度时，那么水平方向排版由text-align属性来决定；
* 垂直方向上，行内级盒的对齐方式由vertical-align控制，默认对齐方式为baseline；
* 行盒的高度由内部子元素中实际高度最高的盒子计算出来，值得注意的是，行内盒（inline boxes）的垂直的border，padding与margin都不会撑开行盒的高度；

注：在IFC的环境中，是不能存在块级元素的，如果将块级元素插入到IFC中，那么此IFC将会被破坏掉变成BFC，而块级元素前的元素或文本和块级元素后的元素或文本将会各自自动产生一个匿名块盒将其包围；
具体行盒高度及垂直对齐方式渲染效果可参看：
* [行内级元素垂直对齐方式](http://coding.imweb.io/demo/p3/vfm/ifc-vertical-align.html)
* [行盒高度](http://coding.imweb.io/demo/p3/vfm/ifc-height.html)

## 其他格式化上下文

可参见原文地址


