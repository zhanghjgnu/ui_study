以前github一直用得好好的，今天在powershell下更新flutter出现错误，提示:
 flutter upgrade
Upgrading Flutter from D:\src\flutter...
fatal: unable to access 'https://github.com/flutter/flutter.git/': Failed to connect to github.com port 443: Timed out
试了一下网页访问还好好的，在powershell下ping github.com提示请求超时,肯定是自己的问题了。
最近主要是更新了代理的版本，进入「网络和 Internet」设置的代理设置界面，看见「使用设置脚本」是打开的，并且有一个脚本地址，在浏览器中访问脚本地址会自动下载名为「pac」的文件，文件内容是需要进行代理的域名配置和使用代理的逻辑，搜索一下可以很多的找到「github.com」也位列其中，那问题应该就是代理导致的了。
想解决这个问题，在 「pac」文件中找到 「proxy」的值，将其设置为 git 的 http.proxy 的值就行，设置参数可以通过下列两个 git 命令完成。


为全局的 git 项目都设置代理


git config --global http.proxy 127.0.0.1:1080 
git config --global https.proxy http://127.0.0.1:1080

或者为某个 git 项目单独设置代理
git config --local http.proxy 127.0.0.1:1080 
git config --local https.proxy http://127.0.0.1:1080

再次在pwoershell下运行 
flutter upgrade
成功，下面是运行结果(关掉360，否则会拦截更新)
PS C:\Users\Administrator> flutter upgrade
Upgrading Flutter from D:\src\flutter...
From https://github.com/flutter/flutter
 * [new branch]          DaveShuckerow-patch-1 -> origin/DaveShuckerow-patch-1
 + 27321ebba...659dc8129 beta                  -> origin/beta  (forced update)
   09126abb2..ec1044a87  dev                   -> origin/dev
 * [new branch]          fix-widget-controller-doc -> origin/fix-widget-controller-doc
   36e599eb5..d189b94ba  master                -> origin/master
 * [new branch]          revert-45126-gallery_enable_platform_views -> origin/revert-45126-gallery_enable_platform_views
 * [new branch]          revert-45951-driver   -> origin/revert-45951-driver
 * [new branch]          revert-46090-gallery_density -> origin/revert-46090-gallery_density
 * [new branch]          revert-47027-fix-sliver-layout-assert -> origin/revert-47027-fix-sliver-layout-assert
 * [new branch]          revert-47177-setEditingState -> origin/revert-47177-setEditingState
 * [new branch]          revert-48532-integ_test_embedv2 -> origin/revert-48532-integ_test_embedv2
 * [new branch]          sjindel.aar           -> origin/sjindel.aar
 * [new branch]          slider-text-field     -> origin/slider-text-field
 * [new branch]          time-text-scale       -> origin/time-text-scale
 * [new tag]             v1.13.1               -> v1.13.1
 * [new tag]             v1.13.6               -> v1.13.6
 * [new tag]             v1.14.2               -> v1.14.2
 * [new tag]             v1.13.2               -> v1.13.2
 * [new tag]             v1.13.3               -> v1.13.3
 * [new tag]             v1.13.4               -> v1.13.4
 * [new tag]             v1.13.5               -> v1.13.5
 * [new tag]             v1.13.7               -> v1.13.7
 * [new tag]             v1.13.8               -> v1.13.8
 * [new tag]             v1.13.9               -> v1.13.9
 * [new tag]             v1.14.0               -> v1.14.0
 * [new tag]             v1.14.1               -> v1.14.1
Updating cf37c2cd0..27321ebba
 24 files changed, 651 insertions(+), 218 deletions(-)
Checking Dart SDK version...
Downloading Dart SDK from Flutter engine 2994f7e1e682039464cb25e31a78b86a3c59b695...
Unzipping Dart SDK...
Building flutter tool...
Running pub upgrade...

Upgrading engine...
Downloading Android Maven dependencies...                          42.7s

Flutter 1.12.13+hotfix.5 ? channel stable ? https://github.com/flutter/flutter.git
Framework ? revision 27321ebbad (6 weeks ago) ? 2019-12-10 18:15:01 -0800
Engine ? revision 2994f7e1e6
Tools ? Dart 2.7.0

Running flutter doctor...
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, v1.12.13+hotfix.5, on Microsoft Windows [Version 10.0.16299.726], locale zh-CN)
[!] Android toolchain - develop for Android devices (Android SDK version 29.0.2)
    ! Some Android licenses not accepted.  To resolve this, run: flutter doctor --android-licenses
[√] Android Studio (version 3.5)
[!] Connected device
    ! No devices available

! Doctor found issues in 2 categories.

  ╔════════════════════════════════════════════════════════════════════════════╗
  ║                 Welcome to Flutter! - https://flutter.dev                  ║
  ║                                                                            ║
  ║ The Flutter tool uses Google Analytics to anonymously report feature usage ║
  ║ statistics and basic crash reports. This data is used to help improve      ║
  ║ Flutter tools over time.                                                   ║
  ║                                                                            ║
  ║ Flutter tool analytics are not sent on the very first run. To disable      ║
  ║ reporting, type 'flutter config --no-analytics'. To display the current    ║
  ║ setting, type 'flutter config'. If you opt out of analytics, an opt-out    ║
  ║ event will be sent, and then no further information will be sent by the    ║
  ║ Flutter tool.                                                              ║
  ║                                                                            ║
  ║ By downloading the Flutter SDK, you agree to the Google Terms of Service.  ║
  ║ Note: The Google Privacy Policy describes how data is handled in this      ║
  ║ service.                                                                   ║
  ║                                                                            ║
  ║ Moreover, Flutter includes the Dart SDK, which may send usage metrics and  ║
  ║ crash reports to Google.                                                   ║
  ║                                                                            ║
  ║ Read about data we send with crash reports:                                ║
  ║ https://github.com/flutter/flutter/wiki/Flutter-CLI-crash-reporting        ║
  ║                                                                            ║
  ║ See Google's privacy policy:                                               ║
  ║ https://www.google.com/intl/en/policies/privacy/                           ║
  ╚════════════════════════════════════════════════════════════════════════════╝

PS C:\Users\Administrator>
