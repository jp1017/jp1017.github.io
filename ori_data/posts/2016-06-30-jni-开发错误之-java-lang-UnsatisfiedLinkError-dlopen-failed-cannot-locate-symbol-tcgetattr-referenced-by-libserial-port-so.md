title: 'jni 开发错误之 java.lang.UnsatisfiedLinkError: dlopen failed: cannot locate symbol tcgetattr referenced by libserial_port.so...'
date: 2016-06-30 09:12:57
categories: Android
tags: [Android,jni]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![每日一景](https://drscdn.500px.org/photo/133812917/m%3D2048/d7dab63e94897ec0ede8d33f5fdbb33f)

昨天把串口的驱动代码重新编译下，出现了问题：

```
java.lang.UnsatisfiedLinkError: dlopen failed: cannot locate symbol tcgetattr referenced by libserial_port.so...
```

一般 jni 开发的话，C 代码命名常出错，检查了下，是 OK 的，然后 google了下，发现基本就下面两个：

[[Android 调试] android串口通信问题，基于android studio](http://www.eoeandroid.com/thread-914514-1-1.html)
[NDK UnsatisfiedLinkError when lowering project api level 21 to 19](https://stackoverflow.com/questions/35080172/ndk-unsatisfiedlinkerror-when-lowering-project-api-level-21-to-19/38112597#38112597)

而 stackoverflow 的哥们 rebuild 就好了，我各种折腾也不见好呐，于是看到上一篇 eoe 的，给了我思路：

我们看下包含该函数 `**tcgetattr**` 的文件， 比较下 NDK api 19 和 23 的 `termios.h` 文件：

23 的该文件目录：`D:\Android\Sdk\ndk-bundle\platforms\android-23\arch-arm\usr\include`

<!--more-->

api 23 下 termios.h 文件：

![23](http://7xlah4.com1.z0.glb.clouddn.com/20160630174226.jpg)

api 19 下 termios.h 文件：

![19](http://7xlah4.com1.z0.glb.clouddn.com/20160630174409.jpg)

这两个文件确实不一样，于是把19下的该文件复制到 jni 目录，

![jni](http://7xlah4.com1.z0.glb.clouddn.com/20160630174555.jpg)

重新编译后，运行，OK，当然设置 `compileSdkVersion 19` 也是可以的

看了下，该文件从 api 21 就变化了，那么升级 SDK 后，不能高版本编译 so 了吗？

还搜到了 gradle 实验插件：

[Experimental Plugin User Guide](https://sites.google.com/a/android.com/tools/tech-docs/new-build-system/gradle-experimental#TOC-Standalone-NDK-Plugin)

玩了下，由于采用的插件较多，各种问题，没成功，有玩过的朋友欢迎指点啊，谢谢！


最后，非常感谢您的阅读，有任何疑问，可以后面评论，谢谢！

神奇的安卓开发网站：[http://androidcat.com/](http://androidcat.com/)

安卓开源库收集整理：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！!