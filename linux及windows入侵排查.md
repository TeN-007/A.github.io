## linux木马排查实战

背景：

- 某台安全设备检测到了客户端的某台主机，发现了大量的请求外发，而且是恶意的域名，怀疑是主机中毒了

### 前期摸索阶段

![image-20210220220411496](D:\安全\wp\wp截图\image-20210220220411496.png)

- top命令查看当前进程，发现4473占用CPU较多。不是一输入top就会出现，可能要等一会![image-20210220211125501](D:\安全\wp\wp截图\image-20210220211125501.png)
- 使用ps命令来查看进程状态`ps -ef|grep 4473`，发现没法看到其进程，无法看到其对外的连接状态
- 下意识的使用`kill -9 4473`来杀死该进程。再使用top命令查看发现又出现了一个随机数的进程
- 使用pstree，在下面发现了随机数的进程，2个进程在运行![image-20210220211555561](D:\安全\wp\wp截图\image-20210220211555561.png)
- linux在创建一个进程的时候会在/proc下创建一个以pid命名的文件夹，文件夹里有该进程的详细信息。所以使用命令`ll /proc/130286`来查看进程的详细信息。![image-20210220211907100](D:\安全\wp\wp截图\image-20210220211907100.png)
- 发现原文件在`/boot/zrgopiucca`使用命令`scp  /boot/zrgopiucca /tmp/zrg`将文件拷出来。使用命令`sz zrg`来将文件下载到本机(在Linux和Windows之间传输文件)
- 将下载的文件上传到X情报社区去检测`https://x.threatbook.cn/`发现检测出来是一个恶意文件![image-20210220212705901](D:\安全\wp\wp截图\image-20210220212705901.png)
- 然后进入boot目录使用`rm -rf 文件名`来删除文件。然后再使用`kill -9 pid`来删除进程。
- top查看，还是有。可能还存在一个定时任务
- 命令`cat /etc/crontab`![image-20210220214022385](D:\安全\wp\wp截图\image-20210220214022385.png)
- 命令`cat /etc/cron.hourly/cron.sh`查看定时任务写的什么![image-20210220214142991](D:\安全\wp\wp截图\image-20210220214142991.png)
- 发现修改了环境变量，并且发现了/lib/udev/udev是木马。在定时任务中，所以最简单的方法就是修改定时任务`rm -rf /etc/cron.hourly/cron.sh`
- 再 修改文件`vi /etc/crontab`，清除最后一行任务![image-20210220214416545](D:\安全\wp\wp截图\image-20210220214416545.png)
- 再使用`chattr +i /etc/crontab`设置计划任务无法写入
- 再删除上面2个木马文件
- 判断开机启动项可能也有问题，查看开机启动项`ls /etc/rc.d/init.d`发现了启动项里有之前找到的木马文件
- 清除恶意链接和非法开机启动项：还需要将这些取消开机启动项`rm -rf 名称`。再切换到另外的目录下`cd /etc/rc0.d`  `rm -rf K90* S90*`![image-20210220215620916](D:\安全\wp\wp截图\image-20210220215620916.png)
- 然后再top检查，自查`cd ~;chkconfig --list | grep on` 是否有异常，`cat /etc/passwd`是否有乱七八糟的用户，`cat /etc/shadow`，查看用户最后一次登录时间`lastlog`,`last`查看最近登录的日志

## windows木马排查

### 常用工具：

- Autoruns
- TCPview
- Process Explorer
- PCHunter
- D盾
- 火绒剑