## css盒模型和BFC

##### 盒模型

CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：外边距（margin）、边框（border）、内边距（padding）、实际内容（content）四个属性。

标准盒模型：width/height=content
IE盒子模型：width/height= content + padding + border

标准：box-sizing: content-box; ( 浏览器默认设置 )
IE： box-sizing: border-box;

##### BFC

> 一个块格式化上下文（block formatting context）是Web页面的可视化CSS渲染的一部分。它是块盒子的布局发生，浮动互相交互的区域。

具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。

触发 BFC：

- body 根元素
- 浮动元素：float 除 none 以外的值
- 绝对定位元素：position (absolute、fixed)
- display 为 inline-block、table-cells、flex
- overflow 除了 visible 以外的值 (hidden、auto、scroll)

边距重叠解决方案：

- 建立了新的BFC的盒不和后代元素发生margin collapse（外边距折叠，外边距合并）
- 建立新的BFC的盒不会和其所处的同一个块格式化上下文中的浮动元素重叠。
- 建立新的BFC的盒height:auto时，会被内部的浮动元素撑开（clearfix）

BFC 的用途：

- 清楚浮动
- 解决外边距合并
- 布局







