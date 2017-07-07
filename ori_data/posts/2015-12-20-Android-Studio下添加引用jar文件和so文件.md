title: Android Studio下添加引用jar文件和so文件
date: 2015-12-20 14:46:57
categories: Android Studio
tags: [Android Studio,jar]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

安卓开发中我们常会遇到jar文件和so文件的引用，下面介绍下在as下如何添加使用，这里以百度地图sdk所需的文件为例。

# 添加jar
---

1 在本地复制jar文件，然后到as界面，project标签下，找到app/libs，ctrl+v粘贴到libs文件夹下，结果如下：

![libs](http://7xlah4.com1.z0.glb.clouddn.com/20150928175419.png)

<!--more-->

2 添加到依赖库

之前可以右键jar包，“Add As Library”的，这个1.3.2版本给删除了吗？我们可以这样：ctrl+shift+alt+s进入project structure设置界面，然后添加包，操作如下：

![add](http://7xlah4.com1.z0.glb.clouddn.com/20150928175917.png)

在弹出的对话框中，找到libs下的三个jar包，依次添加即可，只能一次添加一个，不知google怎么想的：

![libs](http://7xlah4.com1.z0.glb.clouddn.com/20150928175951.png)

添加完成后，项目会自动同步，完成后，会在gradle.build脚本里看到添加了依赖。

![dep](http://7xlah4.com1.z0.glb.clouddn.com/20150928180627.png)

# 添加so
---

没有so文件或添加路径不对，会出现下面类似错误：

``` bash
java.lang.UnsatisfiedLinkError: Native method not found: 
    com.baidu.platform.comjni.map.commonmemcache.JNICommonMemCache.Create:()
```

添加时有个注意点就是添加的路径要设置正确，Android Studio 默认的so文件路径是`app/src/main/jniLibs/armeabi`，和eclipse是不一样的，要注意哦。

按照添加jar文件的方法，复制粘贴就可以，没有jniLibs文件夹的新建一个，添加后的结果如下：

![so](http://7xlah4.com1.z0.glb.clouddn.com/20150928181202.png)

好了，这样就ok的，有时还需要添加armeabi-v7a，x86文件夹，视平台酌情增删。

20150930补充：

当然这个so文件的目录是可以指定的，比如在gradle脚本里这样配置：

``` bash
sourceSets {
    main {
        jniLibs.srcDirs = ['libs']
    }
}
```

这样配置的话，so文件位置就和jar文件目录一致，也就是和eclipse一样，但是我还是推荐使用as默认的文件目录结构，而我在这里也犯了一个错误，请看我的另一篇文章：[安卓百度地图开发so文件引用失败问题研究](http://jp1017.gitcafe.io/2015/09/30/%E5%AE%89%E5%8D%93%E7%99%BE%E5%BA%A6%E5%9C%B0%E5%9B%BE%E5%BC%80%E5%8F%91so%E6%96%87%E4%BB%B6%E5%BC%95%E7%94%A8%E5%A4%B1%E8%B4%A5%E9%97%AE%E9%A2%98%E7%A0%94%E7%A9%B6/)


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！