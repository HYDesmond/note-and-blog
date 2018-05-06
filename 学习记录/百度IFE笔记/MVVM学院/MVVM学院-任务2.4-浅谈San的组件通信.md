# 前言
这章标题上写的是事件处理,其实更多考验的是组件之间的通信,通过看文档我们会发现(参考资料给出了相关内容的文档,不过还是建议文档全看)组件之间的通信的方法,无非就是三种:
+ 父组件->子组件通信,通过将传入属性值,在子组件中就可以使用该属性
+ 子组件->父组件通信,可以通过**自定义事件**通信也可以通过**消息**进行通信
    + 自定义事件进行直接父子元素之间的通信
    + 消息进行子组件与更高父组件进行通信
+ 子组件->更高层父组件,只能通过消息来通信
> 消息将沿着组件树向上传递，直到遇到第一个处理该消息的组件，则停止。通过 messages 可以声明组件要处理的消息。messages 是一个对象，key 是消息名称，value 是消息处理的函数，接收一个包含 target(派发消息的组件) 和 value(消息的值) 的参数对象。

了解了这些之后我们这节课的内容就很轻松了,首先既然是在学San那就让我们先忘掉其他框架的用法(其实其他框架也不会只看过文档尴尬),按照任务要求一步步来

# 定义组件结构
因为任务比较简单我就没有使用webpack直接写到单文件中了,定义下面三层的组件结构:
```js
// 子组件
var input_component = san.defineComponent({
    // 这里要利用双向数据绑定,否则就获取不到在子组件输入的值了
    template: `<div>
                    <label for="ic">子组件：<input type="text" id="ic" value="{= input =}"></label>
                    <input type="button" value="通知父组件" on-click="message">
                </div>`,
});
// 子组件的直接父组件，子组件通过事件和他通信
var input_superior = san.defineComponent({
    // 引入子组件
    components: {
        "ui-input": input_component
    },
    // 对父组件input中的数据进行双向绑定，达到修改父组件子组件的值也修改的效果
    template: `<div>
                    <ui-input on-customclick="customClicker" input="{{ messagetext }}"></ui-input>
                    <label for="is">父组件：<input class="upper" type="text" id="is" value="{= messagetext =}"></label>
                </div>`,
});
// 最上层的”祖宗“组件，子组件通过消息和他通信
var App = san.defineComponent({
    // 引入父组件
    components: {
        "ui-input-superior": input_superior
    },
    // 因为没有要求所以祖宗组件就不进行双向绑定，只对父组件进行双向绑定
    template: `<div>
                    <ui-input-superior messagetext="{{ app_text }}"></ui-input-superior>
                    <label for="app">更高层的父组件：<input class="top" type="text" id="app" value="{{ app_text }}"></label>
                </div>`,
});
```

# 实现子组件与父组件和高层组件的通信
之前说过,直接父子组件之间通过自定义事件来通信与更高层之间通过消息来通信,那么下面来给button添加一个点击事件,通过`fire方法`来派发一个自定义事件(所谓派发就是触发这个事件绑定的函数,系统事件都有相应的行为触发,自定义事件没有相应行为我们就只能利用代码的逻辑来派发这些事件),然后使用`dispatch方法`来派发消息,消息会沿着组件树一直向上传递直到有组件处理它.
```js
// button点击事件触发的函数,向父组件派发一个自定义事件,同时派发一个消息交给更高层组件处理
message: function () {
    let value = this.data.get("input");
    // 派发自定义事件
    this.fire("customclick", value)
    // 派发消息
    this.dispatch('UI:alerts', value);
}

// 在父组件中用`on-`绑定自定义事件,并设置好触发函数
<ui-input on-customclick="submitClicker" input="{{ messagetext }}"></ui-input>

submitClicker: function (arg) {
                this.data.set("messagetext", arg);
            }

// 更高层组件中则是使用`messages`来接受派发的消息
// 处理子组件派发的消息
messages: {
    "UI:alerts": function (arg) {
        this.data.set("app_text", arg.value);
    },
},
```
虽然我们的例子中可以使用分别派发两个消息的方式来与两个高层组件通信,或者通过连续的自定义事件来通过高层自定义事件间接触发更高层的事件来达到通信的方法,但是课程这样设计显然就是为了让我们学习使用两种通信的方法,在合适的地方用合适的方法,所以还是不要偷懒的好.

# 父组件与子组件通信的问题
现在我们已经能解决子组件变化可以通知父组件和高层组件变化文案,接下来进行父组件控制子组件来变化,最开始就说了父与子的通信**直接使用属性来传值**,要更改父组件的值则**使用双向数据绑定就可以了**.那么这个问题现在就变得很简单了,将父组件的值绑定到子组件的属性上就可以了.利用双向数据绑定就可以很方便的更改他们的值.

*注意:通过属性给子组件传值后,因为值是通过属性传递而来的,`initData`初始化的方法就不在有效了,改为利用`inited`生命周期钩子来初始化值*

# 最后
其实还是看官方文档的好,我这通篇都是大白话,只不过把我的理解写了出来,文档还是更正规.

完整代码就不贴了,有凑字数的嫌疑,可以直接在[GitHub](https://github.com/xluos/ife/blob/gh-pages/MVVM%E5%AD%A6%E9%99%A2%E2%80%94SAN%E6%A1%86%E6%9E%B6/task2.4/index.html)上看,**注释写的很清楚**
[Demo地址](https://xluos.github.io/ife/MVVM%E5%AD%A6%E9%99%A2%E2%80%94SAN%E6%A1%86%E6%9E%B6/task2.4/)

