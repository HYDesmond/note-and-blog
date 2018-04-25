+ 只有width和height属性，也可以用CSS设置大小
+ 默认尺寸300*150
+ `<canvas></canvas>` 标签内部的内容是替换内容，在浏览器支持canvas时不渲染内部的内容，不支持是渲染内部的内容展示出来
+ `<canvas>` 元素有一个叫做 getContext() 的方法，这个方法是用来获得渲染上下文和它的绘画功能。getContext()只有一个参数，上下文的格式。
```js
    var canvas = document.getElementById('tutorial');
    var ctx = canvas.getContext('2d');
```
+ canvas内部的像素是他的坐标体系和css设置的像素不同。左上角为(0,0)
+ canvas设置的宽高真正的宽高，css设置canvas的宽高会拉伸canvas显示的图像，类比拉伸图片
# 绘制矩形
坐标相对于矩形的左上角。x，y绘制起点坐标   width, height矩形宽高
+ 绘制实心矩形`void ctx.fillRect(x, y, width, height);`
+ 绘制空心矩形`void ctx.strokeRect(x, y, width, height);`
+ 擦除矩形位置`void ctx.clearRect(x, y, width, height);`
# 绘制文字
坐标默认相对于文字的左边和底边    text 绘制文本  绘制坐标  maxWidth 最大宽度
+ 绘制实心文字`void ctx.fillText(text, x, y [, maxWidth]);`
+ 绘制空心文字`void ctx.strokeText(text, x, y [, maxWidth]);`
+ 测量文本`ctx.measureText(text);`，返回TextMetrics 对象。具有width属性

# 绘制线型
+ 线的宽度`ctx.lineWidth = value;` 只接受正值 其他的会被忽略
+ 线末端类型`ctx.lineCap = "butt";`  butt方的无加长，round加上一节半圆 square加上一节方形，类似于将round的半圆变成方形
+ 拐点形状`ctx.lineJoin` miter尖的 round圆角  bevel把尖磨平
+ 限制miter尖角的长度`ctx.miterLimit` 默认为10，接收正值
+ 获取当前线段样式`ctx.getLineDash();`是交替描述线段和间距的数组，支线返回空
+ 设置线段样式`ctx.setLineDash()`
+ 设置虚线的偏移量`ctx.lineDashOffset = value;`接收浮点数

# 文本样式
+ 文字样式`ctx.font = value;`CSS font语法的字符串
+ 对齐样式`ctx.textAlign = "left" || "right" || "center" || "start" || "end";` 相对于绘制文字的坐标

# 填充和描边样式
+ 设置内部填充的颜色`ctx.fillStyle`，设置边框颜色`ctx.strokeStyle`他们的值都是
    + color DOMString 字符串，被转换成 CSS <color> 颜色值.
    + gradient CanvasGradient 对象 （线性渐变或者放射性渐变）.
    + pattern CanvasPattern 对象 （可重复图像）。
# 渐变和图案
+ 创建一个线性渐变`CanvasGradient ctx.createLinearGradient(x0, y0, x1, y1);`
    + 指定线路的线性渐变，起点(x0,y0)终点(x1,y1)
    + 使用`addColorStop(offset, color)`方法添加颜色断点，offset值在0-1之间
+ 创建一个径向渐变`CanvasGradient ctx.createRadialGradient(x0, y0, r0, x1, y1, r1);`
    + 参数分别是初始圆形圆心的坐标和半径，结束圆形圆心的坐标和半径
    + `addColorStop(offset, color)`中0是初始圆形的位置，1是结束圆形的位置
+ 创建图案`CanvasPattern ctx.createPattern(image, repetition);`[API](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/createPattern)

# 阴影
+ 模糊程度`ctx.shadowBlur = level;`level是一个float值
+ 阴影颜色`ctx.shadowColor = color;`
+ 偏移量`ctx.shadowOffsetY`和`ctx.shadowOffsetX`都是float值

# 路径
+ `void ctx.beginPath();`开始一条新路径
+ `void ctx.closePath();`尝试绘制闭合路径
+ `void ctx.moveTo(x, y);`移动画笔（抬笔移动）
+ `void ctx.lineTo(x, y);`绘制路径（下笔移动）
+ `void ctx.arc(x, y, radius, startAngle, endAngle, anticlockwise);`画圆
    + x,y,radius 圆心，半径
    + startAngle起点弧度，endAngle终点弧度  相对于x轴
    + anticlockwise 绘制方式可选，true逆时针
+ `void ctx.arcTo(x1, y1, x2, y2, radius);`画圆弧
    + 根据当前描点与给定的控制点1连接的直线，和控制点1与控制点2连接的直线，作为使用指定半径的圆的切线，画出两条切线之间的弧线路径。
+ `void ctx.rect(x, y, width, height);`画矩形路径，参数:起点坐标和宽高

# 绘制路径
+ `void ctx.fill();`填充路径
+ `void ctx.stroke();`描边路径
+ `void ctx.clip();`将当前路径设置为剪切路径（类似剪切蒙版）
+ `boolean ctx.isPointInStroke(x, y);`判断点是否在边线上
+ `boolean ctx.isPointInPath(x, y);`判断点是否在路径内

# canvas 状态
+ `void ctx.save();`当前状态入栈
+ `void ctx.restore();`当前状态出栈恢复