# avd模拟器提示Package install error: Failure [INSTALL_FAILED_DEXOPT]
### android studio建立模拟器,之前运行好好的,做了几个测试程序后再运行提示Package install error: Failure [INSTALL_FAILED_DEXOPT],仔细检查代码没有发现问题,查询模拟器设置发现配置确实不高，解决办法有两个:
#### 1 在模拟器最右边Actions栏选择下拉菜单-Wipe Data,清除用户数据,重启模拟器后正常
#### 2 在模拟器最右边Actions栏选择下拉菜单-Cold Boot Now,也是清除了数据。
我们每次在模拟器上的poweroff或者restart，都是热启动，选择[]按钮发现以前的程序还都在后台运行呢，所以需要清理一下。
