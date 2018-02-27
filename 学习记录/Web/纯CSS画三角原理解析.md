#纯CSS画三角原理解析
因为之前做一个页面出现了很多三角，开始直接用图片感觉并不是很好用，看着总是怪怪的颜色还很难调整的跟背景一样，就搜了一波代码直接用上了，事后想了一下感觉不知道具体原理是什么，很奇怪为什么**边框**能设置成三角的样式。于是搜了一波原理整理如下

# 1.边框到底是什么样的？
因为很少用到很粗的边框，而且90%的情况下我们用边框都是一个颜色的。所以我发现我并不知道边框到底是什么样子的，一直一来我都以为四条边都是一条线（不要告诉我就我一个人这样认为）。

实验了一下才发现边框越来越粗的时候，很明显每条边都是一个**梯形**

![](http://www.xluos.com/usr/uploads/2018/02/3407089098.png)

# 2.如何做出三角？
因为之前看的代码都会写上`width: 0; height: 0;`当时不理解为什么，现在很容易就能想到，用极限的思维，当内容大小**趋近与零**的时候,每个边就是一个三角了。

![](http://www.xluos.com/usr/uploads/2018/02/2870997276.png)

这个时候就可以看到三角已经出现，我们要做的就是把其他边设置为透明的就可以得到我们需要的三角了。

# 3.还有没有更多的情况？
通过分别取消边框发现下面几种情况：
+ 取消一条边的时候，与这条边相邻的两条边的接触部分会变成直的
+ 当仅有邻边时， 两个边会变成对分的三角
+ 当保留边没有其他接触时，极限情况所有东西都会消失。

![图像 022.png](http://www.xluos.com/usr/uploads/2018/02/3111040956.png)
![图像 023.png](http://www.xluos.com/usr/uploads/2018/02/2859901719.png)
![图像 024.png](http://www.xluos.com/usr/uploads/2018/02/3678721391.png)
![图像 025.png](http://www.xluos.com/usr/uploads/2018/02/3384240413.png)
![图像 026.png](http://www.xluos.com/usr/uploads/2018/02/2146785425.png)
![图像 027.png](http://www.xluos.com/usr/uploads/2018/02/3941884968.png)

# 4.拓展
明白了这些之后，再看代码是不是感觉就很清晰了呢？然后我们就可以做出更多形状的三角。有了这些形状再加上旋转属性，基本所有的场景都能使用。
## 直角三角
![图像 028.png](http://www.xluos.com/usr/uploads/2018/02/2191440879.png)
```css
.box {
    /* 内部大小 */
    width: 0px;
    height: 0px;
    /* 边框大小 只设置三条边*/
    border-top: #4285f4 solid;
    border-right: transparent solid;
    border-left: transparent solid;
    border-width: 85px; 
    /* 其他设置 可有可无*/
    margin: 50px;
}
```
## 更小的直角三角形
利用对边的情况，做出更小的直角三角形
![图像 029.png](http://www.xluos.com/usr/uploads/2018/02/2126461688.png)
```css
.box {
    /* 内部大小 */
    width: 0px;
    height: 0px;
    /* 边框大小 只设置两条边*/
    border-top: #4285f4 solid;
    border-right: transparent solid;
    border-width: 85px; 
    /* 其他设置 */
    margin: 50px;
}
```
## 等腰锐角三角形
通过更改底边的长度可以做出各种不同的三角形
![图像 030.png](http://www.xluos.com/usr/uploads/2018/02/2041877816.png)
```CSS
.box {
    /* 内部大小 */
    width: 0px;
    height: 0px;
    /* 边框大小 */
    border-top: #4285f4 170px solid;
    border-right: transparent 85px solid;
    border-left: transparent 85px solid;
     
    /* 其他设置 */
    margin: 50px;
}
```
## 对话气泡
把伪元素设置成三角，再通过定位确定位置，就可以制作出来经典的对话气泡了。

![气泡](http://www.xluos.com/usr/uploads/2018/02/1461721219.png)
```css
.bubbly {
    position: absolute;
    top: 30%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: #00ccbb;
    border-radius: 8px;
    width: 200px;
    padding: 40px 10px;
    text-align: center;
    color: white;
    font-size: 20px;

.bubbly:after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    border: 34px solid transparent;
    border-top-color: #00ccbb;
    border-bottom: 0;
    border-left: 0;
    margin: 0 0 -34px -17px;
}
```
# 总结
通过对四条边的长度的设置，还可以做出各种各样的三角形，几乎所有三角的形状都可以设置出来。
另外还可以通过设置**宽高中的一项为0另一个不为0**，来设置出体形的形状，大家可以自由实验

