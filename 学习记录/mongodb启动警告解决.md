# mongodb启动警告解决

安装没啥好说的`sudo apt install mongodb`

但是启动的时候总是有两句警告看着挺烦的（不要在意下面的时间，我的已经弄好了这是在网上找的警告信息）：
```
2015-03-22T09:27:00.222+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2015-03-22T09:27:00.222+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
```
修改`/sys/kernel/mm/transparent_hugepage/defrag`设置后重启mongo没有报错了，然而系统重启后这个设置又会改回来。

**偶然发现还有另一个办法解决这个警告,就是启用mongo的身份验证这个报错就消失,具体原理还不清楚**

搜索了一通后找到了解决的办法，把修改的命令加入mongo的配置文件里面，这样启动mongo的时候就会修改过来，这样就没有烦人的警告了。方法如下：
找到`/etc/init/mongod.conf`文件，然后在`script和end script`之间插入下面的代码
```
if test -f /sys/kernel/mm/transparent_hugepage/enabled; then  
   echo never > /sys/kernel/mm/transparent_hugepage/enabled  
fi  
```

最后重启mongo服务，搞定
```
sudo service mongod restart
```