# JS模块化总结
对于前端模块化的概念一直很模糊，总是各种方法混用，没有好好总结过。模块化的概念优点什么的就不废话，一搜一大把，直接上干货。
## CommonJS
CommonJS是Node.js使用的模块化标准，主要用在服务端。在CommonJS中，有一个全局性方法require()，用于加载模块。
下面看看如何使用：
+ 定义模块
    根据CommonJS规范，一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，也就是说，在该模块内部定义的变量，无法被其他模块读取，除非定义为`global`对象的属性
+ 模块输出：
    模块只有一个出口，`module.exports`对象，我们需要把模块希望输出的内容放入该对象
+ 加载模块：
    加载模块使用`require`方法，该方法读取一个文件并执行，返回文件内部的`module.exports`对象

举个例子：
文件A定义一个模块
```js
var a = {};
var b = [];
var c = function(){};
module.exports = {
    A: a,
    B: b,
    C: c
}  

```
文件B使用这个模块
```js
const A_module = require('文件A Path')  //使用var也可以，最好使用const

A_module.C();

```
模块内部的变量函数可以随意使用，但是其他文件只能使用`module.exports`对象导出的内容。

## AMD / CMD
因为`CommonJS`是同步的，在浏览器上如果加载很慢就会造成“卡死”的假象，所以在浏览器中需要使用异步加载的方案，那就是`AMD`和`CMD`。由于ES6之前的JavaScript原生不支持模块化，所以就有了各种模块化的库。
> AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。  
> CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。
下面摘自：[谦行博客](http://www.cnblogs.com/dolphinX/p/4381855.html)
### AMD
AMD 即`Asynchronous Module Definition`，中文名是异步模块定义的意思。它是一个在浏览器端模块化开发的规范。
#### 语法
requireJS定义了一个函数 define，它是全局变量，用来定义模块
```js
define(id?, dependencies?, factory);
```
+ `id`：可选参数，用来定义模块的标识，如果没有提供该参数，脚本文件名（去掉拓展名）
+ `dependencies`：是一个当前模块依赖的模块名称数组
+ `factory`：工厂方法，模块初始化要执行的函数或对象。如果为函数，它应该只被执行一次。如果是对象，此对象应该为模块的输出值

在页面上使用`require`函数加载模块:
```js
require([dependencies], function(){});
```
require()函数接受两个参数

+ 第一个参数是一个数组，表示所依赖的模块
+ 第二个参数是一个回调函数，当前面指定的模块都加载成功后，它将被调用。加载的模块会以参数形式传入该函数，从而在回调函数内部就可以使用这些模块
`require()`函数在加载依赖的函数的时候是异步加载的，这样浏览器不会失去响应，它指定的回调函数，只有前面的模块都加载成功后，才会运行，解决了依赖性的问题。
#### 例子
```js
// 定义模块 myModule.js
define(['dependency'], function(){
    var name = 'Byron';
    function printName(){
        console.log(name);
    }

    return {
        printName: printName
    };
});

// 加载模块
require(['myModule'], function (my){
　 my.printName();
});
```
### CMD
CMD 即`Common Module Definition`通用模块定义，CMD规范是国内发展出来的，就像AMD有个requireJS，CMD有个浏览器的实现SeaJS，SeaJS要解决的问题和requireJS一样，只不过在模块定义方式和模块加载（可以说运行、解析）时机上有所不同。

Sea.js 推崇一个模块一个文件，遵循统一的写法，所以
+ 一个文件一个模块，所以经常就用文件名作为模块id
+ CMD推崇依赖就近，所以一般不在define的参数中写依赖，在factory中写
#### 语法
定义模块：
```js
define(id?, deps?, factory)
```
factory函数：
```js
//factory有三个参数
function(require, exports, module)
```
+ `require`：`require(id)`是 factory 函数的第一个参数,`require` 是一个方法，接受 **模块标识** 作为唯一参数，用来获取其他模块提供的接口。
+ `exports`：exports 是一个对象，用来向外提供模块接口
+ `module`： module 是一个对象，上面存储了与**当前模块**相关联的一些属性和方法

#### 例子
```js
// 定义模块  myModule.js
define(function(require, exports, module) {
  var $ = require('jquery.js')
  $('div').addClass('active');
});

// 加载模块
seajs.use(['myModule.js'], function(my){

});
```
### AMD和CMD的区别
>最明显的区别就是在模块定义时对依赖的处理不同
>
>AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块
>CMD推崇就近依赖，只有在用到某个模块的时候再去require
>这种区别各有优劣，只是语法上的差距，而且requireJS和SeaJS都支持对方的写法
>
>AMD和CMD最大的区别是对依赖模块的执行时机处理不同，注意不是加载的时机或者方式不同
>
>很多人说requireJS是异步加载模块，SeaJS是同步加载模块，这么理解实际上是不准确的，其实加载模块都是异步的，只不过AMD依赖前置，js可以方便知道依赖模块是谁，立即加载，而CMD就近依赖，需要使用把模块变为字符串解析一遍才知道依赖了那些模块，这也是很多人诟病CMD的一点，牺牲性能来带来开发的便利性，实际上解析模块用的时间短到可以忽略
>
>为什么我们说两个的区别是依赖模块执行时机不同，为什么很多人认为ADM是异步的，CMD是同步的（除了名字的原因。。。）
>
>同样都是异步加载模块，AMD在加载模块完成后就会执行改模块，所有模块都加载执行完后会进入require的回调函数，执行主逻辑，这样的效果就是依赖模块的执行顺序和书写顺序不一定一致，看网络速度，哪个先下载下来，哪个先执行，但是主逻辑一定在所有依赖加载完成后才执行
>
>CMD加载完某个依赖模块后并不执行，只是下载而已，在所有依赖模块加载完成后进入主逻辑，遇到require语句的时候才执行对应的模块，这样模块的执行顺序和书写顺序是完全一致的
>
>这也是很多人说AMD用户体验好，因为没有延迟，依赖模块提前执行了，CMD性能好，因为只有用户需要的时候才执行的原因

## ES6模块化
在ES6中提供了原生的模块化支持，提供了同步和异步两种独立的模块加载机制。

### 模块定义
ES6中模块定义与CommonJS很像，只不过没有了module这个对象，而直接调用export。 可以export任何一个 函数，变量，对象
```js
//expt.js
export function abc(){}//export 一个命名的function  
export default function(){} //export default function  
export num=123 //export 一个数值  
export obj={}  
export { obj as default };

//import
import expt from 'expt'//default export  
import {default as myModule} from 'expt' //rename  
import {abc,num,obj} from 'expt' 
```
### 模块加载
同步加载使用`import`关键字
```js
import {foo} from module
```
异步加载使用`System.import API`的方式。使用了Promise对象的方式
```js
System.import('some_module')  
	  	.then(some_module => {
		  	// Use some_module
	  	})
	  	.catch(error => {
		  	...
	  	});
```
更多用法看这篇文章：http://2ality.com/2014/09/es6-modules-final.html

## 最后
关于模块化，理解的还很浅显。这篇文章更多的是对一些主流的模块化方法的总结，参考了很多前辈大佬的文章，有一部分还是直接摘抄过来的，因为我确实没有用过哪两个库。写这篇文章的目的也是让自己对几种模块化的方法有一个系统的认识。

## 参考
+ https://cnodejs.org/topic/5641bfbc184a5f7a5b5077fd
+ http://www.ruanyifeng.com/blog/2012/10/javascript_module.html
+ http://www.cnblogs.com/dolphinX/p/4381855.html
+ https://www.cnblogs.com/surfaces/p/5898925.html