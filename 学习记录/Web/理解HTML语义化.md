# 什么是HTML语义化
说HTML语义化就要先说说HTML到底负责的什么？下面摘自维基百科：
> 超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。
> HTML元素是构建网站的基石。HTML允许嵌入图像与对象，并且可以用于创建交互式表单，它被用来结构化信息——例如标题、段落和列表等等，也可用来在一定程度上描述文档的外观和语义。
关于对于语义化的理解[顾轶灵：如何理解 Web 语义化？](https://www.zhihu.com/question/20455165)这里讲的很清楚了我就简单说一下我的理解：
通俗的来讲就是从代码上来展示页面的结构，而不是从最终视觉上来展示结构。单纯的HTML代码是**不带任何样式的**只是用来标记这一段是标题、这一块是代码、那一个是要强调的内容等等，但是为什么我们只写HTML在浏览器中不同的标签也是有不同的样式呢？那是因为各个浏览器都自带的有相应标签的默认样式，为了方便在没有设定样式的情况下友好的展示页面。
良好的语义化代码可以直接从代码上就能看出来那一块到底是要表达什么内容。
# 为什么要使用HTML语义化标签
为什么要使用语义化标签？我用DIV+CSS也能做出来一样的效果，确实单纯看效果两者并没有什么区别，但是页面不止是给人看的，机器也要看爬虫也要看。
网页结构更清晰方便开发维护：

![对比图2](http://www.5icool.org/uploadfile/2010/0613/20100613113000239.jpg)
![对比图2](http://www.5icool.org/uploadfile/2010/0613/20100613113000852.jpg)

另外，在网络或其他原因页面样式文件丢失的时候，满是div组成的页面和良好语义结构组成的页面那个对用户更友好？
## 优点
+ 标签语义化有助于构架良好的HTML结构，有利于搜索引擎的建立索引、抓取。简单来说，试想在H1标签中匹配到的关键词和在div中匹配到的关键词搜索引擎会吧那个结果放在前面。
+ 有利于不同设备的解析（屏幕阅读器，盲人阅读器等）满是div的页面这些设备如何区分那些是主要内容优先阅读？
+ 有利于构建清晰的机构，有利于团队的开发、维护。

## 大厂都是怎么做的？
看一下大厂的操作，打开淘宝的页面查看它首页的源码发现，**全局只有一个h1**标签就是他的LOGO。

![](http://www.xluos.com/usr/uploads/2018/04/296625203.png)

再往下看主题分栏的标题是h2

![](http://www.xluos.com/usr/uploads/2018/04/871789002.png)
再往下看，分栏都是用的h3标签，并且内部使用了一个隐藏的`<em></em>`专门强调处理。
![](http://www.xluos.com/usr/uploads/2018/04/3873154428.png)

尽管这些东西都是用div+css就能搞出来的，但是它还是专门使用了相应的语义化标签来强调作用。

# 写语义化代码应该注意什么
+ 尽可能少的使用无语义的标签div和span；在语义不明显时，既可以使用div或者p时，尽量用p, 因为p在默认情况下有上下间距，对兼容特殊终端有利；
+ 不要使用纯样式标签，如：b、font、u等，改用css设置。需要强调的文本，可以包含在strong或者em标签中（浏览器预设样式，能用CSS指定就不用他们），strong默认样式是加粗（不要用b），em是斜体（不用i）；
+ 使用表格时，标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头用th，单元格用td；
+ 表单域要用fieldset标签包起来，并用legend标签说明表单的用途；每个input标签对应的说明文本都需要使用label标签，并且通过为input设置id属性，在lable标签中设置for=someld来让说明文本和相对应的input关联起来。

# 常用的一些语义化标签
+ `<h1>`~`<h6>` ，作为标题使用，并且依据重要性递减，`<h1>` 是最高的等级。
+ `<p>`段落标记，知道了 `<p>` 作为段落，你就不会再使用 `<br />` 来换行了，而且不需要 `<br />` 来区分段落与段落。`<p>` 中的文字会自动换行，而且换行的效果优于 `<br />`。段落与段落之间的空隙也可以利用 CSS 来控制，很容易而且清晰的区分出段落与段落。
+ `<ul>`、`<ol>`、`<li>`，`<ul>` 无序列表，这个被大家广泛的使用，`<ol>` 有序列表不常用。在 Web 标准化过程中，`<ul>` 还被更多的用于导航条，本来导航条就是个列表，这样做是完全正确的，而且当你的浏览器不支持 CSS 的时候，导航链接仍然很好使，只是美观方面差了一点而已。
+ `<dl>`、`<dt>`、`<dd>`，`<dl>` 就是“定义列表”。比如说词典里面的词的解释、定义就可以用这种列表。**dl不单独使用，它通常与dt和dd一起使用。dl开启一个定义列表，dt表示要定义的项目名称，dd表示对dt的项目的描述。**
+ `<em>`、`<strong>`，`<em>` 是用作强调，`<strong>` 是用作重点强调。
+ `<table>`、`<thead>`、`<tbody>`、`<td>`、`<th>`、`<caption>`， 就是用来做表格不要用来布局



# HTML5新增的那些
HTML5标准更加的讲究语义化了，借用一张网上的图来说明这些新标签

![H5新增布局标签](http://www.html5jscss.com/pic/htmljscss/html5-layout.jpg)

+ `header元素`：header 元素代表“网页”或“section”的页眉。
+ `footer元素`：footer元素代表“网页”或“section”的页脚，通常含有该节的一些基本信息，譬如：作者，相关文档链接，版权资料。
+ `hgroup元素`：
+ `nav元素`：nav元素代表页面的导航链接区域。用于定义页面的主要导航部分。
+ `aside元素`：aside元素被包含在article元素中作为主要内容的附属信息部分，其中的内容可以是与当前文章有关的相关资料、标签、名次解释等。（特殊的section）
+ `section元素`：section元素代表文档中的“节”或“段”，“段”可以是指一篇文章里按照主题的分段；“节”可以是指一个页面里的分组。section通常还带标题，虽然html5中section会自动给标题h1-h6降级，但是最好手动给他们降级。
+ `article元素`：article元素最容易跟section和div容易混淆，其实article代表一个在文档，页面或者网站中自成一体的内容，其目的是为了让开发者独立开发或重用。譬如论坛的帖子，博客上的文章，一篇用户的评论，一个互动的widget小工具。（特殊的section）除了它的内容，article会有一个标题（通常会在header里），会有一个footer页脚。

推荐看原博主的博客：[传送门](http://www.html5jscss.com/html5-semantics-section.html)

# 参考链接
+ [https://www.zhihu.com/question/20583492/answer/260535796](https://www.zhihu.com/question/20583492/answer/260535796)
+ [https://www.zhihu.com/question/20455165](https://www.zhihu.com/question/20455165)
+ [https://www.zhihu.com/question/20583492](https://www.zhihu.com/question/20583492)
+ [https://www.cnblogs.com/freeyiyi1993/p/3615179.html](https://www.cnblogs.com/freeyiyi1993/p/3615179.html)
+ [http://www.html5jscss.com/html5-semantics-section.html](http://www.html5jscss.com/html5-semantics-section.html)
+ [https://segmentfault.com/a/1190000004179484](https://segmentfault.com/a/1190000004179484)
