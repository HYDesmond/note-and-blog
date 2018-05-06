# 什么是事件循环（Event Loop）
> 为了`协调事件`（event），`用户交互`（user interaction），`脚本`（script），`渲染`（rendering），`网络`（networking）等，`用户代理`（user agent）必须使用`事件循环`（event loops）。
> 有两类事件循环：一种针对`浏览上下文`（browsing context），还有一种针对`worker`（web worker）。

一个事件循环有着至少一个**任务队列**，每个任务队列中都是特定的**任务源**产生的任务，这些任务源通常都是：事件、回调、I/O操作，DOM操作等，同一个任务队列中的任务严格按照先进先出的顺序执行，不同任务队列的任务执行顺序不一定。

当然任务还有`macrotask`和`microtask`之分，在这里不谈他们两个因为我也没有太搞懂这两个东西的区别。

# 为什么要有事件循环
因为JavaScript在浏览器中特点导致了它只能是的单线程的，这样就导致了Javascript中的并发模型不能像传统语言那样多线程操作。
> JavaScript 的并发模型基于"事件循环"。这个模型与像 C 或者 Java 这种其它语言中的模型截然不同。

因为Javascript在浏览器中是单线程，所以在进行一个延时操作时并不能真的就让进程在原地等待那么久，那样用户的相关操作就“卡死”在哪里了，所以引入了**事件循环**的机制来，将需要消耗时间的操作“跳过去”，等主线程的操作完成了，再检查**任务队列**中是否还有任务，然后将任务调出来执行。就这样反复循环执行，所以就叫做**事件循环**。

下面这张图形象的描述了浏览器中JavaScript的执行过程（来自Philip Roberts演讲的PPT）：
![](https://pic2.zhimg.com/80/v2-e902447823cf13e5a547214363233858_hd.jpg)

> 在上图中，调用栈中遇到DOM操作、ajax请求以及setTimeout等WebAPIs的时候就会交给浏览器内核的其他模块进行处理，webkit内核在Javasctipt执行引擎之外，有一个重要的模块是webcore模块。对于图中WebAPIs提到的三种API，webcore分别提供了DOM Binding、network、timer模块来处理底层实现。等到这些模块处理完这些操作的时候将回调函数放入任务队列中，之后等栈中的task执行完之后再去执行任务队列之中的回调函数。

# 事件循环的例子
这里用一个知乎小姐姐文章中的例子，原文[看这里](https://zhuanlan.zhihu.com/p/26229293)

![](https://pic1.zhimg.com/80/v2-9e5a3e686df7e84068575121c1ec9fcd_hd.jpg)

首先main()函数的执行上下文入栈

![](https://pic1.zhimg.com/80/v2-1a9a6a95c4a7d3dc45facb5b2545640d_hd.jpg)

代码接着执行，遇到console.log(‘Hi’),此时log(‘Hi’)入栈，console.log方法只是一个webkit内核支持的普通的方法，所以log(‘Hi’)方法立即被执行。此时输出’Hi’。

![](https://pic4.zhimg.com/80/v2-4a8c8a73009cc73d5efdb833fdf81b03_hd.jpg)

当遇到setTimeout的时候，执行引擎将其添加到栈中。

![](https://pic3.zhimg.com/80/v2-37a7df54154dd521ced4dae2e3470c22_hd.jpg)

调用栈发现setTimeout是之前提到的WebAPIs中的API，因此将其出栈之后将延时执行的函数交给浏览器的timer模块进行处理。

![](https://pic2.zhimg.com/80/v2-800d6abf4bcdef9a354c9d60f2882a26_hd.jpg)

timer模块去处理延时执行的函数，此时执行引擎接着执行将log(‘SJS’)添加到栈中，此时输出’SJS’。

![](https://pic4.zhimg.com/80/v2-e804f07a0d9b0436941e3e48550349b9_hd.jpg)

当timer模块中延时方法规定的时间到了之后就将其放入到任务队列之中，此时调用栈中的task已经全部执行完毕。

![](https://pic1.zhimg.com/80/v2-c3c2686ecfa45b21ff3bc99b0252b90b_hd.jpg)
![](https://pic4.zhimg.com/80/v2-f78ad9009fd2622c139f2da66f61d19b_hd.jpg)

调用栈中的task执行完毕之后，执行引擎会接着看执行任务队列中是否有需要执行的回调函数。这里的cb函数被执行引擎添加到调用栈中，接着执行里面的代码，输出’there’。等到执行结束之后再出栈。

看到这里也就明白了，为什么说setTimeout只是保证最低延时时间，就好比下面的例子
```js
setTimeout(()=>{console.log("1")}, 0);
for(let i=0;i<1000000000;i++);
console.log("2");
```
哪怕setTimeout中的延时时间再短也是先输出2再输出1，因为要等待`stack`中的执行完毕才会去检查任务队列中的任务去执行。

# 最后
到这里差不多就对事件循环有了基本的认识了，这是只是浅谈更复杂的多任务队列会是什么情况不在这里讨论，以上只是个人理解如有不当指出欢迎指出。强烈推荐看**Philip Roberts的演讲：Help, I’m stuck in an event-loop.**，简直就是生动形象的不需要英语都能看懂，[地址在这儿](https://vimeo.com/96425312)需要自备梯子,还有他网站上的一个在线演示的页面[戳这里](http://latentflip.com/loupe/)

# 参考

+ [Philip Roberts: Help, I’m stuck in an event-loop.](https://vimeo.com/96425312)
+ [深入浅出Javascript事件循环机制(上)](https://zhuanlan.zhihu.com/p/26229293)
+ [什么是浏览器的事件循环（Event Loop）？](https://segmentfault.com/a/1190000010622146)
+ [深入理解 JavaScript 事件循环（一）— event loop](https://www.cnblogs.com/dong-xu/p/7000163.html)(这个有在线例子可以看)
+ [MDN:并发模型与事件循环](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop)
+ [JavaScript 运行机制详解：再谈Event Loop](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)