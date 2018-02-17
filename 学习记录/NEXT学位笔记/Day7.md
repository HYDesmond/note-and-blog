# Day7课程内容
+ **this**
![this](http://www.xluos.com/usr/uploads/2017/12/3505202749.png)
个人理解为，谁调用谁就是`this`，全局下面`this`就是`window`，关键是分清到底是谁调用了这个带`this`的方法，谁就是`this`
+ **对象和实例**
在其他语言中大概就是**类与对象**，对象是对一类事物的抽象出来的描述并没有具体的存在与内存中也没有具体的数值，而实例是对象的实例化，具体存在于内存，有数值，可以操作。
![](https://pic3.zhimg.com/50/4112d9dd17779326c86fa68a54848eaf_hd.jpg)
一图圣千言来自https://link.zhihu.com/?target=http%3A//www.4guysfromrolla.com/webtech/chapters/BuildASPNETWebSite/ch02.2.shtml
+ **工厂模式**
> 抽象工厂模式提供了一种方式，可以将一组具有同一主题的单独的工厂封装起来。在正常使用中，客户端程序需要创建抽象工厂的具体实现，然后使用抽象工厂作为接口来创建这一主题的具体对象。客户端程序不需要知道（或关心）它从这些内部的工厂方法中获得对象的具体类型，因为客户端程序仅使用这些对象的通用接口。抽象工厂模式将一组对象的实现细节与他们的一般使用分离开来。——维基百科

工厂模式就是想一个工厂一样把原料丢进去，就会生产出来一个需要的产品，不用关系这个产品是怎么生产的。

![图像 016.png](http://www.xluos.com/usr/uploads/2017/12/1609244214.png)

但是，工厂模式创建对象有一个弊端就是很难准确的判断对象到底是什么类型的，是谁的对象。
+ **构造函数**
    ![构造](http://www.xluos.com/usr/uploads/2017/12/2694022570.png)

    使用构造函数
    ![函数](http://www.xluos.com/usr/uploads/2017/12/2823934297.png)
    构造函数的声明：
    **约定俗成的构造函数首字母大写**
    ![图像 017.png](http://www.xluos.com/usr/uploads/2017/12/4161860130.png)
    一定要使用 `new` 关键字来创建对象，这样再能使用 `instanceof`关键字来判断对象是什么类型的。

+ **原型portotype**
    > 我们创建的每个函数都有一个 `prototype` （原型）属性，这个属性是一个指针，指向一个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。如果按照字面意思来理解，那么 `prototype` 就是通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。——《js高程》

    ![js高程](http://www.xluos.com/usr/uploads/2017/12/995197295.png)
    关于原型感觉自己理解的还不是很深，可能要到项目中实际用的多了才能慢慢领悟吧。
+ **什么是继承**

    **继承**可以使子类具有**父类**的**属性和方法**，而不需要重复编写相同的代码，将某些类中共同的属性方法提取出来，形成一个父类，让这些类继承这个父类从而达到减少编写重复代码的效果，js中使用原型链来实现继承
    ![TIM截图20171228145036.png](http://www.xluos.com/usr/uploads/2017/12/1386106251.png)
+ **什么是原型链**
    ![图像 018.png](http://www.xluos.com/usr/uploads/2017/12/2917318730.png)
    ![原型链](http://www.xluos.com/usr/uploads/2017/12/538482539.png)
# Day7课程补充知识
+ 阮一峰 继承机制 （http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html）
+ MDN Canvas教程（https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial）
# Day7总结
如果说上一节感觉课程是跳跃的，那这一节很明显的看出来课程是不完整的，可能是这一届概念比较重要也比较难懂，没办法简单的讲只能从正式课程中抽出来几节介绍性的，看完确实很迷糊，又跑去看了看书上的介绍依然迷糊。这一节真的是看的难受，这应该是js的难点所在了，慢慢啃书本吧 ╮(╯▽╰)╭
# Day7干货分享
28个纯CSS加载loading动画特效
http://www.qdfuns.com/notes/40816/f82b78b283e1ca85fa57d8e7c65ca31b.html 
css小细节总结
http://www.qdfuns.com/notes/47113/f61186cc48e057038e6f39ef212b24f3.html 