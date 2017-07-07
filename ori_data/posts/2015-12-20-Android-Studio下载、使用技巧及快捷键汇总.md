title: Android Studio下载、使用技巧及快捷键汇总
date: 2015-12-20 14:15:39
categories: Android Studio
tags: Android Studio
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：[追风917](http://www.cnblogs.com/jp1017/)

# Android Studio及SDK下载
---

下载地址：[http://www.androiddevtools.cn/](http://www.androiddevtools.cn/)

# 加快gradle构建
---

1 开启gradle单独的守护进程
在下面的目录下面创建gradle.properties文件：

/home/username/.gradle/ (Linux)
/Users/username/.gradle/ (Mac)
C:\Users\username\\.gradle (Windows)

在gradle.properties文件中输入如下代码：

<!--more-->

``` bash
# Project-wide Gradle settings.
# IDE (e.g. Android Studio) users:
# Settings specified in this file will override any Gradle settings
# configured through the IDE.
# For more details on how to configure your build environment visit
# http://www.gradle.org/docs/current/userguide/build_environment.html
# The Gradle daemon aims to improve the startup and execution time of Gradle.
# When set to true the Gradle daemon is to run the build.
# TODO: disable daemon on CI, since builds should be clean and reliable on servers
org.gradle.daemon=true
# Specifies the JVM arguments used for the daemon process.
# The setting is particularly useful for tweaking memory settings.
# Default value: -Xmx10248m -XX:MaxPermSize=256m
org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
# When configured, Gradle will run in incubating parallel mode.
# This option should only be used with decoupled projects. More details, visit
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:decoupled_projects
org.gradle.parallel=true
# Enables new incubating mode that makes Gradle selective when configuring projects. 
# Only relevant projects are configured which results in faster builds for large multi-projects.
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:configuration_on_demand
org.gradle.configureondemand=true
```

当然，上面的这些参数也可以配置到前面的用户目录下的gradle.properties文件里，那样就不是针对一个项目生效，而是针对所有项目生效。
gradle.properties配置文件主要就是增大gradle运行的java虚拟机的大小，让gradle在编译的时候使用独立进程，让gradle可以平行的运行。

2 申请大内存
as安装目录/bin/studio64.vmoptions or studio.vmoptions(linux下，其他系统找类似文件)
使用文本编辑器打开，找到起始两行，如下
-Xms128m
-Xmx750m

修改最小值和最大值，建议为
-Xms256m
-Xmx2048m

3 incremental dex
改变incremental dexing的值，这是一个实验的功能并且默认是关闭的。打开这个开关有可能会导致构建失败，错误信息类似jdk finished with non-zero exit value 2
（尤其是在连续运行的时候），但我还是推荐你尝试一下，看看它是否对你有用。

在主APP模块的build.gradle文件中，添加下面的代码：

```gradle
    dexOptions {
        incremental true
    }
```

或者在Project Structure里设置下：

![incremental](http://7xlah4.com1.z0.glb.clouddn.com/20151011164422.png)

# 使用技巧
---

1 自动导入
自动导入。当你从其他地方复制了一段代码到Android Studio中，默认的Android Studio不会自动导入这段代码中使用到的类的引用。你可以这么设置。
Settings --> Editor --> Auto Import ，设置Insert imports on paste为All，并勾选 Add unambiguous improts on the fly 。

![自动导入](http://7xlah4.com1.z0.glb.clouddn.com/20151011164638.png)

2 显示行号
Settings --> Editor --> General --> Appearance ，勾选 Show line numbers 。

![显示行号](http://7xlah4.com1.z0.glb.clouddn.com/20151011164847.png)

3 plugins

请看这里：[Android Studio中常用插件及浅释](http://jp1017.gitcafe.io/2015/09/26/Android-Studio%E4%B8%AD%E5%B8%B8%E7%94%A8%E6%8F%92%E4%BB%B6%E5%8F%8A%E6%B5%85%E9%87%8A/)

4 分析某个值的来源

Find Actions(ctrl+shift+a)输入”Analyze Data Flow to Here”，可以查看某个变量某个参数其值是如何一路赋值过来的。
对于分析代码非常有用。

![Analyze Data Flow to Here](http://7xlah4.com1.z0.glb.clouddn.com/201510100.gif)

5 多行编辑
强大的神技之一，用过vim的vim-multiple-cursors或者Sublime Text的多行编辑都不会忘记那种快感！ 也许不是平时用得最多的技能，但是却是关键时刻提高效率的工具。

快捷键：Alt+J

![多行编辑](http://7xlah4.com1.z0.glb.clouddn.com/201510103.gif)

6 列编辑

在vim中叫作块编辑，同样神技！使用方法：按住Alt加鼠标左键拉框即可，另外也可按Alt+Shift+Insert之后拖框选择。

![列编辑](http://7xlah4.com1.z0.glb.clouddn.com/201510101.gif)

7 分析堆栈信息

Find Actions(ctrl+shift+a)输入”analyze stacktrace”即可查看堆栈信息。

![分析堆栈信息](http://7xlah4.com1.z0.glb.clouddn.com/201510105.gif)

8 tools命名空间在布局预览中的使用

as的实时预览功能很棒，右侧点击Preview就可以，为了预览效果更好我们可以在布局上填充数据，比如我们在TextView中设定text属性来看下字体大小与布局是否正确，但是软件发布后这些都要一一删除，很麻烦吧。别着急，as很强大，这个细节当然不会错过，so，tools命名空间就来解决这一难题：

只需要在xml布局文件中添加tools命名空间，并设置其text属性就ok了，此属性和正式发布的版本完全无关，是不是很酷？

好了，我们来玩一下：用之前只需要在根布局添加命名空间就ok了

```xml
<LinearLayout
 xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    ...
```

用法很简单，只需要**用tools的命名空间代替android的命名空间**，比如我们有个按钮，可以添加文字预览：

![tools命名空间](http://7xlah4.com1.z0.glb.clouddn.com/20151015110039.png)

注意的是tools属性只能在layout文件中使用，而且只能使用framework自带的一些属性，不可以用使用自定义属性，不过这足够了，基本上能满足我们的需求了。


# Android Studio常用快捷键
---

工欲善其事必先利其器，as快捷键还是很有必要的，下面总结下。

说明：该快捷键是windows通用的，在linux下大部分通用，mac用户绕过，可查看keymap自行学习，或者点击Tips of the Day学习。


|	功能	| 快捷键     |
|--  -       |:--- |
|代码注释(//)    |  Ctrl+/|
|代码注释(/*)     |  Ctrl+Shist+/|
|清除无用的包引用  |   Ctrl+Alt+O|
|快速覆盖方法    |    Ctrl+O|
|放大选中范围   | Ctrl+W
|缩小选中范围   |  Ctrl+Shift+W
|代码的大小写转换  | Ctrl+Shift+U
|文件方法结构（查看整个类结构）   |  Ctrl+F12
|新建Constructor Getter Setter.. | Alt+Insert
|折叠代码   | Ctrl+ -
|展开代码   | Ctrl+ +
|折叠所有代码|  Ctrl+Shift+ -
|展开所有代码 | Ctrl+Shift+ +
|删除一行代码 | Ctrl+Y
|代码上移  | Ctrl+Shift+UP
|代码下移  | Ctrl+Shift+Down
|搜索   | Ctrl+F
|查找替换 | Ctrl+R
|搜索无处不在 | Shift+Shift
|搜索类  | Ctrl+N
|搜索文件 | Ctrl+Shift+N
|搜索字符 | Ctrl+Shift+Alt+N
|查找line行   | Ctrl+G
|构建   | Ctrl+F9
|运行|  Shift+F10
|显示最近打开的文件列表 | Ctrl+E
|显示最近修改的文件列表 | Ctrl+Shift+E
|智能代码补全  | Ctrl+Shift+空格
|代码自动完成  | Ctrl+Shift+Enter
|意图行动，也可点击前面的灯泡  | Alt+Enter
|重构(refactor),重命名  | Shift+F6
|重构，提取代码到单独的方法 | Ctrl+Alt+M
|可以把代码包在一块内，例如try/catch | Ctrl+Alt+T
|重构，可选项更多 | Ctrl+Shift+Alt+T
|动态模版   | Ctrl+J
|可以整合两行 | Ctrl+Shift+J
|单步调试  | Ctrl+F8
|模拟器旋转屏幕  | Ctrl+F11
|显示类或方法详情，也就是看javadoc详情 | Ctrl+Q
|关闭打开的文件    | Alt+F4 或者Shift+click
|跳转到上次编辑的地方  | Ctrl+Shift+backspace
|计算变量值   | Alt+F8
|在方法间上下移动    |Alt+up/down
|启用/禁用拼写检查    |Ctrl+Alt+H
|查看方法的调用位置   |Ctrl+Shift+H
|查看类的继承关系    |Ctrl+H
|git commit changes   |Ctrl+K
|git push commits   |Ctrl+Shift+K
|添加或移除书签   |F11
|显示书签   |Shift+F11或者Favorates里查看
|查找Actions  |Ctrl+Shift+A


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！