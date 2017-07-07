title: Android Studio 下安卓 jni 开发错误 undefined reference to __android_log_print
date: 2016-03-15 19:10:08
categories: Android
tags: [Android,jni]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

# jni
---

最近要搞安卓下串口的读写，需要用到 jni，然后遇到了这个问题，坑挺多。

串口读写参考文章：[Android串口操作，简化android-serialport-api的demo（附源码）](http://blog.csdn.net/akunainiannian/article/details/8740007)

我把这个源码导入了 android studio，上传到 github：[AndroidSerialPort](https://github.com/jp1017/AndroidSerialPort)

jni 开发参考文章：[【Android 应用开发】Android 开发 之 JNI入门 - NDK从入门到精通](http://blog.csdn.net/shulianghan/article/details/18964835)

还有个极客学院的：[JNI 开发流程](http://wiki.jikexueyuan.com/project/jni-ndk-developer-guide/workflow.html)

<!--more-->

# 跳坑
---

这个问题就是导入 android studio 后遇到的，错误如下：

```java
undefined reference to __android_log_print
```

该函数在 ndk 目录中文件 `**\ndk-bundle\platforms\android-24\arch-arm\usr\include\android\log.h` 里，我的代码里包含了该头文件。

 `Android.mk` 文件内容是这样的：

```mk
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

LOCAL_MODULE    := serial_port
LOCAL_SRC_FILES := SerialPort.c
LOCAL_LDLIBS    := -llog

include $(BUILD_SHARED_LIBRARY)
```

google 了下，有如下解决方法：

1 修改 Android.mk 文件相关:

```mk
LOCAL_LDLIBS := -llog
```

而我的本来就是这个样子啊，失败

2 修改 Android.mk 文件:

```mk
LOCAL_LDLIBS := -L$(SYSROOT)/usr/lib -llog
```

问题依旧~~~

3 如果使用 android studio，需要修改 gradle 脚本：

```gradle
android {
    defaultConfig {
        ndk {
            moduleName "serial_port"
            ldLibs "log"
        }
    }
}
```

也就是添加了一句：`ldLibs "log"`

这个解决了我的问题。

详细代码我传到了 github：[AndroidSerialPort](https://github.com/jp1017/AndroidSerialPort)

>其实，用 Android Studio 开发 jni，不需要 Android.mk 文件，只要配置好 gradle 就可以了。今天遇到同样的问题，来这里看看吧：[Android Studio 下安卓 jni 开发错误 undefined reference to AndroidBitmap_getInfo](http://www.jianshu.com/p/bfe32e201eb9)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！