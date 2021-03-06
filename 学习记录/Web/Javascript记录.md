# Javascript学习记录
记录基本语法入门笔记

严正声明，javascript和java完全不是一回事儿，就像雷峰塔和雷锋，苹果手机和苹果一样！！！那些看见我学这个说我在学java的人你们够了！



# 基本语法
js与其他语言相比，差别并不大，有语言基础的比较容易学习。
## 数据类型和变量
介绍js中各种数据类型以及变量的定义方法
### 变量
js中变量统一使用`var`来声明，虽然不用`var`定义也可以使用，但是不用`var`时默认定义全局变量，在某些地方可能会有变量的冲突导致一些诡异的错误，所以一个好的编程习惯都要使用`var`来定义。
### 运算符
js中运算符与其他语言差别不大，但是有几点需要注意
+ js中多了`===`，因为js中使用`==`会自动转换类型比较，而`===`不会，比如
```js
'9' == 9   //ture
'9' === 9  //false
```
+ `+`加号运算符不仅做运算，还作为连接运算符。
    1. 如果`+`（加号）两边都是数字，则肯定是加法运算
    2. 如果`+`两边有`boolean`、`number`类型或`null`值的某一个，则是加法运算，比如：`1+true`是`1`，`true+true`也是`2`,`null + false`是`0`，`1 + null`是`1`
    3. 如果加号两边有最少一边是字符串，则是字符串拼按，比如`1 + "23"`是`"123"`
    4. 如果加号两边最少有一边是是对象类型，这个对象先对象它的`toString`方法，然后再做字符串拼接，比如：（这些涉及到一些对象的原理性的东西，先做简单了解，以后会讲）`5 + [1,2,3,4]`  是`51,2,3,4`   比如`({}+{})`是 `"[object Object][object Object]"`
    5. `数字`、`布尔`、`null`和`undefined`做加运算的结果是`NaN`，
### 空值
> `null`表示一个“空”的值，它和`0`以及空字符串`''`不同，`0`是一个数值，`''`表示长度为`0`的字符串，而`null`表示“空”。
> 在JavaScript中，还有一个和`null`类似的`undefined`，它表示“未定义”。
> JavaScript的设计者希望用`null`表示一个空的值，而`undefined`表示值未定义。事实证明，这并没有什么卵用，区分两者的意义不大。大多数情况下，我们都应该用`null`。`undefined`仅仅在判断函数参数是否传递的情况下有用。

### Number-数值类型
+ js作为弱类型语言对于数值没有区分的那么细，`int`、`float`等统一表示为`Number`。
+ 注意的是js中多了两种特别的Number，`NaN`和`Infinity`
    + `NaN`表示一个无法计算的结果，蔽日`sqrt(-1)`,`0/0`等。**`NaN`不与任何数相同，包括他自己**。可以用`isNaN()`来判断它。
    + `Infinity`表示一个极大值，超过了js的运算范围，比如：`999**999`、`1/0`等。`Infinity`与它自身比较结果为`true`。

### String-字符串
字符串指用单引号`''`或双引号`""`括起来的任意文本,如果这两个符号本身就是字符串的一部分需要使用`\`来转移，转义的字符大致与其他语言相同。
另外js中还提供了一种多行文本的方法用反斜杠包裹字符串**`**,在Esc键下面
#### 模版字符串
js中字符串和变量之间可以用`+`号来连接，但是如果太多了就比较麻烦，所以ES6中又提供了一种模版字符串的写法。使用`${变量名}`写在字符串中，就可以自动替换字符串中的变量。
**模版字符串需要用反引号包裹起来**
#### 字符串操作
js提供的这些方法不会改变原来字符串的内容，而是返回一个新的串
+ 索引字符串，js中的字符串也可以用下标操作取出某个字符，下标不存在时不会报错返回`undefined`
+ 字符串不可变，对某个索引赋值不报错，但是不会改变任何东西
+ `split` 分割字符串，将字符串指定某个字符分割为字符串数组
+ `toUpperCase()` 改为大写
+ `indexOf()`返回子串出现的第一个位置，从`0`开始，子串不存在返回`-1`
```js
var s = 'hello, world';
s.indexOf('world'); // 返回7
s.indexOf('World'); // 没有找到指定的子串，返回-1
```
+ `substring()` 返回指定区间的子串
```js
var s = 'hello, world'
s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
s.substring(7); // 从索引7开始到结束，返回'world'
```

### 数组
js中的数组很奇妙，可以包含任意类型的数据甚至是另一个数组（js中多维数组的写法？），而且数组的`length`属性是可写的。（很奇妙的操作）
```js
var arr = [1, 2, 3];
arr.length; // 3
arr.length = 6;
arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
arr.length = 2;
arr; // arr变为[1, 2]

```
可以对索引直接进行赋值修改，**如果索引的值超出了范围，也会引起数组长度的改变**，未定义的元素值为`undefined`
```js
var arr = [1, 2, 3];
arr[5] = 'x';
arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
```
#### 数组常用方法

+ `indexOf()`  和字符串的方法类似，这个查找不会转换类型及10和'10'不一样
+ `slice()` 截取数组部分元素，与substring用法相同
+ `push()和pop()`  在数组**末尾**添加删除元素，push可以添加多个
+ `unshift()和shift()` 用处与push和pop相反，这两个是在数组**头部**添加和删除头部第一个元素。用法相同
+ `pop()和shift()`,都会在删除元素时返回值为删除的元素
+ `sort`，数组排序方法，默认**字典序升序**排列
+ `reverse()` 反转数组所有元素
+ `splice()`:`splice()`方法是修改`Array`的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：
```js
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
```
+ `concat()`连接两个数组，并返回这个连接起来的新数组，
> 实际上，concat()方法可以接收任意个元素和Array，并且自动把Array拆开，然后全部添加到新的Array里：
```js
var arr = ['A', 'B', 'C'];
arr.concat(1, 2, [3, 4]); // ['A', 'B', 'C', 1, 2, 3, 4]
```
+ `join()`方法，将数组中的元素用指定字符串连接起来，返回这个连接好的字符串。如果数组中的元素不是字符串，js会把他们转换为字符串。

### 两种数据结构Map和Set
这两种数据结构都是ES6规范中新引入的，如果不支持ES6将无法使用
#### Map（映射）
`map`是任意类型的键值对结合形成的集合，具有极快的速度（C++中的Map是利用树来查找时间复杂度`logN`，Javascript中怎么实现目前还不清楚，应该也是类似的结构）。用法如下：
```js
//定义
var mp = new Map([['aaa', 1], ['bbb', 2], ['ccc', 3]]);
// 添加
mp.set('ddd',4);
// 查询是否存在
mp.has('aaa'); //true
mp.has('abc'); //false

// 获取key对应的值
mp.get('bbb'); // 2
mp.get('abc'); // undefined
// 删除
mp.delete('ccc');
```
Map中的key是不能重复的，所以对同一key多次赋值会冲掉前面的值。

#### Set（集合）
集合，顾名思义和数学上的集合一样，值不能重复。所以创建集合和添加元素的时候重复的值会自动删除。Javascript中集合元素的顺序是按照加入的顺序排列的，并不会自动排序。用法如下：
```js
// 定义
var se = new Set([1,21,123]);

// 添加元素
se.add(4);
se.add('0');

//遍历元素
for(var value of se){
    console.log(value);
}
// 1 21 123 4 '0'

// 删除元素
se.delete(123);

//判断元素是否存在
se.has('0');  //true
se.has(123)   //false
```

### 判断

和其他语言一样判断都是`if () { ... } else { ... }`的判断，当然也有`switch(n) {case ...: 执行代码块 break;  default: 代码块}`

### 循环

循环各大语言中最基础的就是下面这三种：
+ `for (var i = 0; i < n; i++) { ... }` 普通for循环
+ `while(表达式) { ... }` while循环
+ `do { ... } while(表达式);` do...while()循环中，循环体至少会执行一次，而上面两种都可能一次都不执行。

下面的两种循环类型其他语言中可能就没有：
+ `for (var key in obj)`循环,可以将对象中的所有属性遍历出来。如果只需要对象自身的属性就需要配合`hasOwnProperty()`来实现。因为数组（Array）也是一个对象，所以也可以遍历，但是遍历出来的是数组的下标而不是元素的值。
+ `for (var value of iterable)` 这是ES6中新增加循环方法，因为遍历`Array`可以采用下标循环，遍历`Map`和`Set`就无法使用下标。为了统一集合类型，ES6标准引入了新的`iterable`类型，`Array`、`Map`和`Set`都属于`iterable`类型。用法如下：
    ```js
    var a = ['A', 'B', 'C'];
    var s = new Set(['A', 'B', 'C']);
    var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
    for (var x of a) { // 遍历Array
        console.log(x);
    }
    for (var x of s) { // 遍历Set
        console.log(x);
    }
    for (var x of m) { // 遍历Map
        console.log(x[0] + '=' + x[1]);
    }
    ```

### 函数和对象
#### 对象的操作
简单的说就是键值对的集合，在另一个笔记里记过就直接copy过来了，键值对之间用逗号隔开，最后一个尾部不加逗号，因为在低版本浏览器中这种写法会报错。
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
+ 使用`in`操作符来判断对象是否具有某种属性,但是这种方法判断出来的可能并不是这个对象本身拥有的也可能是继承来的属性，使用`hasOwnProperty()`来判断元素自身拥有的属性。
```js
'name' in stu;  //true
'toString' in stu; // true

stu.hasOwnProperty('name'); // true
stu.hasOwnProperty('toString'); // false
```
#### 函数
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
#### 函数作用域
![TIM截图20171226172352.png](http://www.xluos.com/usr/uploads/2017/12/808550425.png)
+ 用`var`定义的是局部变量，在函数之外不能访问，没有用`var`的变量是全局的
+ 内部可以**层层往上**访问父级的作用域

#### 闭包
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
# 参考资料

> http://www.w3school.com.cn/js/
> https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000