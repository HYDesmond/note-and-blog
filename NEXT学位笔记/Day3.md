
# Day3课程内容
第三天是基本的css内容，同样之前有基础很快就过了。

+ css语法
    + css的语法如下图：多条声明语句之间由分号隔开
    ![css语法](http://www.xluos.com/usr/uploads/2017/12/1892450823.png)
+ 如何引入css
    + 内联式直接写在标签里面如果有多条样式代码用分号分开，如：`<p style="color:red;font-size:12px">这里文字是红色。</p>`
    + 嵌入式css样式必须写在`<style type="text/css"></style>`之间，type属性样式表的 MIME 类型。目前，唯一可能的值是 "text/css"。通常`<style></style>`标签写在`<head></head>`之间
    + 外部式，把css样式写在一个单独的以.css结尾的文件中，使用`<link>`标签链接到HTML文件内(该标签一般也放在`<head></head>`之间)，用法：`<link rel="stylesheet" href="base.css">`,*rel="stylesheet" 是固定写法不可修改。*
    + 在相同权值下，样式的选择分别为：内联>嵌入>外部，就近原则
+ css选择器
    + **元素选择器：** 基本的选择器，可以选中html中的元素比如`<p></p>`,用法比如：`p {color:red;}`
    + **类选择器：** 类样式的选择器可以自定义但是要以`.`开头，使用是在标签中加入属性class=“类名（不带点儿）”
    + **ID选择器：** 类似类选择器，只不过为标签设置id="ID名称"，而不是class="类名称"。ID选择符的前面是井号（#）号，而不是英文圆点（.）。*两者区别，类选择器可以使用多次，id选择器只能使用一次方便使用js通过id来获取属性调用*
    + **子选择器（.first>span{color:red;}）与后代选择器(.first  span{color:red;})：** 子选择器作用于元素的第一个后代，后代选择器则是全部后代  
    + **通用选择器（* {。。。}）：** 为所有标签加上样式
    + **伪类选择符（标签：状态{样式}）：** 可以为一个不存在的标签（比如某个标签的鼠标划过状态）设置样式
    + **分组选择符:** 为多个标签设置同一个样式时用`，`分割，如：`h1,span{color:red;}`
+ css盒模型
    + 每一个元素都被当作是一个盒子，从内到外分别是**内容区域**也就是平时设定`width`和`height`区域、**内填充（padding）**、**边框（border）**、**外边距（margin）**。这四个区域加起来的大小才是元素页面真正占用的区域。

        ![css盒子](http://www.xluos.com/usr/uploads/2017/12/3484864675.png)
    + 对于**行内元素**高度的设置是无效的，`margin`的上下边距设置也是无效的
    + 浏览器一般都有默认的`margin`等样式，所以在自己设置样式时一般需要清除默认样式
+  `float`属性可以让块级元素摆脱独占一行的性质，像普通的文档流一样让块元素浮动到同一行，有`left`和`rigth`两个属性分别是向左浮动和向右浮动
+ `position`属性
    + `position`代表定位，有三种属性分别是`absolute`绝对定位、`relative`相对定位、`fixed`固定定位
    + `absolute`，使元素脱离文档流，相对于最接近的一个具有定位属性的父元素进行绝对定位，如果不存在父元素则相对于浏览器窗口
    + `relative`，使用`relative`的时候文件并没有脱离本身的文档流，原来的位置还在占用着，这个属性的偏移量是相对于原来位置的
    + `fixed`，同样也脱离的文档流，位置是相对于浏览器窗口的固定不变
+ 常用css属性记录
    + https://github.com/xluos/note-and-blog/blob/master/html%E8%AE%B0%E5%BD%95.md
# Day课程补充知识

+ [w3school CSS资料](http://www.w3school.com.cn/css/index.asp)

+ [w3school CSS z-index属性](http://www.w3school.com.cn/cssref/pr_pos_z-index.asp)

+ [饥人谷-制作个人简历](https://ke.qq.com/webcourse/index.html#course_id=192657&term_id=100228265&taid=1148547269587089&vid=w1417nevzms)

+ [职坐标- CSS 定位于DIV布局](https://ke.qq.com/webcourse/index.html#course_id=198306&term_id=100235071&taid=1252309384496802&vid=y14003f1tqb)

+ [MDN-CSS布局](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Getting_started/Layout)

# Day3直播内容
直播主要讲了用CSS3的动画制作自己的loading，但是Day3的直播是提前的，涉及到Day4的内容，所以这一块我决定等看完Day4以后，在Day4中详细的写。

# Day3总结
css部分因为之前的基础很容易就过了，但是晚上的直播课因为涉及到比较多的`CSS3`的动画，之前这一块儿还没有接触，虽然看着很容易和用AE做动画差不多，但是具体的属性都不是很了解所以自己做就做不出来了，还是需要去看看那些属性的用法。
# Day3干货分享
2017年最受欢迎的在线编程挑战网站 
https://zhuanlan.zhihu.com/p/30164522 

为什么整个互联网行业都缺前端工程师 
https://zhuanlan.zhihu.com/FrontendMagazine/20598089