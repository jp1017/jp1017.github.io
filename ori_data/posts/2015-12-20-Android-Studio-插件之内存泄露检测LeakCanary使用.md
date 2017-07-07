title: Android Studio 插件之内存泄露检测LeakCanary使用
date: 2015-12-20 14:43:11
categories: Android Studio
tags: [Android Studio,LeakCanary]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：[追风917](http://www.cnblogs.com/jp1017/)

# [LeakCanary](https://github.com/square/leakcanary)
---

A memory leak detection library for Android and Java.

适用于安卓和java的内存泄露检测库，点击上面的标题进入github开始使用吧。

<!--more-->

so，这货不是as专享的，eclipse用户添加jar包就ok的，至于as插件就这么说吧，sowhat。。。

# 使用
---

这货的使用也是很简单，两步：

1 添加库到项目

as里直接在gradle脚本里添加依赖即可：

``` gradle
dependencies {
   debugCompile 'com.squareup.leakcanary:leakcanary-android:1.3.1'
   releaseCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.3.1'
 }
```

2 应用里安装

```java
public class ExampleApplication extends Application {

  @Override public void onCreate() {
    super.onCreate();
    LeakCanary.install(this);
  }
}
```

Ok，you're good to go! LeakCanary will automatically show a notification when an activity memory leak is detected in your debug build.

来几个图，欣赏下吧：

经过上面两步后，LeakCanary就在监控你的应用咯，

![app](http://7xlah4.com1.z0.glb.clouddn.com/2015-10-14-11-15-24.png)

我们看，安装好应用后，生成了一个Leaks的应用(最后一个黄黄的东西)，这货会实时监控你，以防你干坏事。。。。。。。

![leaks](http://7xlah4.com1.z0.glb.clouddn.com/2015-10-14-11-15-0.png)

我们看到，这货检测到我的应用出现了内存泄露问题，然后给出详细内容，还是很赞的哦，后面我们就可以解决这个问题，使应用更健壮。


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！