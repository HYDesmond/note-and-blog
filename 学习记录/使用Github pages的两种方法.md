# 使用Github pages的两种方法
首先何为Github pages？简单的说就是
GitHub免费提供的一个静态的网站托管服务。
只能用来存放静态内容，可以用来设计个人，组织，或GitHub仓库项目页面。
如果个人没有服务器的话，使用pages来展示我们的项目介绍，个人主页，甚至是blog都是很好的选择。
但是pages不支持服务器端代码，如PHP，Ruby，Python。
官方解释：
>What is GitHub Pages?
>GitHub Pages is a static site hosting service.
>GitHub Pages is designed to host your personal, organization, or project pages directly from a GitHub repository. To learn more about the different types of GitHub Pages sites, see "User, organization, and project pages."
>You can create and publish GitHub Pages online using theJekyll Theme Chooser. If you prefer to work locally, you can useGitHub Desktopor thecommand line.
>GitHub Pages is a static site hosting service and doesn't support server-side code such as, PHP, Ruby, or Python.

其实官网已经给出了很详细的教程了，英文好的同学可以去看官网的教程

https://pages.github.com/

https://help.github.com/articles/what-is-github-pages/

## 使用pages时域名产生规则
知道规则的话，产生的域名也是很好记的
使用下面第一种方法创建的pages域名为：

`Github用户名.github.io`

第二种则是：

`Github用户名.github.io/项目名`

当然也可以自己注册一个喜欢的域名使用**CNAME记录解析**到pages产生的域名上

## 如何使用GitHub pages
有两种使用pages的方法，第一种适合作为个人或组织主页，第二种适合作为仓库主页或介绍使用

### 第一种
创建一个新的仓库并命名为`GitHub帐号.github.io`,比如我的GitHub名为abc，我就新建一个名字为`abc.github.io`的仓库。
然后将主页文件上传的这个仓库，然后提交到GitHub，稍等片刻就可以访问了。

## 第二种
如果我们要为每一个项目添加一个介绍这个项目的页面或者根域名我已经用来做我的blog使用了。用第一种方法就很不方便了，这时候第二个方法是比较合适，也是比较常用的，用法也很简单。

默认我们已经有了一个仓库，然后我们在这个仓库创建一个`gh-pages`分支

命令 `git branch gh-pages`

切换到该分支 `git checkout gh-pages`

然后将页面文件存放到这个分支里面，上传GitHub中就可以访问了