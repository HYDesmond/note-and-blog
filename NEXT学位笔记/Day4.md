# Day4课程内容
Day4的课程`tranistion 动画`和`animation 动画`,课程内容讲的并不详细，课程作业也预先写好了大部分代码，需要自己好好琢磨才能掌握。
## `tranistion 动画`和`animation 动画`简介
`tranistion 动画`为补间动画，即我们只需要定义一个动画开始和结束的两个状态，浏览器就会计算出动画中间的自动帮我们做好展示。
`animation 动画`是帧动画的意思，意思是我们定义的**不再是开始和结束**两个状态了，而是**根据动画原型来定义**，我们动画原型分为几个阶段，就定义几个**关键帧**来表示状态，每个关键帧之间的动画由浏览器计算得出。
如下图就是两种最简单的动画：

![css1.gif](http://www.xluos.com/usr/uploads/2017/12/2617119284.gif)

![css2.gif](http://www.xluos.com/usr/uploads/2017/12/2224219172.gif)

在我看来`tranistion 动画`是`animation 动画`的一种特殊形式，为了更容易书写或者渲染方便等原因所以单独分出来一个属性

这一节用到的一个新属性`transform`，这个属性是`CSS3`中的一个属性功能非常强大，[W3C的定义](http://www.w3school.com.cn/cssref/pr_transform.asp)如下：
> transform 属性向元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。

由于属性太多了，这里只写我们用到一个方法`translate(x,y)`,根据给定的 **left（x 坐标） 和 top（y 坐标）** 位置参数，对元素进行移动。这是一个2D平移操作，**不会使元素脱离文档流**。
x和y的值支持百分比，不过这个百分比是对于自身来说了，参见下图：

![图像 014.png](http://www.xluos.com/usr/uploads/2017/12/3400714682.png)

## tranistion 动画
### 参数

### Dome

## animation 动画
### 参数

### Dome

# Day3直播课内容

# Day课程补充知识（非常多）
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

# Day4干货分享
2017前端框架及进阶知识汇总
 http://www.jianshu.com/p/302f14643c69 

10个优美的程序猿把玩儿的实用科技设计酷站
 http://www.jianshu.com/p/37a5998f6a20 

