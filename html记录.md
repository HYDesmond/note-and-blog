# html标签
+ `<html></html>` 根标签，所有的标签都要在它之中
+ `<head></head>` 头标签，网页文档的头部存放各种属性信息
+ `<title></title>` 存放网页的标题，这个标签的内容会出现在标题栏
+ `<body></body>` 网页上显示的内容都在`<body>`标签中
+ `<p></p>` 段落标签
+ `<hx></hx>` 标题标签，x为1-6 显示效果分别从大到小
+ `<em></em>` 斜体标签
+ `<strong></strong>` 加粗标签
+ `<span></span>` 为文字设立单独样式
+ `<q></q>` 短文本引用，浏览器解析是会在标签处显示为引用符号
+ `<blokquote></blokquote>` 长文本引用，短文本引用使用引用符号括起来，长文本会以左右缩进的形式显示
+ `<br />` 换行标签  html代码中换行和空格是被忽略的，要用标签来达到换行的效果
+ `&nbsp;` html代码中的空格
+ `<hr />` 分割线，标签所在的地方浏览器会加上一条分割线
+ `<address></address>` 地址标签，在浏览器上显示为单独一行并斜体表示
+ `<code></code>` 单行代码标签，多行代码需要用`<pre>`标签
+ `<pre></pre>` 多行代码标签，可以保证代码格式不变，空格和换行不需要使用`html`标签
+ `<ul><li></li></ul>` 无序列表标签，其中`<li></li>`的数量代表列数量
+ `<ol><li></li></ol>` 有序列表标签，每一列前有数字表示序号
+ `<div></div>`   `<div>`标签相当于一个容器或者说是一个看不见的框，把网页分成一块儿块儿的方便为每一块儿设置效果，使用id属性为div命名，方便我们辨识语法 `<div id="...">...</div>`
![图片](http://static.mukewang.com/img/52d38d7b00017fb804800357.jpg)
+ `<table></table>` 表格标签，创建表格四个元素`<table>,<tbody>,<tr>,<th>,<td>` summaty摘要属性，类似于注释不显示在网页中，方便编写代码是区分表格内容
0. `<caption></caption>` 表格标题
1. `<table>…</table>`：整个表格以`<table>`标记开始、`</table>`标记结束。
2. `<tbody>…</tbody>`：如果不加`<thead><tbody><tfooter>` , table表格加载完后才显示。加上这些表格结构， tbody包含行的内容下载完优先显示，不必等待表格结束后在显示，同时如果表格很长，用tbody分段，可以一部分一部分地显示。（通俗理解table 可以按结构一块块的显示，不在等整个表格加载完后显示。）
3. `<tr>…</tr>`：表格的一行，所以有几对tr 表格就有几行。
4. `<td>…</td>`：表格的一个单元格，一行中包含几对`<td>...</td>`，说明一行中就有几列。
5. `<th>…</th>`：表格的头部的一个单元格，表格表头。
+ `<a></a>` 超链接，href属性为链接，title为鼠标放置在链接上是提示内容并且方便搜索引擎了解链接地址内容更加友好，标签内文字为显示内容，target属性设置链接跳转方式，使用mailto属性在连接中打开Email
![mailto参数](http://static.mukewang.com/img/52da4f2a000150b714280550.jpg)
![实例](http://static.mukewang.com/img/52da52200001e00e07930061.jpg)
+ `<img />`标签插入图片，语法`<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">`
+ `<form></form>`是表单标签，所有的表单控件都要放在`<form>`之间才能上传，action属性浏览者输入的数据被传送到的地方,比如一个PHP页面(save.php)。method ： 数据传送的方式（get/post）
+ `<input />`是表单中的输入框标签，type属性为text时文本输入框，为password时密码输入框。neme属性为文本框命名方便后台程序使。value为文本输入框设置默认值
+ `<input />`设置单选多选框
1. type:
+ 当 type="radio" 时，控件为单选框
+ 当 type="checkbox" 时，控件为复选框
2. value：提交数据到服务器的值（后台程序PHP使用）

3. name：为控件命名，以备后台程序 ASP、PHP 使用

4. checked：当设置 checked="checked" 时，该选项被默认选中
+ `<input />`设置提交按钮，type设置为submit是才有提交作用，value为按钮上显示的文字
+ `<input />`设置重置按钮，重置表单的默认值，type设置为reset，value为按钮上显示的文字
+ `<texttarea></texttarea>`文本域标签，cols属性输入域列数，rows输入域行数，标签之间可以输入默认值
+ `<select><option></option></select>`下拉列表控件，`<select>`设置multiple=“multiple”属性为多选。`<option>`代表选项，value属性为提交值，标签中间为显示内容，设置slected=“selected”属性为默认选项
+ `<label></label>`标签简单的来说将文字和表单的选项绑定达到点击文字同样可以选择的效果。for属性要和选项的id属性相同
#CSS样式
css 样式由选择符和声明组成，而声明又由属性和值组成，如下图所示：

![](http://static.mukewang.com/img/52fde5c30001b0fe03030117.jpg)

css中使用/**/作为注释
css在html中的使用分为内联，嵌入，外部三种
+ 内联式直接写在标签里面如果有多条样式代码用分号分开，如：`<p style="color:red;font-size:12px">这里文字是红色。</p>`
+ 嵌入式css样式必须写在`<style type="text/css"></style>`之间，type属性样式表的 MIME 类型。目前，唯一可能的值是 "text/css"。通常`<style></style>`标签写在`<head></head>`之间
+ 外部式，把css样式写在一个单独的以.css结尾的文件中，使用`<link />`标签链接到HTML文件内(该标签一般也放在`<head></head>`之间)，用法：`<link href="base.css" rel="stylesheet" type="text/css" />`,*rel="stylesheet" type="text/css" 是固定写法不可修改。*

在相同权值下，内联>嵌入>外部，就近原则

**类选择器：** 类样式的选择器可以自定义但是要以`.`开头，使用是在标签中加入属性class=“类名（不带点儿）”

**ID选择器：** 类似类选择器，只不过为标签设置id="ID名称"，而不是class="类名称"。ID选择符的前面是井号（#）号，而不是英文圆点（.）。
*两者区别，类选择器可以使用多次，id选择器只能使用一次方便使用js通过id来获取属性调用*

**子选择器（.first>span{color:red;}）与后代选择器(.first  span{color:red;})：** 子选择器作用于元素的第一个后代，后代选择器则是全部后代

**通用选择器（* {。。。}）：** 为所有标签加上样式

**伪类选择符（标签：状态{样式}）：** 可以为一个不存在的标签（比如某个标签的鼠标划过状态）设置样式

**分组选择符:** 为多个标签设置同一个样式时用`，`分割，如：`h1,span{color:red;}`

**权值：** 如果同一个代码有多个css样式那么最终显示效果会是权值最大的那个*继承的权值只有0.1（可以理解为只要有别样式就比继承的权值高），标签的权值为1，类选择符的权值为10，ID选择符的权值最高为100*，例如：
+ `p{color:red;}` /*权值为1*/
+ `p span{color:green;}` /*权值为1+1=2*/
+ `.warning{color:white;}` /*权值为10*/
+ `p span.warning{color:purple;}` /*权值为1+1+10=12*/
+ `#footer .note p{color:yellow;}` /*权值为100+10+1=111*/

如果层叠（同一元素多个相同权值的样式存在）按照顺序最后一条样式会被应用
有特殊需要时可以使用`!important`来设置为最高权值，语法：`p{color:red!important;}`  
**注意：** 要在分号前面只能把写`!important`的那条属性设置为最高权限，而不是整个样式
## 文字排版属性
+ `font-family:"...";` 设置字体
+ `font-size:npx;` 设置字号（n为任意正数）
+ `color:...;` 设置颜色，可以设置颜色的名称，16进制值，rgb（）值
+ `font-weight:bold;` 粗体
+ `font-style:italic;` 斜体
+ `text-decoration:underline;` 下划线
+ `text-decoration:line-through;` 删除线
+ `text-indent:2em;` 缩进
+ `line-height:1.5em;` 行间距
+ `letter-spacing:50px;` 字间距
+ `text-align:center/left/right;` 居中/左/右
## 元素
+ `width` 宽
+ `height` 高
+ `display:block` 设置为块级元素
+ `display:inline` 设置为内联（行）元素
+ `display:inline-block` 设置为内联块状标签
## 盒子模型
+ `border` 边框
    + `-style`样式`dashed虚线`、`dotted（点线）`、`solid（实线）`
    + `-color`颜色
    + `-width` 宽度
    + `-top`、`-bootom`、`-right`、`-left` 单独设置上下左右边框
+ `padding` 填充
    + 可以分为上右下左（顺序不要错）缩写`div{padding:20px 10px 15px 30px;}`
    + `-top`、`-bootom`、`-right`、`-left` 单独设置上下左右边框
    + 如果四周填充宽度相同可以`div{padding:10px;}`
    + 如果上下填充一样为10px，左右一样为20px，可以这么写：`div{padding:10px 20px;}`
+ `margin` 边界，用法与`padding` 相同
+ `float` 浮动模型可以让块元素浮动到同一行 `left`和`rigth`两个属性
+ `position: absolute;` 绝对定位，相对于最接近的一个具有定位属性的父包含块进行绝对定位，如果不存在父元素则相对于浏览器窗口
+ `position:relative` 相对定位，元素的实际位置并没有改变，只是显示位置相对于原本在流状显示发生的移动
+ `position:fixed` 固定定位，将元素固定与相对于网页显示大小的位置除非改变网页大小或浏览器位置才会移动位置（常见的右下角小广告）
    + 使用`left`、`right`、`top`、`bottom`属性设置偏移量

