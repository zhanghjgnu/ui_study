## flutter开发环境的安装配置

前提条件:需要安装jdk8和android studio.
### 1 安装jdk8,注意不能只安装JRE，应该JDK和JRE都安装,然后设置JAVA_HOME和增加PATH
   我的电脑-高级环境变量 JAVA_HOME=C:\Program Files\Java\jdk1.8.0_201\bin;

### 2.安装android studio
到https://developer.android.com/studio下载和操作系统对应的版本，点击文件安装,安装目录选择D:\Program Files\Android,顺便安装SDK和AVD，SDK目录选择D:\AndroidSdk，经过比较长的时间下载安装完成。
安装之后可以用java和kotlin开发android应用了

### 3.安装fultter SDK
从https://flutter.dev/docs/get-started/install/windows下载zip文件，解压到D:\src\,然后更新环境变量
PATH=D:\src\flutter\bin;

### 4.在android stdio中安装dart和flutter plugin

### 5. 验证flutter安装
运行powershell,然后
fultter docker
提示找不到android studio和找不到设备，在android studio下建立flutter项目运行也提示flutter run: No connected devices。

网上搜一通，发现两个有用的链接:https://github.com/flutter/flutter-intellij/issues/2084
https://stackoverflow.com/questions/44485848/android-sdk-cannot-be-found-by-flutter/51644461#51644461

linux环境下
export ANDROID_HOME=/path/to/Android/Sdk
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/tools       <--- I was missing this one

export PATH=$PATH:/path/to/flutter/bin

我的windows环境增加了ANDROID_HOME=D:\AndroidSdk问题就解决了，据说如果把androidSDK和androidstudio安装在缺省的环境下就没这个问题。

再次在powershell下运行
flutter doctor
flutter emulators 
提示找到了android studio和设备，可以开发app了。
