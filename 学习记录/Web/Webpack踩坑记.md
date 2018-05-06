# 前言
闲扯两句废话，有任务驱动确实做事会高效很多`webpack`这个东西很早就听说了，但是一直抱着我基础还没打好先打完基础再说这些工具，一直没有去学。正好接IFE这次机会学一下`webpack`，也不能说两天就学会吧。最起码了解基本的概念和用法，任务介绍里面给出了两种工具`webpack`和`parcel`两者的文档我都去看了，不得不说`parcel`确实很简单，看了很多评论大家也都奔着`parcel`去了。相比`parcel`的开箱即用html为入口文件，`webpack`确实显得繁琐且晦涩，但是不能因为这个难，配置麻烦就不去学，在网上查了查发现，虽然`parcel`的这种开箱即用可能以后会是主流，但是就目前来看`webpack`的生态还是相当强劲，个人小项目用`parcel`合适，大项目不想当*铁头娃、填坑侠*的话还是去拥抱`webpack`吧，毕竟webpack是目前的主流，并且更多的配置意味着对项目打包的掌控力更强。说实话没有人指导、全靠自己扣这个过程确实很难，折腾了一个下午各种报错，各种不理解。好几次让我产生了还是去学`parcel`的念头，但是可能是曾经无数次的WA锻炼了我强大的神经，最终我还是坚持了下来，对一个工具的理解也就在这一次次的报错中逐渐加深，谨以此一文纪念我一个下午的填坑之旅，希望对他人能有一些帮助。

# 解锁前置技能
首先要学会基本使用方法，才能继续下去。关于webpack的介绍和基本使用我就不赘述了，官方的文档已经写的很舒服了搬运文档只会浪费时间，同时San框架的文档也要看一下。看文档永远都是学习新东西最好的方式，一定要耐着心读下去，照着指南上的步骤来自己试验一下。
+ [webpack中文文档](https://www.webpackjs.com/guides/)
+ [San框架文档](https://baidu.github.io/san/tutorial/start/)

另外吐槽一下San框架的文档有个地方有些过时，需要注意！当时没仔细看在这个报错上面卡了好久（注意画圈的地方已经改了）
![](http://www.xluos.com/usr/uploads/2018/04/1915867683.png)
![](http://www.xluos.com/usr/uploads/2018/04/640362043.png)

看完前两个相信对这两个已经有了初步的了解，可以去看一下官方的这个Demo（[San框架官方Demo](https://github.com/baidu/san/tree/master/example/todos-esnext)），因为San这个框架的资料比较少不像Vue那样网上随便一搜都教程一大把，还是需要借鉴看看官方的Demo，内容很多刚开始可能找不到看的方向**一定要耐心耐心**，如果你是在看不下去没关系，下面跟着我来一步步做一个webpack和san协作的`hello world`

# 准备工作
**node一定要有，版本不要太低**，包管理推荐使用**yarn**当然用npm也一样他们二者是兼容的。这两者怎么安装就不多说了，同样百度一大堆。

## 初始化工具
```
yarn init -y //初始化package.json
yarn add webpack webpack-cli -D  //添加webpack、webpack-cli
```
`-D`相当于`--save-dev`，关于什么时候用`--save`什么时候用`-dev`我摘抄了一段话：
> 在安装一个要打包到生产环境的安装包时，你应该使用 npm install --save，如果你在安装一个用于开发环境的安装包（例如，linter, 测试库等），你应该使用 npm install --save-dev。请在 npm 文档 中查找更多信息。

## 建立目录结构
新建两个文件夹src（存放源码）和dist（存放构建出来的代码）
在dist中新建`index.html`代码如下：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>webpack4 San</title>
</head>
<body>
    <div id="app"></div>
    <script src="main.js"></script>
</body>
</html>
```
src文件夹中新建`main.js`
```js
console.log('hello webpack4 San');
```
然后你的文件目录应该是下面这样的
```bash
├── dist
│   └── index.html
├── package.json
├── src
│   └── main.js
└── yarn.lock
```

# 第一次打包
在终端，文件夹根目录运行下面命令（Node 8.2+ 版本提供的 npx 命令，可以运行在初始安装的 webpack 包(package)的 webpack 二进制文件）：
```bash
npx webpack ./src/main.js
```

紧接着你会发现，dist目录新增了一个`main.js`打开发现正是压缩过的代码，在浏览器打开`index.html` 控制台输出：
```bash
#console
hello webpack4 San
```
webpack4之后也是**无需配置就可以打包**的，但是如果是复杂的大型项目还是需要我们来配置获得更好的体验

# 第二次打包
接下来我们创建一个配置文件,目录结构如下：
```bash
├── dist
│   ├── index.html
│   └── main.js
├── package.json
├── src
│   └── main.js
├── webpack.config.js   //新增文件
└── yarn.lock
```
`webpack.config.js`文件内容：
```bash
const path = require('path');

module.exports = {
  entry: './src/main.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```
再次打包，因为`webpack.config.js`是`webpack`的默认配置所以无需指定配置文件：
```bash
npx webpack
```
`dist`目录下新增了 `bundle.js`文件

# 第三次打包
每次都输入npx太麻烦了，让我们使用一个npm脚本：
```bash
#在package.json中添加下面的内容
"scripts": {
      "start": "webpack-dev-server --mode development --open",
      "build": "webpack --mode production"
    },
```
在上面的脚本中看到了一个没见过的东西`webpack-dev-server`，这就是我们下面要装的，每次构建文件都要手动刷新打开太麻烦，**当文件变化时**让它来自动帮我们构建刷新，下面命令安装：
```bash
yarn add webpack-dev-server -D

#配置文件中添加下列内容，代表服务启动的文件夹是dist
devServer: {
             contentBase: './dist'
    },
```

其实dist文件夹中的内容应该全部都是生成的，现在这样写死一个index非常不方便，所以让我们**先把index.html文件剪切到src文件夹**，用另一个东西来生成它，安装命令：
```
yarn add  html-webpack-plugin -D
```
这个工具用来生成html文件，在`webpack.config.js`添加下列配置来使用它，默认生成文件名是index.html。更多配置参考[GitHub的README](https://github.com/jantimon/html-webpack-plugin)
```
    const HtmlWebpackPlugin = require('html-webpack-plugin');
...省略号...
plugins: [
        new HtmlWebpackPlugin({
            template: './src/index.html' //生成文件参照的模版也可以是其他文件
        })
    ],
```
这时候你的目录结构应该是这样的（注意清理一下你的dist文件夹）：
```
├── dist
├── package.json
├── src
│   ├── index.html
│   └── main.js
├── webpack.config.js
└── yarn.lock
```
接下来我们进行第三次构建：

```
yarn run build  和npm run build等效，不过两个工具最好不要混用

```

可以看到dist目录已经生成了我们想要的文件，并且`bundle.js`文件也引入了index.html中
```
├── dist
│   ├── bundle.js
│   └── index.html
├── package.json
├── src
│   ├── index.html
│   └── main.js
├── webpack.config.js
└── yarn.lock
```

# 第四次构建
接下来我们添加`San`文件，来做一个最简单的`hello world`

首先安装San和san-router：
```bash
yarn add san san-router -D
```
src目录新建app.san文件，内容如下(其实就是官方`hello world`的内容)：
```html
<template>
    <div class="hello">hello {{msg}}</div>
</template>

<script>
    export default {
        initData () {
            return {
                msg: 'world'
            };
        }
    }
</script>

<style>
    .hello {
        color: blue;
    }
</style>
```
在入口文件中引入（也就是在~~index.js~~ **main.js**写上下面的代码）：
```js
import san from 'san';
import {router} from 'san-router';
import San from './app.san';

console.log('hello webpack  San');

//这是控制路由，引入San组件  不要问我怎么知道，看官方demo学来的。这个东西坑了我好久
router.add({rule: '/', Component: San, target: '#app'});

// 一定要记得启动
router.start()

```

### 接下来我们尝试构建一下：
```bash
#妥妥的构建失败
ERROR in ./src/app.san
Module parse failed: Unexpected token (1:0)
You may need an appropriate loader to handle this file type.

#错误信息大概就是san解析失败，没有合适的加载程序
```
**错误信息很重要，一定要学会看错误信息**，看不懂可以翻译嘛大概知道什么意思就可以了，知道缺什么了我们就安装什么，没什么大不了是吧
处理san文件的包就叫san-loader，让我们来安装它：
```bash
yarn add san-loader

#在webpack.config.js中加入配置
module: {
        rules: [
            {
                test: /\.san$/,
                use: 'san-loader'
            }
        ]
    }
```
继续尝试构建，发现又报错了：
```bash
ERROR in ./src/app.san
Module not found: Error: Can't resolve 'html-loader' in '/home/code/Desktop/test/src'
 @ ./src/app.san 9:19-133
 @ ./src/main.js

ERROR in ./src/app.san
Module not found: Error: Can't resolve 'style-loader' in '/home/code/Desktop/test/src'
 @ ./src/app.san 3:0-157
 @ ./src/main.js
```

很显然缺少`html-loader`和`style-loader`这两个东西，继续安装方法和上面一样，安装完之后构建：
```bash
#通红的报错依然刺激着我们的眼珠
ERROR in ./src/app.san
Module not found: Error: Can't resolve 'css-loader' in '/home/code/Desktop/test/src'
 @ ./src/app.san 3:0-157
 @ ./src/main.js
```
相必你应该明白该干什么了吧，装完这个之后我们终于构建成功了！！其实如果你足够细心就会发现，安装`san-loader`的时候就已经提醒你要安装剩下的三个了：
```bash
#安装san-loader时的警告信息
warning " > san-loader@0.0.7" has unmet peer dependency "style-loader@*".
warning " > san-loader@0.0.7" has unmet peer dependency "html-loader@*".
warning " > san-loader@0.0.7" has unmet peer dependency "css-loader@*".
```
所以说这个经历再一次告诉了我们，**看报错和警告很重要**

最后启动我们的项目`yarn run start`,可以看到一个蓝色的`hello world`已经出现在屏幕上，我们的第一个项目算是完成了，当你修改文件时，会自动构建并刷新浏览器。
![](http://www.xluos.com/usr/uploads/2018/04/1033217407.png)

夜已深，不知不觉写到了两点半，写到这里我又梳理了一次自己的思路，按照我所踩坑的写下了这篇文章，当然自己扣的时候远没有写下的步骤这般顺利。感谢你能看到最后，这是我自己对于一天的踩坑的梳理，也希望记下来能帮助到你。当然这只是webpack的冰山一角，但是希望我的文章能帮助到你，相信你接下来可以自己看着文档学会。

附上[GitHub](https://github.com/xluos/ife)

如果我的文章帮到了你请给我评论或点赞

如果还有什么疑问可以加我的QQ群一块交流【770474102】

同时感谢下面的朋友指出我的错误


