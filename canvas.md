# Canvas第一天笔记

## 1.canvas元素
> <canvas id="tutorial" width="150" height="150"></canvas>

canvas是html5新增的一个标签，与其他元素不同的是canvas元素没有src和alt属性，它只有width和height属性，当没有设置这两个属性时，width的默认值是300px，height的默认值是150px.

***

## 2.canvas的渲染上下文
在这里主要学习的是canvas的2D绘制方法，主要使用的是这个接口[CanvasRenderingContext2D](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D)，这个接口包含着绘制2d图像的各种方法.

***

## 3.绘制图形
### 1.绘制矩形
canvas提供了三种绘制矩形的方法，这也是canvas唯一支持的原生绘制的图形  
`fillRect(x, y, width, height)`  
绘制一个填充的矩形    

`strokeRect(x, y, width, height)`  
绘制一个矩形的边框  
  
`clearRect(x, y, width, height)`
清除指定矩形区域，让清除部分完全透明

***

### 2.绘制路径
图形的基本元素是路径，在canvas中如果要画出一个复杂的图形，那么这个图形的每一条线，每一条弧等等都是一条路径，这个图形就是一个路径的集合

canvas中绘制路径的步骤
1. 创建路径起始点
2. 用画图命令画出路径
3. 封闭路径
4. 对路径填充或者描边  

`beginPath()`  
新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径  

`closePath()`    
闭合路径之后图形绘制命令又重新指向到上下文中  

`stroke()`   
通过线条来绘制图形轮廓  

`fill()`    
通过填充路径的内容区域生成实心的图形  

**注意**
当前路径为空，即调用`beginPath()`之后，或者canvas刚建的时候，第一条路径构造命令通常被视为是`moveTo()`，无论实际上是什么。出于这个原因，你几乎总是要在设置路径之后专门指定你的起始位置。

**注意**
当你调用`fill()`函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用`closePath()`函数。但是调用`stroke()`时不会自动闭合。

***

### 3.移动笔触
`moveTo(x, y)`  
将笔触移动到指定的坐标x以及y上

***

### 4.线
`lineTo(x, y)`  
绘制一条从当前位置到指定x以及y位置的直线

***

### 5.圆弧
`arc(x, y, radius, startAngle, endAngle, anticlockwise)`  
画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成

**注意**
anticlockwise为一个布尔值。为true时，是逆时针方向，为false时，顺时针方向。
**注意** 
arc()函数中的角度单位是弧度，不是度数。角度与弧度的js表达式:**radians=(Math.PI/180)*degrees**

`arcTo(x1, y1, x2, y2, radius)`  
根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。

***

### 6.二次贝塞尔曲线及三次贝塞尔曲线
`quadraticCurveTo(cp1x, cp1y, x, y)`  
绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点

`bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`  
绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点

***

### 7.矩形
`rect(x, y, width, height)`  
绘制一个左上角坐标为（x,y），宽高为width以及height的矩形

**注意**
该方法是把一个矩形添加到一个路径上，和上面的绘制矩形的方法是不同的

**注意** 
当该方法执行的时候，moveTo()方法自动设置坐标参数（0,0）。也就是说，当前笔触自动重置回默认坐标

***

## 4.Path2D 对象
`Path2D()`  
Path2D()会返回一个新初始化的Path2D对象

`Path2D.addPath(path [, transform])​`  
添加了一条路径到当前路径（可能添加了一个变换矩阵）


















