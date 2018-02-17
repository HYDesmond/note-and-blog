# Day5课程内容
Day5讲的是JS的基本知识，有编程基础的都不会难理解。
## js知识整理（持续更新中）
+ https://github.com/xluos/note-and-blog/blob/master/Javascript%E8%AE%B0%E5%BD%95.md

## 对象的操作
因为有编程基础所以知道对象和实例是怎么回事儿
+ 定义：js中的对象定义很容易也很方便，使用大括号包裹键值对定义如下图：
    ![TIM截图20171226163757.png](http://www.xluos.com/usr/uploads/2017/12/1730028395.png)
+ 定义对象还有很多种方法
+ 当key不是合法字符时，需要用引号包裹，访问方法为`Obj['key']`,key为合法字符时使用 ` . ` 来访问如：`Obj.key`（合法字符即：仅有数字、字母、下划线和$组成，且必须以字母、下划线或$开头）
+ js中的对象是动态类型可以直接用访问的方法为对象添加属性。如：
    ```js
    var stu = {
        name: '小明'
    }
    stu.age = 20;
    stu.age; // undefined
    stu.age = 20; // 新增一个age属性
    stu.age; // 20
    delete stu.age; // 删除age属性
    stu.age; // undefined
    delete stu['name']; // 删除name属性
    stu.name; // undefined
    delete stu.school; // 删除一个不存在的school属性也不会报错
    ```
## 函数
+ js中函数使用关键字`function`定义，定义方法如下：
    ![TIM截图20171226170754.png](http://www.xluos.com/usr/uploads/2017/12/4086292101.png)
    ```js
    // 定义语法 第一种
    function fname(参数列表) {
        //函数体
        return //返回值，返回什么根据情况而定
    }

    // 定义语法 第二种
    var fneme = function (参数列表) {
        //函数体
        return //返回值，返回什么根据情况而定
    }

    ```
+ 两种定义方法的区别，第一种无论写在那里都可以调用，而第二种必须在函数后面才能调用，为什么呢？
    1. js的解释器会在碰到第一种方式定义的函数时会在全局声明这个函数
    2. 第二种其实就是定义了一个匿名函数，然后将这个匿名函数复制给了变量`fname`，所以如果在函数前调用的话，这个变量还没有定义，当然不能用了
## 函数作用域
![TIM截图20171226172352.png](http://www.xluos.com/usr/uploads/2017/12/808550425.png)
+ 用`var`定义的是局部变量，在函数之外不能访问，没有用`var`的变量是全局的
+ 内部可以**层层往上**访问父级的作用域

## 闭包
刚说到作用域，js中说到作用域就不能不说闭包
> **闭包**是指有权访问另一个函数作用域中的变量的函数——《javascript高级程序设计》

>[维基百科中闭包的解释](https://zh.wikipedia.org/w/index.php?title=%E9%97%AD%E5%8C%85_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)&variant=zh-cn)
在计算机科学中，**闭包**（英语：Closure），又称**词法闭包**（Lexical Closure）或**函数闭包**（function closures），是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。所以，有另一种说法认为**闭包是由函数和与其相关的引用环境组合而成的实体。** 闭包在运行时可以有多个实例，不同的引用环境和相同的函数组合可以产生不同的实例。

并不是说一个函数返回另一个函数就是闭包了，如果那个函数没有访问其他函数作用域中的变量就不能算是闭包

使用闭包可以在函数外部访问函数内的局部变量，及时函数执行完毕相关变量也不会被销毁
```js
function f1() {
    var name = '小明';
    var n = 0;
    return function (){
        console.log("n = ",n);
        console.log("name = ",name);
        n++;
    }
}

var f2 = f1();

f2();
// n =  0
// name =  小明
f2();
// n =  1
// name =  小明
```
# Day5课程补充知识
+ w3cscool学习数据类型 （http://www.w3school.com.cn/js/js_functions.asp）
+ w3school js学习、运算符、 if、for、switch （http://www.w3school.com.cn/js/js_functions.asp）

# Day5总结
这一天讲的是js基础知识，虽然只是基础，但是一天的时间也是远远不够的，老师讲的比较浅还是需要自己下去多看书多钻研，关于闭包了目前的理解仅仅只有这么多。今后会慢慢的理解的更深刻。
# Day5干货分享
+ 如何在三年内快速成长为一名技术专家 
http://www.jianshu.com/p/c4a597ab8dc1 
+ 常用的正则表达式整理 
http://www.jianshu.com/p/dcbbfaf424cf 