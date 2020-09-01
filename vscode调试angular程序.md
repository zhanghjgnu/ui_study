## vscode调试angular程序
1 用angular cli创建项目
ng new cloveropen-app01 --strict
cd cloveropen-app01
启动应用程序(这个vscode不能代劳)
ng serve
启动vscode
code.

2 现在应该已经启动了vscode，可以看到语法高亮和括号匹配
需要配置调试插件，安装Debugger for Chrome扩展，打开扩展视图(ctrl+shift+X),输入chrome搜索，安装Debugger for Chrome

3 设置断点，比如app.component.ts文件的
export class AppComponent {
  title = '调试例子';
}
在title上设置个断点(红点)

4 配置chrome debugger，在运行调试界面(ctrl+shift+D)创建一个launch.json的调试配置文件，我们需要修改缺省端口8080为4200
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Launch Chrome against localhost",
      "url": "http://localhost:4200",
      "webRoot": "${workspaceFolder}"
    }
  ]
}

5 开始调试,F5开始调试程序，剩下的步骤和其余的调试一样

注意一点:不要以为start debugging就能运行 ng serve,这个需要提前手动执行一下.


参考
https://code.visualstudio.com/docs/nodejs/angular-tutorial
https://www.digitalocean.com/community/tutorials/how-to-debug-angular-cli-applications-in-visual-studio-code
