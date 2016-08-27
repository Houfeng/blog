---
layout: post
title: 浏览器页面渲染之“reflow”
category: 前端
published: true
date: 2012-12-27
---


### 基本概念
浏览器为了重新渲染部分或整个页面，重新计算页面元素位置和几何结构（geometries）的进程叫做 reflow（回流）。有时 reflow 页面中的一个元素会 reflow 它的祖先元素以及所有子元素。
由于 reflow 的开销非常之大，因此要尽可能的避免 reflow 的发生。

<!--more-->

### 产生 reflow 的原因
1. 调整窗口大小；
2. 改变字体；
3. 增加或者移除样式表；
4. 内容变化，比如用户在 input 框中输入文字；
5. 激活 CSS 伪类，比如 :hover (IE 中为兄弟结点伪类的激活)；
6. 操作 class 属性；
7. 脚本操作 DOM；
8. 计算 offsetWidth 和 offsetHeight 属性；
9. 设置 style 属性的值。

### 如何减少 reflow
1. 如果想设定元素的样式，通过改变元素的 class 名 (尽可能在 DOM 树的最里层)；
2. 避免设置多项内联样式；
3. 用于表现动画的元素，使用 position 属性的 fixed 值或 absolute 值（脱离文档流）；
4. 权衡平滑和速度（调大每帧间隔，减少 reflow 次数）；
5. 避免使用 table 布局；
6. 避免使用CSS的 JavaScript 表达式 (仅 IE 浏览器)；
7. 减少不必要的 DOM 层级，改变 DOM 树中的一级会导致所有层级的改变，上至根部，下至被改变节点的子节点；