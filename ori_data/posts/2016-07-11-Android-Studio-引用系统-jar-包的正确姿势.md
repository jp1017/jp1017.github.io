title: Android Studio 引用系统 jar 包的正确姿势
date: 2016-07-11 17:46:59
categories: Android Studio
tags: [Android Studio,jar]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![每日一景](https://drscdn.500px.org/photo/118785927/m%3D2048/51dda482036d6d91f2902406c3e8f41f)

Android Studio 如何引用 jar 包，相信大家都会操作的，现在问题来了，对于系统里的 jar，比如 framework.jar该如何配置呢？

这里来个简单的需求吧，我们自己编译系统，并且有串口驱动，该驱动有提供 so 和 jar。这些文件配置到系统里面，当jar更新，api 不变的话，只要更新系统就可以，而应用程序可以不变而采用系统最新的 jar 包。

也就是说，编译时引用jar包，而不把该 jar 打包进 apk，而apk运行时采用系统里面的 jar 即可。

这个需求在 eclipse 里面很好配置，只要设置 jar 为系统 jar 就可以，那么 AS 该如何配置呢？

<!--more-->

AS 采用 gradle 编译，那么配置 gradle 就可以的，google 后来到了这里：

[ Android Studio导入framework.jar等系统jar包方式](http://blog.csdn.net/zhuzp_blog/article/details/51674468)

也就是说配置 jar 包时采用 `provided` scope，比如：

```
provided files('libs/BaiduLBS_Android.jar')
```

这里采用 `provided`，意思是说编译时引用 `BaiduLBS_Android.jar`,而不把该 jar 打包进 apk。
而我们常用的是 `compile`，意思是编译时引用 jar 包，并打包进 apk。

配置好后，sync 出现错误：

```
Error:(26, 1) Failed to notify build listener.
> Could not get unknown property 'options' for root project '***' of type org.gradle.api.Project.
> Could not get unknown property 'options' for project ':app' of type org.gradle.api.Project.
```

再次 google，无果，没人遇到过，，不知道当时作者怎么解决的，还没给我回复，后来删除 root gradle 配置部分，也就是仅仅修改 jar 包的引用方式为 `provided`，编译通过，运行后，出现异常：

```
java.lang.NoClassDefFoundError: com.xx.xx
```

说的很明白了，Class 找不到，因为没有把 jar 打包进 apk ，肯定找不到了，那么还需要配置别的地方，看来还得配置 root gradle ，但是上面的错误搜索不到，那么该如何进行下去，要换 eclipse 开发？显然这不能忍。。。

>非淡泊无以明志，非宁静无以致远

没错，静下来，静下来，从最简单的开始。

看了下所长的 eclipse 版本项目，这仅仅是所长测试驱动调试使用的，移植到 AS 后，发现编译都不过，因为项目里没有 jar 包，那这个应用是如何跑起来的呢，然后在项目清单文件里找到了答案。

`AndroidManifest.xml` 文件里有这样的配置：

```
<application
    android:name=".app.BaseApplication"
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
    <uses-library android:name="com.**.**" />
    
    ...

</application>
```

大家都看到了，就是 `<uses-library android:name="com.**.**" />` 这一句，后面就是 jar 包的包名。

这样编译，运行都正常了，Bingo！

最后总结下，Android Studio 引用系统 jar 包需要配置两个内容：

1. `provided` 方式应用 jar 包，//编译时引用 jar 而不把 jar 打包进 apk
2. 清单文件里配置 `<uses-library android:name="com.**.**" />` //告诉应用引用系统 jar 的包名


最后，非常感谢您的阅读，有任何疑问，可以后面评论，谢谢！

神奇的安卓开发网站：[http://androidcat.com/](http://androidcat.com/)

安卓开源库收集整理：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！