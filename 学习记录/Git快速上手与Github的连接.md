# Git基础与Github的连接
## 基础部分
### 安装与配置
安装：`apt install git ` 一句命令搞定

配置个人用户信息与电子邮件
```c
$ git config --global user.name "xluos"             //提交用户名
$ git config --global user.email email@xluos.com    //如果要连接github这里要和github账户邮箱一致
// --global 选项设置为所有项目  如果不加就是当前项目或用户
```
`git config --list` 查看所有配置信息
### 基本使用
**初始化：**`git init` 在需要创建为版本库的目录下使用命令把该目录创建为版本库

**添加到索引库（缓存区）：** `git add [文件名]` 文件名支持通配符
+ `git add -A`添加所有改变、新建、删除的文件
+ `git add .` 添加所有跟踪过（曾经添加过索引）的文件

**移除出缓存区：**
**提交修改到本地版本库：** `git commit -m ‘提交说明（必须写）’` 
+ `-a` 选项直接提交之前add过的文件改动

**克隆仓库：** `git clone [仓库地址] [克隆目录]` 目录为空的话默认当前目录，`github`中仓库地址在这里：
![地址](http://www.xluos.com/usr/uploads/2017/09/2666597849.png)

**查看项目状态：** `git status -s`

**查看提交历史：** `git log [参数]`
+ `--oneline` 查看简洁版本
+ `--graph` 查看分支情况
+ `--reverse` 逆向查看
+ `--author=用户名` 查看特定用户提交
### 分支管理
**列出/创建/删除分支：** `git branch [参数] [分支名]` 没有参数时，列出现有分支
+ 都为空 列出分支
+ 参数为空 创建分支
+ `-d` 删除分支

**切换分支：** `git checkout [参数] [分支名]` 
+ `-b` 创建并立即切换到新分支
> 当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

**合并分支：** `git merge [分支名]` 将分支合并到主分支中
+ 合并冲突，当两个分支有相同文件名不同内容的文件时，合并就会冲突。这个时候就要我们手动修改冲突后，使用`git add`告诉Git冲突解决

**标签：** `git tag [参数] [标签名]`标签说白了就是给某一次提交起一个名字方便我们切换找到这次提交，比如软件到了1.0 2.0版本的时候就应该创建一个标签方便有需要的切换到相应的版本，不然在大量的comment中找到一个特定版本太难记忆了。
+ `-a` 标签加上注释，如什么时间谁打的
+ 追加标签，对于之前一次提交在标签名后面放上需要标签comment号（log简洁查看时前面的编号）
+ `-d` 删除标签
+ `git show [标签]` 查看该标签的版本改动

## 连接Github
1. 注册Github账号
2. 生成`ssh-key`，命令：`ssh-keygen -t rsa -C "你注册Github的邮箱"` 然后出来的选项都回车就好，然后复制`～/.ssh/id_rsa.pub`内的全部内容
3. 打开Github网站依次如下图打开（图片来自菜鸟教程）：
![](http://www.runoob.com/wp-content/uploads/2015/03/48840BF0-992F-4CCC-A388-15CB74819D88.jpg)
![](http://www.runoob.com/wp-content/uploads/2015/03/106AD534-A38A-47F3-88A3-B7BE3F2FEEF1.jpg)
复制的内容粘贴到`key`中，`Title`随意，然后`Add SSH Key`就好，接下来输入`ssh -T git@github.com`查看是否验证成功。
4. 在网页端创建版本库
5. 可以直接`git clone`我们创建的版本库，或者在本地的与版本库同名文件夹中`git init`初始化一个版本库
### 上传到Github
+ **给远程库起个名字：** 方便提交，`git remote add 你起的名字 git@github.com:你的Github账户名/要上传的版本库.git `
+ **上传：** `git push -u 版本库名称 master（如要上传到其他分支用其他分支名）`
+ **更新本地：** `git fetch`或`git pull`不同的是`fetch`取回不合并，`pull`取回直接合并
    + 语法：`git fetch <远程主机名> <分支名>`，不写分支取回所有分支
    + 语法：`git pull <远程主机名> <远程分支名>:<本地分支名>` 不写本地分支默认合并当前分支
+ **远程版本库操作：** `git remote [参数]`
    + `-v`显示详细信息
    + 无参数列出所有版本库
    + `add` 添加远程库
    + `rm` 删除远程库
    + `rename 原名 新名` 远程库改名
> 之所以标题写快速上手，因为只是简单记录用法，看完可以做到基本操作但是更详细的没有写可以看网上其他更详细的教程，如：
> + [Git官方文档](https://git-scm.com/docs)
> + [廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
> + [菜鸟教程Git](http://www.runoob.com/git/git-tutorial.html)
> + [阮一峰blog](http://www.ruanyifeng.com/blog/2014/06/git_remote.html) 