title: Android Studio美化之优雅的logcat
date: 2015-11-17 18:45:33
categories: Android Studio
tags: [Android Studio,logcat]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

先来个图，图样吐sexy：

![sexy](http://7xlah4.com1.z0.glb.clouddn.com/20151113160101.jpg)

很简单，跟我走吧，两步：

<!--more-->

**1. 引入Logger库**
---

首先，这个sexy logcat用到了一个库，地址：[https://github.com/orhanobut/logger](https://github.com/orhanobut/logger)

把这个库加到你项目里，可以gradle配置库，也可以把库里的主要java文件加到项目里，推荐后者，还可以修改，上面的图就是我修改后的效果，去掉显示级别的显示。

然后初始化该Logger，在项目的BaseApplication类的onCreate()方法里加入init代码：

```java
    if (BuildConfig.DEBUG) {
        Logger.init().hideThreadInfo().setLogLevel(LogLevel.FULL);
    }
```

然后随便打印就可以优雅的显示：

```java
    Logger.v("sexy logcat");
    Logger.d("sexy logcat");
    Logger.i("sexy logcat");
    Logger.w("sexy logcat");
    Logger.e("sexy logcat");
```

**2. 显示logcat的显示颜色**
---

这个就easy了，不同的主题有有不同的设置，更多的主题请来这里：[Android Studio主题仓库](http://color-themes.com/?view=index) 

当然我们还是自定义，在Settings --> Editor --> Colors&Fonts --> Android Logcat里设置颜色即可

![color](http://7xlah4.com1.z0.glb.clouddn.com/20151113161235.png)

自己定义下吧，也可以去颜色空间库里找个你喜欢的。

赶紧设置下吧，sexy logcat，你值得拥有。。。


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！