# Day4课程内容
Day4的课程`tranistion 动画`和`animation 动画`,课程内容讲的并不是很多，课程作业也预先写好了大部分代码，需要自己好好琢磨才能掌握。
## `tranistion 动画`和`animation 动画`简介
`tranistion 动画`为补间动画，即我们只需要定义一个动画开始和结束的两个状态，浏览器就会计算出动画中间的自动帮我们做好展示。
`animation 动画`是帧动画的意思，意思是我们定义的**不再是开始和结束**两个状态了，而是**根据动画原型来定义**，我们动画原型分为几个阶段，就定义几个**关键帧**来表示状态，每个关键帧之间的动画由浏览器计算得出。
如下图就是两种最简单的动画：

![css1.gif](http://www.xluos.com/usr/uploads/2017/12/2617119284.gif)

![css2.gif](http://www.xluos.com/usr/uploads/2017/12/2224219172.gif)

在我看来`tranistion 动画`是`animation 动画`的一种特殊形式，为了更容易书写或者渲染方便等原因所以单独分出来一个属性，应该说以一种简化的`animation`

这一节用到的一个新属性`transform`，这个属性是`CSS3`中的一个属性功能非常强大，[W3C的定义](http://www.w3school.com.cn/cssref/pr_transform.asp)如下：
> transform 属性向元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。

由于属性太多了，这里只写我们用到一个方法`translate(x,y)`,根据给定的 **left（x 坐标） 和 top（y 坐标）** 位置参数，对元素进行移动。这是一个2D平移操作，**不会使元素脱离文档流**。
x和y的值支持百分比，不过这个百分比是对于自身来说了，参见下图：

![图像 014.png](http://www.xluos.com/usr/uploads/2017/12/3400714682.png)

## tranistion 动画
补间动画或者说**过渡**更加贴切，毕竟官方就是这么称呼的。相对来说比较容易
### 参数
`tranistion`是一个简写属性默认值为`all 0 ease 0`，这四个值分别代表下面四个属性。
+ `transition-property: none|all|property;` 设置应用过渡效果的css属性名称
    |值|描述|
    |:---|:---|
    |none|没有属性会获得过渡效果|
    |all|所有属性都将获得过渡效果|
    |property|定义应用过渡效果的 CSS 属性名称列表，列表以逗号分隔|
+ `transition-duration: time;` 设置过渡完成的时间
    |值|描述|
    |:---|:---|
    |time|规定完成过渡效果需要花费的时间（以秒或毫秒计）。默认值是 0，意味着不会有效果|
+ `transition-timing-function` 设置过渡效果的速度曲线
    + 语法`transition-timing-function: linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier(n,n,n,n);`

    |值|描述|
    |:---|:---|
    |linear|规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）。|
    |ease|规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）。|
    |ease-in|规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）。|
    |ease-out|规定以慢速结束的过渡效果（等于 cubic-bezier(0,0,0.58,1)）。|
    |ease-in-out|规定以慢速开始和结束的过渡效果（等于 cubic-bezier(0.42,0,0.58,1)）。|
    |cubic-bezier(n,n,n,n)|在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值。|
    + `cubic-bezier`函数实际上设置的是运动速率的**贝塞尔曲线**，在这个网站上可以实时设置并立马看到效果[CSS贝塞尔曲线可视化](http://cubic-bezier.com/#.17,.67,.83,.67)
+ `transition-delay: time;` 设置过渡开始的延迟时间
    |值|描述|
    |:---|:---|
    |time|规定在过渡效果开始之前需要等待的时间，以秒或毫秒计。|

### Dome
在线查看:

代码：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
    .window{
        position: relative;
        width: 500px;
        height: 200px;
        background-color: aliceblue;
        border: darkgrey solid 1px;
        margin: 0 auto;
        /* transition: all 1s; */
    }
    .qiu {
        position: absolute;
        top: 50px;
        transform: translate(0,50%);
        height: 50px;
        width: 50px;
        border-radius: 50%;
        background-color: aqua;
        transition: all 1s;
    }
    .window:hover .qiu {
        transform: translate(450px,50%);
        background-color: aquamarine;
    }
    </style>
</head>
<body>
    <div style="text-align: center">鼠标放到方框查看动画</div>
    <div class="window">
        <div class="qiu"></div>
    </div>
</body>
</html>

```
## animation 动画

### 定义动画：`@keyframes 规则`
> 通过 @keyframes 规则，您能够创建动画。
创建动画的原理是，将一套 CSS 样式逐渐变化为另一套样式。
在动画过程中，您能够多次改变这套 CSS 样式。
以百分比来规定改变发生的时间，或者通过关键词 "from" 和 "to"，等价于 0% 和 100%。
0% 是动画的开始时间，100% 动画的结束时间。
为了获得最佳的浏览器支持，您应该始终定义 0% 和 100% 选择器。
注释：请使用动画属性来控制动画的外观，同时将动画与选择器绑定。

### 语法
+ `@keyframes animationname {keyframes-selector {css-styles;}}`
    
    |值|描述|
    |:---|:---|
    |`animationname`|必需。定义动画的名称。|
    |`keyframes-selector`|必需。动画时长的百分比。合法的值：0-100% from（与 0% 相同）to（与 100% 相同）|
    |`css-styles`|必需。一个或多个合法的 CSS 样式属性。|

+ `keyframes-selector`可以有很多个，值可以不按顺序写（在chrome测试），比如先写0%和100%的状态，再写25%和50%的状态，但是在可读性上来说还是按顺序写跟利于维护。
### 给元素加入动画
语法：`animation: name duration timing-function delay iteration-count direction fill-mode play-state;` 

默认值为`none 0 ease 0 1 normal`

看着很乱东西很多，这是简写的六个属性，他们分别是下面这些（部分属性和上面的`transition`相同细节不重复写）

+ `animation-name: keyframename|none;`为 @keyframes 动画规定名称。 设置为`none`可以覆盖级联的动画效果
+ `animation-duration: time;` 设置动画时间，默认为0（无动画）
+ `animation-timing-function: value;` 设置动画速度曲线，参数和前面的一样，默认是`ease(慢快慢)`
+ `animation-delay: time;` 动画开始前的延时
+ `animation-iteration-count: n|infinite;` 动画播放的次数
    |值|描述|
    |:---|:---|
    |n|定义动画播放次数的数值。|
    |infinite|规定动画应该无限次播放。|
+ `animation-direction: normal|alternate;`  定义是否要轮流反向播放动画，见下图
    |值|描述|
    |:---|:---|
    |normal|默认值。动画应该正常播放。|
    |alternate|动画应该轮流反向播放。|

    ![css3.gif](http://www.xluos.com/usr/uploads/2017/12/525904668.gif)
+ `animation-play-state: paused|running ` 规定动画正在运行还是暂停。（PS：您可以在 JavaScript 中使用该属性，这样就能在播放过程中暂停动画。）
+ `animation-fill-mode : none | forwards | backwards | both;` 
>animation-fill-mode 属性规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。默认情况下，CSS 动画在第一个关键帧播放完之前不会影响元素，在最后一个关键帧完成后停止影响元素。animation-fill-mode 属性可重写该行为。

|值|描述|
|:---|:---|
|none|不改变默认行为。|
|forwards|当动画完成后，保持最后一个属性值（在最后一个关键帧中定义），即在动画完成后，保持最后一个状态，不再变回原来的状态|
|backwards|在 `animation-delay` 所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义）。|
|both|`forwards`和`backwards`两种模式都被应用。|
+ `:nth-child(n)` 这是一个在动画中经常用到的伪选择器，中间的n替换为正整数，意思是选中其父元素的第几个子元素，通过这个可以方便的设置各个小组件的延时达到流畅动画的效果。
+ 
### 小玩意
最后在介绍一个函数`steps()`，这个函数是一个`timing-function（速度曲线）`的值，与之前不同的是，前面的都是设置为平滑过渡，而这个函数是一个阶跃函数，可以设置为分段过渡，用这个可以完成很多很有意思的效果。
+ `steps()`有两个参数
+ 第一个参数指定动画分为多少段完成
+ 第二个参数有两个值`start`和`end`，指定在每段的起点还是终点发生变化。默认值为`end` 

可以看这个学习`steps`的用法：https://segmentfault.com/a/1190000007042048

下面是两个很有意思的Demo，时钟Demo来自[这里](https://designmodo.com/demo/stepscss/pawprints.html)，打字Demo来自阮一峰博客

![](http://www.xluos.com/usr/uploads/2017/12/1985179436.png)

![css4.gif](http://www.xluos.com/usr/uploads/2017/12/3351138871.gif)
# Day3直播课内容
如果说直播课上还是懵逼，那么现在对这些动画就很清楚那些属性到底是怎么回事儿了。直播课上要求每个人都写一个自己的,现在就很容易写出来一个简单的loading了，[Demo]()效果图如下：

![css5.gif](http://www.xluos.com/usr/uploads/2017/12/2507239177.gif)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>loading</title>
    <style>
    body {
        background-color: #ddd
    }
    /* 设置大盒子水平垂直居中 */
    .box {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
    }
    /* 设置球球动画 */
    @keyframes qiuqiu {
        0% {
            top:-25px;
        }
        100% {
            top:25px;
        }
    }
    /* 设置球的属性 */
    .qiu {
        position: relative;
        top: -25px;
        float: left;
        margin: 10px;
        height: 40px;
        width: 40px;
        border-radius: 50%;
        background-color: #4285f4;
        animation: qiuqiu .4s ease-in-out infinite alternate;
    }
    /* 分别为后面的小球设置颜色和延时 */
    .qiu:nth-child(2) {
        animation-delay: 0.1s;
        background-color: #0f9d58;
    }
    .qiu:nth-child(3) {
        animation-delay: 0.2s;
        background-color: #f4b400;
    }
    .qiu:nth-child(4) {
        animation-delay: 0.3s;
        background-color: #db4437;
    }

    </style>
</head>
<body>
    <div class="box">
        <div class="qiu"></div>
        <div class="qiu"></div>
        <div class="qiu"></div>
        <div class="qiu"></div>
    </div>
</body>
</html>
```
# Day4课程补充知识（非常多）
+ [软谋教育 CSS动画](https://ke.qq.com/webcourse/index.html#course_id=20945&term_id=100003219&taid=638975169548753&vid=t1410nxa13p)
+ [什么是响应式布局设计？](https://www.zhihu.com/question/20976405)
+ [viewport 深入理解](https://www.cnblogs.com/2050/p/3877280.html)
+ [CSS 媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)
+ [下手响应式及断点设置分析](http://imweb.io/topic/56dff5121a5f05dc506430da)
## 整体布局
+ [浮动布局精华 Layout Gala（参考流体负 margin 布局部分）](https://blog.html.it/layoutgala/index.html)
+ [Flexbox Grid（使用 flexbox 的网格布局系统）](http://flexboxgrid.com/)
+ [How to build a responsive grid system](https://zellwk.com/blog/responsive-grid-system/)
## 内容处理相关
+ [响应式导航解析](http://mux.alimama.com/posts/785)
+ [video 自适应问题](http://www.alistapart.com/articles/creating-intrinsic-ratios-for-video/)
+ [响应式图片处理101系列(共10篇)](http://www.w3cplus.com/blog/tags/509.html)
+ [table 处理](https://css-tricks.com/responsive-data-table-roundup/)
+ [图片滚动](http://www.swiper.com.cn/)
## 框架相关
+ [Bootstrap](http://v3.bootcss.com/)
+ [Foundation](http://www.foundcss.com/)
## 工具相关
+ [使用Chrome测试页面响应性](https://segmentfault.com/a/1190000000581601)
+ [测试响应和设备特定可视窗口](http://www.css88.com/doc/chrome-devtools/device-mode/emulate-mobile-viewports/)
+ [Screensiz.es](http://screensiz.es/phone)
+ [Respond.js](https://github.com/scottjehl/Respond)
## 其他
+ [响应式案例](https://mediaqueri.es/)
+ [响应式资源汇总](https://bradfrost.github.io/this-is-responsive/resources.html)
+ 书籍：[《响应式Web设计-HTML5和CSS3实践》](https://www.gitbook.com/book/jobrest/web-html5-css3/details)（英文名为：Responsive Web Design width HTML5 and CSS3）作者：Ben Frain
+ [w3school html5 学习资料](http://www.w3school.com.cn/html5/)

+ [w3school CSS3 学习资料](http://www.w3school.com.cn/css3/)
# Day4总结
动画这一点儿的东西挺多的，只是靠上课那点儿东西自己不自己查的话，作为新手是很难搞清楚的，在一个就是如果之前用过其他软件（比如AE）做过小动画的话，这一点儿理解起来就很容易了，chrome的调试工具中还可以直接拉贝塞尔曲线实时看到效果很赞。
# Day4干货分享
2017前端框架及进阶知识汇总
 http://www.jianshu.com/p/302f14643c69 

10个优美的程序猿把玩儿的实用科技设计酷站
 http://www.jianshu.com/p/37a5998f6a20 

