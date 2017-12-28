# Day6课程内容
这一节的内容是DOM，之前一直再看基础的部分，DOM、BOM、事件这一块没怎么接触过，正好借这次机会接触一下这些。当然以后还是要继续深入的。
## DOM是什么？
全称**文档对象模型（Document Object Model）**，是代码到渲染效果的中间过程，是JS操作页面的桥梁（个人理解）

>Document 接口提供了一些在浏览器服务中作为页面内容入口点而加载的一些页面，也就是 DOM 树。 DOM 树包括诸如`<body>`和`<table>`之类的元素，及其他元素。其也为文档（document）提供了全局性的函数，例如获取页面的 URL、在文档中创建新的 element 的函数。——[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)


![TIM截图20171226181851.png](http://www.xluos.com/usr/uploads/2017/12/2922839190.png)
## DOM操作
### DOM查找
`document`和`Element`的区别，`document`是整个页面，页面载入时整个页面会成为一个`document`对象。`element`表示的是一个`HTML`元素对象，是`document`中的一个节点
![](http://www.xluos.com/usr/uploads/2017/12/2603358435.png)
这些查找方法他们的返回值都是`element`对象或`element`对象集合
+ `document.getElementById()`
    + 从名字就可以看出来通过`元素ID`来查找元素
+ `[document | Element].getElementsByClassName()`
    + 通过`class`查找，返回是多个元素，是一个“集合”，可以使用数组方法
+ `[document | Element].getElementsByTagName()`
    + 通过标签名查找
+ `[document | Element].querySelector()`
    + 通过css选择器选择元素，参数为任何合法的`css选择器`，有多个的情况只选中第一个
+ `[document | Element].querySelectorAll()`
    + 和`querySelector()`用法一样，但是可以选中所有的，返回是一个“集合”
### DOM新增与删除
+ `document.createElement()`创建元素节点。参数：元素名称的字符串
+ `document.createTextNode()`创建文本节点。参数：文本字符串
+ `document.createAttribute()`创建一个属性节点。参数：属性名（比如：`id`、`class`、`style`、`src`、`onclick`等）
    + 访问`value`属性，为这个属性写入值
+ `document.createComment()`createComment() 方法可创建注释节点。参数：注释字符串

+ insertBefore() 前面插入元素节点注释节点
    + 语法：`node.insertBefore(newnode,existingnode)`  **注意：node为插入节点的父节点**
    + `newnode` 必须。要插入的节点对象
    + `existingnode`  必须。(新节点的参照对象)
    + 如果插入的节点是文档中已有的节点，会先删除那个节点然后插入。
    + 返回值：节点对象，你插入的那个节点
+ appendChild() 后面插入元素节点注释节点
    + 语法：`NODE.appendChild(node)` **注意：NODE为插入节点的父节点**
    + 参数：`node` 你要添加的节点对象。
+ setAttributeNode() 插入属性节点
    + 语法：`element.setAttributeNode(attributenode)`
    + 返回值：替换的属性节点，没有则为`null`
+ removeChild() 删除元素节点注释节点
    + 语法：`node.removeChild(node)` **第一个node是删除节点的父节点**
    + 返回值：删除的节点
+ removeNamedItem() 和removeAttribute() 删除属性
    + 语法：`namednodemap.removeNamedItem(nodename)`
    + `element.removeAttribute(attributename)`
    ![](http://www.xluos.com/usr/uploads/2017/12/3024108033.png)
    + `attributes 属性`：返回指定节点属性的集合。
    + `removeAttribute()`更方便
+ 如何选择一个节点的子节点
    + 使用`childNodes`属性，会返回一个子节点集合（这个属性包含空白字符，空白字符为一个`text`节点）
    + 使用`children`属性，获得一个不包含空白字符的集合
    + 使用`firstElementChild`属性，获得第一个节点（不含空白字符）
    + 使用`firstChild`属性，获得第一个节点（含空白字符）
    + 使用`listElementChild`属性，获得最后一个节点（不含空白字符）
    + 使用`listChild`属性，获得最后一个节点（含空白字符）
+ DocumentFragment
    + 这是一个游离在DOM树之外的代码片段，如果要插入大量节点时（循环插入），频繁的操作DOM树会导致频繁渲染页面卡顿，所以可以使用这种方法来加速，现将插入的操作添加进`DocumentFragment` 中，然后将`DocumentFragment`一次性插入DOM树。
    + 下面是示例
    ```js
    var body = document.body;
    var len = 100;
    var i = 0;
    var flagment = document.createDocumentFragment(); // 使用 fragment 来提高性能
    var ul = document.createElement('ul');
    var li;
    var textNode;

    body.appendChild(ul);

    for (; i < len; i++) {
        li = document.createElement('li');
        textNode = document.createTextNode(i + 1);
        li.appendChild(textNode);
        // 添加到虚拟节点中
        flagment.appendChild(li);  
    }
    // 将虚拟节点插入DOM树
    ul.appendChild(flagment);
    ```
推荐 [菜鸟教程](http://www.runoob.com/jsref/dom-obj-document.html) 和 [W3School](http://www.w3school.com.cn/jsref/dom_obj_document.asp)

点击查看[demo]()
## 事件
事件无处不在，用户的所有操作都是时间，鼠标的滑动、点击、按下、弹起，窗口改变、按键、滑动等等等都是时间

通过事件我们可以响应用户的一些操作，根据用户不同的操作来改变不同的东西，比如点击不同的按钮改变背景颜色，点击按钮的次数改变颜色等等
### 事件模型
早期分为两种事件冒泡和事件捕获

![微信截图_20171226221655.png](http://www.xluos.com/usr/uploads/2017/12/2199818116.png)

现代的DOM事件流是下面这样的过程

![TIM截图20171226221729.png](http://www.xluos.com/usr/uploads/2017/12/165662479.png)
### 事件类型（事件句柄）
#### 鼠标类

|属性|描述|
|:---|:---|
|click	|当用户点击某个对象时调用的事件句柄。|
|contextmenu	|在用户点击鼠标右键打开上下文菜单时触发|
|dblclick	|当用户双击某个对象时调用的事件句柄。|
|mousedown|	鼠标按钮被按下。|
|mouseenter|	当鼠标指针移动到元素上时触发。|
|mouseleave|	当鼠标指针移出元素时触发|
|mousemove|	鼠标被移动。|
|mouseover|	鼠标移到某元素之上。|
|mouseout	|鼠标从某元素移开。|
|mouseup	|鼠标按键被松开。|
#### 键盘类

|属性|描述|
|:---|:---|
|keydown|	某个键盘按键被按下。|
|keypress|	某个键盘按键被按下并松开。|
|keyup	|某个键盘按键被松开。|
#### 其他事件
[点击查看更多事件](http://www.runoob.com/jsref/dom-obj-event.html)
### 事件绑定与解绑
一、通过DOM操作绑定
推荐用这种方法，并且通过DOM操作绑定的事件可以同一事件多次绑定。
+ 语法：`[document | Element].addEventListener(event, function, useCapture)`

    |属性|描述|
    |:---|:---|
    |event|必需。描述事件名称的字符串。**注意： 不要使用 "on" 前缀。例如，使用 "click" 来取代 "onclick"。**|
    |function|必需。描述了事件触发后执行的函数。当事件触发时，事件对象会作为第一个参数传入函数。 事件对象的类型取决于特定的事件。例如， "click" 事件属于 MouseEvent(鼠标事件) 对象。|
    |useCapture|可选。布尔值，指定事件是否 在捕获或冒泡阶段执行。默认`false`，事件句柄在冒泡阶段执行|
+ 解绑事件：`[document | Element].removeEventListener(event, function, useCapture)`

    |属性|描述|
    |:---|:---|
    |event|必须。要移除的事件名称。注意： 不要使用 "on" 前缀。 例如，使用 "click" ,而不是使用 "onclick"。|
    |function|必须。指定要移除的函数。|
    |useCapture|可选。布尔值，指定移除事件句柄的阶段。**注意： 如果添加两次事件句柄，一次在捕获阶段，一次在冒泡阶段，你必须单独移除该事件。**|
二、直接绑定至属性
+ 语法：
+ **注意： 直接写到属性需要使用 "on" 前缀。例如，使用 "onclick" 来取代 "click"。**
    ```js
    //HTML中
    <element onclick="SomeJavaScriptCode">
    //js中
    object.onclick=function(){SomeJavaScriptCode};
    ```
+ `SomeJavaScriptCode`：必须，规定该事件发生时执行的 JavaScript。
+ 解绑事件：删除该属性即可解绑该事件

# Day6课程补充知识
+ 职坐标-常见的事件类型 （https://ke.qq.com/webcourse/index.html#course_id=8493&term_id=100133314&taid=532288181903661&vid=j1400yqwbbb）
+ 尚硅谷JavaScript DOM全套视频 （https://ke.qq.com/course/26269#tuin=b64d5a3e）

有两个404了就不放了
# Day6总结
这一节的内容也比较多，查了很长时间零零碎碎的总结了这么多，看着课程有跳跃的感觉，可能是从完整版中挑出来的几节吧，DOM和事件应该是JS的关键几个点儿，这个仅仅只能满足基本操作，各种属性方法浩如烟海还是需要满满积累。
# Day6干货分享
程序猿小白应该注意什么
https://juejin.im/post/59e96c756fb9a0450a6679da 

前端移动端适配总结
https://juejin.im/entry/59e70320f265da431c6f6514 