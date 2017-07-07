title: '安卓 jni 开发错误 UnsatisfiedLinkError: Native method not found'
date: 2016-03-22 18:55:15
categories: Android
tags: [Android,jni]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

jni 开发的坑挺多的，今天遇到好多个，现在这个是这样的：

```
UnsatisfiedLinkError: Native method not found
```

很明显是因为 `native` 方法找不到，google 下发现该错误出现频率还蛮高的，基本有两种：

+ so 文件找不到

这个就需要配合手机 cpu 类型和 so 类型服用，仔细查看去吧，问题不大的。

<!--more-->

+ native 方法名有误

我就是犯了这个错误，涉及 jni 开发最基本的知识点：

C语言方法命名规则 : 

```
Java_完整包名类名_方法名(JNIEnv *env, jobject thiz)  //注意完整的类名包名中包名的点要用 _ 代替;
```

参数介绍 : C语言方法中有两个重要的参数, JNIEnv *env, jobject thiz ;

1. `JNIEnv` 参数 : 该参数代表Java环境, 通过这个环境可以调用Java中的方法;
2. `jobject` 参数 : 该参数代表调用jni方法的类;

好了，原来，我是调整了包名，而这个 native 方法名没有改，导致该错误的发生。

还是那句话，用心一点，用力一点，你会做的更好，加油吧，骚年！


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！