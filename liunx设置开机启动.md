将脚本放到`／etc/init.d`下
使用`update-rc.d`命令为建立在`/etc/rc[0-6].d`软连接
```
//添加这个服务并让它开机自动执行            
update-rc.d apache2 defaults
//并且可以指定该服务的启动顺序：
update-rc.d apache2 defaults 90
//还可以更详细的控制start与kill顺序：
update-rc.d apache2 defaults 20 80
//其中前面的20是start时的运行顺序级别，80为kill时的级别。也可以写成：
update-rc.d apache2 start 20 2 3 4 5 . stop 80 0 1 6 .
//其中0～6为运行级别。
//删除一个服务
update-rc.d -f apache2 remove
```