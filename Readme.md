# 免Root脚本机器人
#简介
一个**不需要**root权限的类似脚本精灵、按键精灵的自动操作的软件。可以实现自动点击、滑动、输入文字、打开应用等，例如QQ自动抢红包。当然了你也可以把它当做js的IDE来写JavaScript（作为IDE而言目前还比较弱，没有自动补全和包列表等）。另外由于不使用root权限，要点击屏幕上的精确位置是不能实现的，像游戏刷副本自动签到什么的基本上不能做到，这些还是用root权限的脚本精灵吧~~

![screen-capture1](https://raw.githubusercontent.com/hyb1996/NoRootScriptDroid/master/screen-captures/ss1.png)

![screen-capture2](https://raw.githubusercontent.com/hyb1996/NoRootScriptDroid/master/screen-captures/ss2.png)

![screen-capture3](https://raw.githubusercontent.com/hyb1996/NoRootScriptDroid/master/screen-captures/ss3.png)

![screen-capture4](https://raw.githubusercontent.com/hyb1996/NoRootScriptDroid/master/screen-captures/ss4.png)

#基本语法与API
目前支持的自动操作函数还比较少，未来会逐渐增加。
###一、语法
本软件使用JavaScript语言([ECMAscript E5/E5.1](http://www.ecma-international.org/ecma-262/5.1/))，基于[Duktape](http://www.duktape.org/)引擎拓展一些自动操作(点击、长按、滑动等)函数。因而语法参见JavaScript(例如[w3cschool教程](http://www.w3school.com.cn/js/))。

###二、自动操作函数
**自动操作的函数都需要开启"自动操作服务"才能执行，否则执行到相应函数时脚本会停止运行。**
* `click(text)` 点击文本text所在的区域，并返回是否点击成功。当前界面没有出现该文本或者该文本所在区域不可点击时返回false。例如`click("发现")`，是点击"发现"。如果要点击"发现"直至点击成功可以用`while(!click("发现"))`。

> 文本所在区域指的是，从文本处向其父视图寻找，直至发现一个可点击的部件为止。

* `click(left, top, bottom, right)`

>  有些按钮或者部件是图标而不是文字（例如发送朋友圈的照相机图标以及QQ下方的消息、联系人、动态图标），这时不能通过`click(text)`来点击，只能通过描述图标所在的区域来点击。left, bottom, top, right描述的就是点击的区域。至于要定位点击的区域，可以在侧拉菜单开启"点击区域辅助"（或者安卓7.0以上在通知栏点击"修改"添加点击区域辅助快捷设定图标)，之后每次点击或长按都会提示这次点击或长按的区域并自动保存，可以在编辑器的右侧拉菜单中插入。

  点击与长方形范围严格匹配的区域，并返回是否点击成功。其中left为长方形左边与屏幕左边的像素距离，top为上边与屏幕上边的像素距离，right为右边与**屏幕左边**的像素距离, bottom为下边与**屏幕上边**的距离。区域严格匹配。

> 以下的longClick、select、scrollUp、scrollDown的参数均与click类似，不再赘述。

* `longClick` 长按
* `select` 选择
* `scrollUp` 上滑。不加参数时会寻找"最大"的可滑动的控件上滑，例如微信消息列表等。
* `scrollDown` 下滑。不加参数时与scrollUp类似。
* `input(text)` 把所有输入框的文本都置为text。例如input("测试")。
* `input(i, text)` 把第i个输入框的文本设为text。i从0开始。

###三、其他函数
* `sleep(n)` 暂停执行n**毫秒**时间。
* `toast(text)` 显示提示文本。
* `launch(packageName)` 运行包名为packageName的应用。例如launch("com.tencent.mm")是运行微信，应用包名可以通过一些工具获取；获取通过函数launchApp代替
* `launch(packageName, className)` 运行包名为packageName，类名(Activity)为className的应用。
* `launchApp(appName)` 运存应用名称为appName的应用。例如launchApp("QQ")。不同应用名称可能相同，这时只运行其中某一个应用。
* `notStopped` 若当前脚本处于运行状态时返回`true`, 否则返回`false`。对于某些循环, 例如`while(true)`，请用`while(notStopped())`代替，以免死循环造成的脚本无法正常停止。
* `isStoppd` 若当前脚本处于停止状态时返回`true`, 否则返回`false`。
###四、全局变量
* `context` ApplicationContext，参见安卓[android.content.Context](https://developer.android.com/reference/android/content/Context.html)

> 这里的context由于是ApplicationContext，是不可见的，不能用于dialog和其他UI相关。如果要显示弹窗或者视图，请启动UI模式（代码的第一行为`"ui";`既可）并使用activity代替。

* `activity` UI模式下会启动一个Activity来运行脚本，并在UI线程下运行。可通过该变量来获取该activity。

###五、在脚本中调用Java
使用importClass来引入要使用的库，例如:
```javascript
importClass("android.view.View.OnClickListener")
view.setOnClickListener(new OnClickListener(function(){
    Toast.makeText(activity, "Button1 Clicked", Toast.LENGTH_SHORT).show();
    var intent = new Intent(activity, "com.furture.react.activity.DetailActivity");
    activity.startActivity(intent);
}));

view2.setOnClickListener(new OnClickListener({
    onClick: function(){
        Toast.makeText(activity, "Button2 Clicked", Toast.LENGTH_SHORT).show();
    }
}));
```

详细文档参见[DuktapeJava Java Docs](http://gubaojian.github.io/DuktapeJava/javadoc/)。

#License
[CC-BY-NC-SA](https://github.com/hyb1996/NoRootScriptDroid/blob/master/LICENSE.md)
* 署名 — You must give appropriate credit, provide a link to the license, and indicate if changes were made. You may do so in any reasonable manner, but not in any way that suggests the licensor endorses you or your use.
* **非商业性使用** — 您不得将本作品用于商业与盈利目的。
* 相同方式共享 — If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.