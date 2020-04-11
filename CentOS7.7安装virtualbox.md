CentOS7.7安装virtualbox

虚拟机可以安装kvm,性能更好一些，不过管理虚拟机不如virtualbox直观,还是用virtualbox顺手
1. 安装
去https://www.virtualbox.org/wiki/Downloads下载，得到的是rpm文件,上传到服务器上
另外把Oracle_VM_VirtualBox_Extension_Pack-6.1.4.vbox-extpack也下载下拉传到服务器上
```
# rpm -ivh VirtualBox-6.1-6.1.4_136177_el7-1.x86_64.rpm
提示如下:
This system is currently not set up to build kernel modules.
Please install the gcc make perl packages from your distribution.
Please install the Linux kernel "header" files matching the current kernel
for adding new hardware support to the system.
The distribution packages containing the headers are probably:
    kernel-uek-devel kernel-uek-devel-4.14.35-1902.300.11.el7uek.x86_64
This system is currently not set up to build kernel modules.
Please install the gcc make perl packages from your distribution.
Please install the Linux kernel "header" files matching the current kernel
for adding new hardware support to the system.
The distribution packages containing the headers are probably:
    kernel-uek-devel kernel-uek-devel-4.14.35-1902.300.11.el7uek.x86_64

There were problems setting up VirtualBox.  To re-start the set-up process, run
  /sbin/vboxconfig
as root.  If your system is using EFI Secure Boot you may need to sign the
kernel modules (vboxdrv, vboxnetflt, vboxnetadp, vboxpci) before you can load
them. Please see your Linux system's documentation for more information.
```
看来缺了一些依赖包，按提示安装
```
# yum install gcc make perl 
# yum install  kernel-uek-devel kernel-uek-devel-4.14.35-1902.300.11.el7uek.x86_64
```
在图形模式下进入终端运行/sbin/vboxconfig
```
#/sbin/vboxconfig
vboxdrv.sh: Stopping VirtualBox services.
vboxdrv.sh: Starting VirtualBox services.
vboxdrv.sh: Building VirtualBox kernel modules.
这回提示正常了。
```
2. 创建虚拟机
root登录进入图形模式,菜单Applications-System Tools-Oracle VM VirtualBox,启动了virtualbox，选择machine-new，建立一台虚拟机，我选择安装一个win10x64系统，提前把ISO传上去，网络选择桥接，其余配置自己定义了。安装好win10就可以正常使用了。

3. 开启win10远程桌面
  这个没啥可以写的,自行百度吧
