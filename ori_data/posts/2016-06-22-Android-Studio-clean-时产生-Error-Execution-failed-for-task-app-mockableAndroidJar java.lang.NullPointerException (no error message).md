title: "Android Studio clean 时产生 Error:Execution failed for task ':app:mockableAndroidJar' > java.lang.NullPointerException (no error message) "
date: 2016-06-22 14:33:48
categories: Android Studio
tags: Android Studio
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![每日一景](https://drscdn.500px.org/photo/125301449/m%3D2048/ba230b1d36c2b50d2b7af57be81c94e7)

Android Studio 使用，clean 后 gradle message 出现如下错误：

```
Error:Execution failed for task ':app:mockableAndroidJar'.
> java.lang.NullPointerException (no error message)
```

![error](‪C:\Users\JP\Desktop\20160627-09-39-53-613_com.eg.android.png)

编译能通过，运行也正常，但是强迫症啊，你懂吧 T_T


<!--more-->


这个 Error 指出，`task ':app:mockableAndroidJar'`运行失败，我们看下 gradle message：

![gradle message](http://7xlah4.com1.z0.glb.clouddn.com/20160622145225.jpg)

我们可以看到 clean 时有这么些个 task，其中就有我们出错的 `:app:mockableAndroidJar`，既然出错，那么暂时先关闭这个咯

原来 as 从2.0开始增加了些实验功能，我们可以在 Setting 里找到：

![expert](http://7xlah4.com1.z0.glb.clouddn.com/20160622145553.jpg)

as 默认最后一项是选中的，我们取消选中就可以了，确定后，我们继续 clean：

![clean](http://7xlah4.com1.z0.glb.clouddn.com/20160622145807.jpg)

这次 task 里就没有了 `':app:mockableAndroidJar'`，当然也就没有错误咯，哈哈。


最后，非常感谢您的阅读，有任何疑问，可以后面评论，谢谢！

神奇的安卓开发网站：[http://androidcat.com/](http://androidcat.com/)

安卓开源库收集整理：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！