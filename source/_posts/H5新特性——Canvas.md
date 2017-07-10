---
title: Canvas绘图技术
date: 2016-07-12 04:11:29
tags: 
	- HTML5
	- Canvas
---
## HTML5新特性——Canvas绘图技术
提示：Canvas绘图的难点在方法和属性的记忆上！
Canvas技术用于在网页上实现绘图，主要应用领域：
> + 绘制各种统计图表，柱状图、饼图、曲线图、分布图...
> + 动画和游戏

  使用Canvas的方法：
	
	<canvas width="500"  height="400">
		您的浏览器不支持Canvas标签！
	</canvas>
<!-- more -->
提示：
1.  Canvas的宽和高只能用属性方式声明！若使用样式来声明则无效。
2.  Canvas的本意是“画布/画板”，可以盛放绘制的内容；真正执行绘图任务的是“画笔/绘图上下文对象”——Context
3.  每一个画布，有且只有一个画笔对象：
	<pre>
	var ctx = canvas.getContext( '2d' );
	</pre>
4.  画笔/绘图上下文对象的成员：
<pre>
	1.fillStyle:"#000000"			填充颜色
	2.font:"10px sans-serif"			字体
	3.globalAlpha:1					全局透明度
	4.lineCap:"butt"					线的端点样式
	5.lineJoin:"miter"				线的连接处样式
	6.lineWidth:1					线条的宽度
	7.shadowBlur:0					阴影部分模糊距离
	8.shadowColor:"rgba(0, 0, 0, 0)"	阴影部分颜色
	9.shadowOffsetX:0				阴影水平偏移量
	10.shadowOffsetY:0				阴影竖直偏移量
	11.strokeStyle:"#000000"			轮廓/描边颜色
	12.arc:arc()					绘制一个弧线
	13.beginPath:beginPath()		开始绘制路径
	14.clearRect:clearRect()		清除一个矩形范围
	15.clip:clip()				裁切
	16.closePath:closePath()		闭合一条路径
	17.createLinearGradient:createLinearGradient() 创建一个线性渐变色
	18.createRadialGradient:createRadialGradient() 创建一个径向渐变色
	19.drawImage:drawImage()		绘制一幅图像
	20.ellipse:ellipse()			绘制一个椭圆
	21.fill:fill()				填充一条路径
	22.fillRect:fillRect()		填充一个矩形区域
	23.fillText:fillText()		填充一段文本
	24.lineTo:lineTo()			绘制一条直线
	25.measureText:measureText()	测量一段文本，得到宽度
	26.moveTo:moveTo()			移动画笔到指定点
	27.rect:rect()				绘制一个矩形路径
	28.rotate:rotate()			旋转
	29.scale:scale()				缩放
	30.stroke:stroke()			对一条路径描边
	31.strokeRect:strokeRect()	对一个矩形进行描边
	32.strokeText:strokeText()	对一段文本进行描边
	33.translate:translate()		进行位移
</pre>
5.使用画笔在画布上绘制图形<br>
5.1  绘制矩形(长方形)——矩形以自己的左上角作定位点

	<pre>ctx.lineWidth = 10;				修改描边的宽度
	ctx.strokeStyle = '颜色'/渐变色;	修改描边的颜色
	ctx.strokeRect(x, y, w, h)			描边一个矩形
	ctx.fillStyle = '颜色'/渐变色对象;	填充样式
	ctx.fillRect(x, y, w, h)			填充一个矩形
	ctx.clearRect(x,y, w, h);			清除一个矩形范围内所有内容
</pre>
5.2  绘制文本——文字以自己的坐下角作定位点

	<pre>ctx.font = '20px SimHei';		设置字体大小和样式
	var w = ctx.measureText(txt).width;	获取一段文本的宽度
	ctx.strokeStyle = "颜色"/渐变;
	ctx.strokeText(txt, x, y)			对文字进行描边
	ctx.fillStyle = "颜色"/渐变;
	ctx.fillText(txt, x, y)				对文字进行填充
	</pre>
5.3  绘制路径

		ctx.beginPath( );		//开始一条新路径
	
	...在路径上添加定位点...

		ctx.closePath( );		//闭合当前路径 
	路径的两个用途：

		(1)ctx.stroke();	//描边刚刚绘制的路径
		(2)ctx.fill();		//填充刚刚绘制的路径
	常见的添加定位点的方法：

		ctx.moveTo(x,y);	//把画笔移动到指定点
		ctx.lineTo(x,y);		//从上一个点开始到当前点绘制一条直线
		ctx.arc(x,y,r,sAngle,eAngle);		//绘制一条圆弧线
		ctx.ellipse(x,y,rx,ry,sAngle,eAngle);	//绘制一条椭圆弧线
5.4  绘制图像——以图像的左上角为定位点

		ctx.rotate( angle );	//旋转画笔，此后绘制的图像/图像都会旋转——旋转围绕着画布的坐标原点为轴
		ctx.translate(x, y);	//平移坐标轴的原点
		ctx.drawImage(img, x, y);		//原宽原高的绘制图像
		ctx.drawImage(img, x, y, width, height);  //使用指定的宽和高绘制图像
		
	提示：
		1)创建图片： var img =new Image();  img.src="xx.jpg";
		2)使用图片必须等待加载完成！  img.onload = function(){ ... }		
		
		
		
		
		
		
		
		
		
		