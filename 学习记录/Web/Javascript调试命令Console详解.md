# Javascript调试命令——你只会Console.log() ?

> Console 对象提供对浏览器控制台的接入（如：Firefox 的 Web Console）。不同浏览器上它的工作方式是不一样的，但这里会介绍一些大都会提供的接口特性。
> Console对象可以在任何全局对象中访问，如 Window，WorkerGlobalScope 以及通过属性工作台提供的特殊定义。
> 它被浏览器定义为 Window.Console，也可被简单的 Console 调用。

最常用的方法就是`Console.log()`,就是在控制台输出内容。刚开始学前端的时候看到大家都是用的`Console.log()`,几乎没有见过`Console`的其他用法，难道`Console`真的没有别的用法了？查了一下后发现`Console`还是非常强大的，至于为什么很少看到有人用可能是因为用过都删掉了吧。在此记录一下`Console`的其他用法。

注意：因为**Console 对象提供对浏览器控制台的接入** 所以在不同浏览器中的支持及表现形式可能不太一样，但是调试内容只有我们开发者会看，所以保证开发环境能用这些方法就可以了，下面演示全部都为`Chrome`上面的效果。

# 分类输出
不同类别信息的输出

```js
console.log('文字信息');
console.info('提示信息');
console.warn('警告信息');
console.error('错误信息');
```
![1.png](http://www.xluos.com/usr/uploads/2018/01/1306895526.png)

# 分组输出
使用`Console.group()`和`Console.groupEnd()`包裹分组内容。

还可以使用`Console.groupCollapsed()`来代替`Console.group()`生成折叠的分组。
```js
console.group('第一个组');
    console.log("1-1");
    console.log("1-2");
    console.log("1-3");
console.groupEnd();

console.group('第二个组');
    console.log("2-1");
    console.log("2-2");
    console.log("2-3");
console.groupEnd();
```
![2.png](http://www.xluos.com/usr/uploads/2018/01/2429050140.png)

`Console.group()`还可以嵌套使用

``` js
console.group('第一个组');
    console.group("1-1");
        console.group("1-1-1");
            console.log('内容');
        console.groupEnd();
    console.groupEnd();
    console.group("1-2");
        console.log('内容');
        console.log('内容');
        console.log('内容');
    console.groupEnd();
console.groupEnd();

console.groupCollapsed('第二个组');
    console.group("2-1");
    console.groupEnd();
    console.group("2-2");
    console.groupEnd();
console.groupEnd();
```
![](http://www.xluos.com/usr/uploads/2018/01/1235926047.png)
# 表格输出
使用`console.table()`可以将传入的对象，或数组以表格形式输出。适合排列整齐的元素

``` js
var Obj = {
    Obj1: {
        a: "aaa",
        b: "bbb",
        c: "ccc"
    },
    Obj2: {
        a: "aaa",
        b: "bbb",
        c: "ccc"
    },
    Obj3: {
        a: "aaa",
        b: "bbb",
        c: "ccc"
    },
    Obj4: {
        a: "aaa",
        b: "bbb",
        c: "ccc"
    }
}

console.table(Obj);

var Arr = [
    ["aa","bb","cc"],
    ["dd","ee","ff"],
    ["gg","hh","ii"],
]

console.table(Arr);
```

![](http://www.xluos.com/usr/uploads/2018/01/1472405887.png)

# 查看对象
使用`Console.dir()`显示一个对象的所有属性和方法
在Chrome中`Console.dir()`和`Console.log()`效果相同

```js
var CodeDeer = {
    nema: 'CodeDeer',
    blog: 'www.xluos.com',
        
}
console.log("console.dir(CodeDeer)");
console.dir(CodeDeer);

console.log("console.log(CodeDeer)");
console.log(CodeDeer);

```
![](http://www.xluos.com/usr/uploads/2018/01/862549852.png)

# 查看节点
使用`Console.dirxml()`显示一个对象的所有属性和方法
在Chrome中`Console.dirxml()`和`Console.log()`效果相同

百度首页logo的节点信息
![](http://www.xluos.com/usr/uploads/2018/01/2663788093.png)

# 条件输出
利用`console.assert()`,可以进行条件输出。

+ 当第一个参数或返回值为真时，不输出内容
+ 当第一个参数或返回值为假时，输出后面的内容并抛出异常

``` js
console.assert(true, "你永远看不见我");
console.assert((function() { return true;})(), "你永远看不见我");

console.assert(false, "你看得见我");
console.assert((function() { return false;})(), "你看得见我");

```
![](http://www.xluos.com/usr/uploads/2018/01/3512106462.png)

# 计次输出
使用`Console.count()`输出内容和被调用的次数

``` js
(function () {
    for(var i = 0; i < 3; i++){
        console.count("运行次数：");
    }
})()
```
![](http://www.xluos.com/usr/uploads/2018/01/1344249262.png)

# 追踪调用堆栈
使用`Console.trace()`来追踪函数被调用的过程，在复杂项目时调用过程非常多，用这个命令来帮你缕清。

```js
function add(a, b) {
    console.trace("Add function");
    return a + b;
}

function add3(a, b) {
    return add2(a, b);
}

function add2(a, b) {
    return add1(a, b);
}

function add1(a, b) {
    return add(a, b);
}

var x = add3(1, 1);
```
![](http://www.xluos.com/usr/uploads/2018/01/3931158865.png)

# 计时功能

使用`Console.time()`和`Console.timeEnd()`包裹需要计时的代码片段，输出运行这段代码的事件。
+ `Console.time()`中的参数作为计时器的标识，具有唯一性。
+ `Console.timeEnd()`中的参数来结束此标识的计时器，并以毫秒为单位返回运行时间。
+ 最多同时运行10000个计时器。
+ 
``` js
console.time("Chrome中循环1000次的时间");
for(var i = 0; i < 1000; i++)
{

}
console.timeEnd("Chrome中循环1000次的时间");

```

![](http://www.xluos.com/usr/uploads/2018/01/1462258827.png)

# 性能分析

使用`Console.profile()`和`Console.profile()`进行性能分析，查看代码各部分运行消耗的时间，但是我在Chrome自带的调试工具中并没有找到在哪里查看这两个方法生成的分析报告。应该需要其他的调试工具。

具体参考这里：
http://www.oschina.net/translate/performance-optimisation-with-timeline-profiles

# 有趣的Console.log()

最后再来介绍一下强大的`Console.log()`,这个方法有很多的用法（其他输出方法的用法，如error()等，可以参照log()使用）。

## 一、提示输出
可以再输出的对象、变量前加上提示信息，增加辨识度
``` js 
var ans = 12345;
console.log("这是临时变量ans的值：",ans);
```
![](http://www.xluos.com/usr/uploads/2018/01/1904893319.png)

## 二、格式化输出

|占位符|含义|
|:---|:---|
|%s|字符串输出|
|%d or %i|整数输出|
|%f|浮点数输出|
|%o|打印javascript对象，可以是整数、字符串以及JSON数据|

样例：
``` js
var arr = ["小明", "小红"];

console.log("欢迎%s和%s两位新同学",arr[0],arr[1]);

console.log("圆周率整数部分：%d，带上小数是：%f",3.1415,3.1415);
```
![](http://www.xluos.com/usr/uploads/2018/01/3241049905.png)

## 三、自定义样式

使用`%c`为打印内容定义样式,再输出信息前加上`%c`，后面写上标准的css样式，就可以为输出的信息添加样式了

``` js
console.log("%cMy stylish message", "color: red; font-style: italic");

console.log("%c3D Text", " text-shadow: 0 1px 0 #ccc,0 2px 0 #c9c9c9,0 3px 0 #bbb,0 4px 0 #b9b9b9,0 5px 0 #aaa,0 6px 1px rgba(0,0,0,.1),0 0 5px rgba(0,0,0,.1),0 1px 3px rgba(0,0,0,.3),0 3px 5px rgba(0,0,0,.2),0 5px 10px rgba(0,0,0,.25),0 10px 10px rgba(0,0,0,.2),0 20px 20px rgba(0,0,0,.15);font-size:5em");

console.log('%cRainbow Text ', 'background-image:-webkit-gradient( linear, left top, right top, color-stop(0, #f22), color-stop(0.15, #f2f), color-stop(0.3, #22f), color-stop(0.45, #2ff), color-stop(0.6, #2f2),color-stop(0.75, #2f2), color-stop(0.9, #ff2), color-stop(1, #f22) );color:transparent;-webkit-background-clip: text;font-size:5em;');

console.log('%cMy name is classicemi.', 'color: #fff; background: #f40; font-size: 24px;');
```

![](http://www.xluos.com/usr/uploads/2018/01/2867117387.png)

# 总结

Console的用法很多，有些再调试过程中非常实用，可以节省很多时间。当然我知道debug还是用断点调试的方法比较好，但是小问题用“printf大法”也是很好用的（滑稽脸）。


# 参考资料

> 1. https://developer.mozilla.org/zh-CN/docs/Web/API/Console
> 2. http://www.jb51.net/article/56504.htm
> 3. https://segmentfault.com/a/1190000000481884
> 4. https://www.cnblogs.com/liyunhua/p/4529079.html#_label18