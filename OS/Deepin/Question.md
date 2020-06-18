## Deepin外接显示器不显示的问题

试试干净系统下执行 

先 xrandr --listproviders 看下有几个provider，如果有多个，那么可能是不同显示口在不同显卡上，

运行 xrandr --setprovideroutputsource 0 1 

或 xrandr --setprovideroutputsource 1 0 做下链接，就可以看到所有硬件接口，

然后 xrandr --output xxxxx --auto 设置输出



## 开机脚本

/etc/profile.d 目录下放入自定义的xxx.sh文件







