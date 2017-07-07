title: Android Studio 开发错误 Installation failed with message INSTALL_FAILED_SHARED_USER_INCOMPATIBLE
date: 2016-04-01 15:39:35
categories: Android Studio
tags: Android Studio
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![每日一景](http://7xlah4.com1.z0.glb.clouddn.com/20160401212606.jpg)

之前所长给了个 [CarLauncher](https://github.com/zhoumushui/CarLauncher) 的项目，当时编译失败，少库，搁置了几天。

后来作者回复了我，我感觉那些库没用，就注释掉了，编译通过，也可以打包了，但是运行出现错误:

![app](http://7xlah4.com1.z0.glb.clouddn.com/20160401153744.jpg)

之前遇到过这个问题，当时是手机有个app，更改签名后再安装出现该界面，所以，删除旧的 app 就可以安装了。

<!--more-->

但是现在的问题是，这个 app 是第一次安装啊，就出现了这个，想了想应该是权限的问题，看了下清单目录，的确有变红的东西，于是暂时注释掉他们，再次编译，安装后问题依旧，想不通了，于是 google 了下，出现此问题的还挺多，在这里找到了答案：

[INSTALL_FAILED_SHARED_USER_INCOMPATIBLE while using shared user id](https://stackoverflow.com/questions/15205159/install-failed-shared-user-incompatible-while-using-shared-user-id)

**解决方法**有下面几种：

+ Removed existing application (if it is already installed )
+ Removed share user ID from android manifest
+ Bulid the application.
+ Now enter share user ID again
+ build the application 1 more time

我用的第二个建议，之前清单文件开头是这样的:

```
    package="com.tchip.carlauncher"
          android:sharedUserId="android.uid.system"
          android:versionCode="2"
          android:versionName="2016.03.30-15:50" >
```

现在我去掉了第一行的 `sharedUserId`，变成这样：

```
    package="com.tchip.carlauncher"
          android:versionCode="2"
          android:versionName="2016.03.30-15:50" >
```

重新编译，运行后，正常安装了，原来原作者是把该 app 放到了系统，并用到系统的一些私有权限等，所以导致 app 安装到外部会出错。

我把该项目 fork 到我的仓库，并导入了 Android Studio，地址：[https://github.com/jp1017/CarLauncher](https://github.com/jp1017/CarLauncher)




安卓开源库收集整理中，欢迎 PR， star，地址：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！