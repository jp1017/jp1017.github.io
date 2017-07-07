title: Android Studio 下安卓 jni 开发错误 undefined reference to AndroidBitmap_getInfo
date: 2016-03-21 19:48:13
categories: Android Studio
tags: [Android Studio,jni]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

# 又掉坑里了
---

今天开发 uvc，又遇到了坑：

![坑](http://7xlah4.com1.z0.glb.clouddn.com/20160321194641.jpg)

和之前的这个坑类似：[Android Studio 下安卓 jni 开发错误 undefined reference to __android_log_print](http://t.cn/RGDdX6q)

错误提示：`函数未定义`

去 ndk 里查看了下，这些函数都有定义的，在 ndk 目录中文件 `**\ndk-bundle\platforms\android-19\arch-arm\usr\include\android\bitmap.h` 里，我的代码里包含了该头文件。

<!--more-->

# 填坑
---

前几天开发 串口 还有印象，当时是配置的 gradle 脚本，这次也应该是的，只是，该如何修改呢？

查看了下 Android.mk 文件，是这样的：

```mk
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)

LOCAL_MODULE    := webcam
LOCAL_SRC_FILES := webcam.c yuv.c video_device.c util.c capture.c
LOCAL_LDLIBS    := -llog -ljnigraphics
LOCAL_CFLAGS += -std=c99

include $(BUILD_SHARED_LIBRARY)
```

注意到，有这一句：`LOCAL_LDLIBS    := -llog -ljnigraphics`，你发现了什么吗，我似乎明白了，于是修改 gradle 脚本如下：

```gradle
    ndk {
        moduleName "webcam"
        ldLibs "log"
        ldLibs "jnigraphics"//新增
    }
```

其实这句的意思就是增加动态链接库 `jnigraphics`，这样就搞定了呢，是的，就是这样。

这么看来，用 Android Studio 开发 jni，比 eclipse 方便好多，虽然我没怎么用过 eclipse，当然这得益于 gradle 脚本强大的功能咯。


>生活不止眼前的苟且，还有远方的诗和田野

# 继续填坑
---

上面的解决了后，编译，又遇到了下面一个坑：

![c99](http://7xlah4.com1.z0.glb.clouddn.com/20160321202830.jpg)

错误定位代码如下：

```c
int uninit_device() {
    for(unsigned int i = 0; i < BUFFER_COUNT; ++i) {
        ***
    }
    ***
}
```

语法错误咯，我修改成下面的：

```c
int uninit_device() {
    unsigned int i = 0;
    for(i; i < BUFFER_COUNT; ++i) {
        ***
    }
    ***
}
```

编译，同样的问题又出现了，只是位置变了，还是语法错误，于是同样的方法修改，再编译，尼玛，还有问题。。。

一番修改，终于编译通过，也正常运行了，但是修改了好多，此时`程序员的懒惰`发挥作用：

>能不能不修改代码，编译工具用 C99 呢？

想法不错，当然这也是可行的，也很好操作。

细心的同学，应该注意到在 Android.mk 文件里有这一句：`LOCAL_CFLAGS += -std=c99`,然。。并。。。。

但是这是 as，需要配合 gradle 服用，so，继续修改 gradle 脚本咯，增加一句：`cFlags "-std=c99"` 搞定。

gradle 脚本 ndk 部分如下：

```gradle
    ndk {
        moduleName "webcam"
        ldLibs "log"
        ldLibs "jnigraphics"
        cFlags "-std=c99"
    }
```

你会发现和 Android.mk 文件类似，当然了，语言不同，语法各异。

关于 uvc 的库和 demo，我找了个，修改后导入 Android Studio, 上传到了 github：[AndroidUvcDemo](https://github.com/jp1017/AndroidUvcDemo)

好了，今天就到这里了，jni，坑很多，用力一点，用心一点，跳出来吧。

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！