
1.首先应该安装gui,我安装时就装好了,然后选择图形模式启动
  systemctl set-default graphical.target
2.安装服务端
  # yum -y install tigervnc tigervnc-server tigervnc-server-module
  # yum update
3.复制配置模板文件为vncserver@:1.service
  如果需要启动多个vnc窗口就复制多个文件，名称可以为vncserver@:2.service、vncserver@:3.service,然后启动多个服务，不过服务器上这么占用资源的事还是不要做了。
   cp /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
4.修改vncserver@:1.service配置文件

# cat /lib/systemd/system/vncserver\@\:1.service | grep -v ^# | grep -v ^$  
[Unit]  
Description=Remote desktop service (VNC)  
After=syslog.target network.target  
[Service]  
Type=forking  
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'  
ExecStart=/usr/sbin/runuser -l root -c "/usr/bin/vncserver %i"  
PIDFile=/root/.vnc/%H%i.pid  
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'  
[Install]  
WantedBy=multi-user.target

#替换文件中的两行，其实就是将<user>换成root
#注意：这里我配置的是服务器，这项写root@
#如果是pc，直接写自己的用户名即可

普通用户的ExecStart不同于root，加/sbin/runuser则会在启动服务时报以下错误
Job for vncserver@:2.service failed because the control process exited with error code. See "systemctl status vncserver@:2.service" and "journalctl -xe" for details.
普通用户配置如下
# vim /etc/systemd/system/vncserver@\:.service
[Service]
Type=forking
User=oracle
# Clean any existing files in /tmp/.X11-unix environment
ExecStartPre=-/usr/bin/vncserver -kill %i
ExecStart=/usr/bin/vncserver %i
PIDFile=/home/oracle/.vnc/%H%i.pid
ExecStop=-/usr/bin/vncserver -kill %i

5.添加防火墙规则，第一个服务窗口采用的是5901
vim /etc/sysconfig/iptables
#在打开的文件（一般为空），加入如下语句，当然如果需要更多的端口，可以加5903，5904...
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 5901 -j ACCEPT
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 5902 -j ACCEPT
 

firewall-cmd --permanent --add-service vnc-server
systemctl restart firewalld.service

6.启动vncserver@:1.service服务，并设置开机自启
systemctl start vncserver@:1.service 
systemctl enable vncserver@:1.service
另一种启动服务
启动一个窗口，如1号窗口
# vncserver :1

停止服务
[root@iZ23zjkwlj8Z ~]# vncserver -kill :1

7.设置登陆密码：vncpasswd
这个密码不同于linux操作系统的登录密码,必须在当前用户环境下

8. 启动状态查看：systemctl status vncserver@:1.service

 9.查看端口状态：netstat -lnt | grep 590*

 10.查看报错信息或日志：grep vnc /var/log/messages

11.安装客户端
我的客户端是win10,到https://www.realvnc.com/en/connect/download/viewer/下载VNC-Viewer-6.20.113-Windows-64bit.exe,打开后在输入栏输入192.168.100.250:5901


12.问题解决
(1).报错：Job for vncserver@:1.service failed because the control process exited with error code. See "systemctl status vncserver@:1.service" and "journalctl -xe" for details.
处理方法：
rm -rf /tmp/.X11-unix/*  
（2）报错:Job for vncserver@:1.service failed.See 'systemctl status vncserver@:1.service' and 'journ alclt -xn' for details
处理方法:
把vncserver@:1.service中的Type改为simple，重启服务试试。
