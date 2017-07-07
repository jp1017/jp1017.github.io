title: 安卓 jni 开发之 native 方法的动态注册
date: 2016-03-23 18:21:38
categories: Android
tags: [Android,jni]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![依旧](http://7xlah4.com1.z0.glb.clouddn.com/20160325222213.jpg)

最近一直在搞 jni 开发，里面坑挺多，其实都是自己不注意导致的。如果你不知道 jni，但是你又想了解这个坑，请先到隔壁喝个茶，取点经，隔壁地址：

[Android 开发 之 JNI入门 - NDK从入门到精通](http://blog.csdn.net/shulianghan/article/details/18964835)

昨天突发奇想，既然 native 方法的命名是这样的：

```
    Java_完整包名类名_方法名()
```

那么如果包名或者类名含有下划线“_”时，会怎么样呢？哈哈哈

<!--more-->

很悲剧，按照上面的 native 方法的命名规则，会出现 `Native method not found` 的异常，那么该如何处理呢？只能保证包名和类名不出现下划线吗？

此时，我又想起了 [bunnyblue](https://github.com/bunnyblue) 大神的名言：

>对于程序来言，一切都是可以实现的，就是代价的问题

多么简单粗暴，一切都是可以搞的，那么这个该如何处理呢，就是本篇文章里讲到的**动态注册 native 方法** 

# 动态注册 native 方法

先上一段代码吧：

```C
static jstring nativeDynamicRegFromJni(JNIEnv *env, jobject obj)
{
    return (*env) -> NewStringUTF(env, "动态注册调用成功");
}

JNINativeMethod nativeMethod[] = {{"dynamicRegFromJni", "()Ljava/lang/String;", (void*)nativeDynamicRegFromJni}};

JNIEXPORT jint JNICALL JNI_OnLoad(JavaVM *jvm, void *reserved)
{
    JNIEnv *env;
    if ((*jvm) -> GetEnv(jvm, (void**) &env, JNI_VERSION_1_4) != JNI_OK)
    {
        return -1;
    }

    jclass clz = (*env) -> FindClass(env, "github/jp1017/hellojni/MainActivity");

    (*env) -> RegisterNatives(env, clz, nativeMethod, sizeof(nativeMethod) / sizeof(nativeMethod[0]));

    return JNI_VERSION_1_4;
}
```

上面就是动态注册的全部代码了，java 代码中的函数是 `dynamicRegFromJni`，调用的 native 方法是 `nativeDynamicRegFromJni`，该方法没有参数，返回值是一个字符串。

如果你看不出来，请继续往下看，容我慢慢道来：

动态注册的原理是这样的：JNI 允许我们提供一个函数映射表，注册给 JVM，这样 JVM 就可以用函数映射表来调用相应的函数，
而不必通过函数名来查找相关函数(这个查找效率很低，函数名超级长)。

# JNINativeMethod

这是一个结构体，在 jni.h 头文件中定义：

```C
typedef struct {

const char* name;

const char* signature;

void* fnPtr;

} JNINativeMethod;
```

Java 与 jni 通过该结构体建立联系，其中有三个变量：


+ name：Java 中函数的名字。
+ signature：签名符号，描述了函数的参数和返回值
+ fnPtr：函数指针，指向一个被调用的函数

我们看上面定义的结构体数组：

```C
JNINativeMethod nativeMethod[] = {{"dynamicRegFromJni", "()Ljava/lang/String;", (void*)nativeDynamicRegFromJni}};
```

可以看出，里面有一个成员，该成员第一个参数 "dynamicRegFromJni",java 函数名；第二个参数“()Ljava/lang/String:",是签名符号，意思是该函数没有参数，返回一个字符串；第三个参数就是要调用的 native 方法。

关于 `签名符号` 可以参考这里：[Android JNI编程—JNI基础](http://www.jianshu.com/p/aba734d5b5cd)

当 java 通过 System.loadLibrary 加载完 JNI 动态库后，紧接着会调用 JNI_OnLoad 的函数。

# JNI_OnLoad()函数

JNI_OnLoad()函数有两个重要的作用：

1 指定 jni 版本：告诉 JVM 该组件使用哪一个 jni 版本(若未提供JNI_OnLoad()函数，JVM 会默认该使用最老的 JNI 1.1版本)，如果要使用新版本的JNI，例如JNI 1.4版，则必须由 JNI_OnLoad() 函数返回常量 JNI_VERSION_1_4 (该常量定义在 jni.h 中) 来告知 JVM 。

2 一系列初始化操作，当 JVM 执行到 System.loadLibrary() 函数时，会立即调用 JNI_OnLoad() 方法，因此在该方法中进行各种资源的初始化操作最为恰当，

# RegisterNatives

动态注册的工作就是在这里完成的。

RegisterNatives在 jni.h 中是这么定义的：

```java
jint RegisterNatives(jclass clazz, const JNINativeMethod* methods,jint nMethods)
```

该函数有三个参数：

+ clazz: java 类名，通过 FindClass 得到
+ methods：JNINativeMethod 结构体指针
+ nMethods： 方法个数

我们看上面该函数的调用：

```C
(*env) -> RegisterNatives(env, clz, nativeMethod, sizeof(nativeMethod) / sizeof(nativeMethod[0]));
```

咦，里面怎么又四个参数，嗯，是的，这个是 C 编写规则，如果是 C++ 的话，是这样的：

```C++
env -> RegisterNatives(clz, nativeMethod, sizeof(nativeMethod) / sizeof(nativeMethod[0]));
```

看出差别了吗，如果你遇到下面类似错误：

```sh
error: request for member 'FindClass' in something not a structure or union
```

可能就是 C 和 C++ 编写规则的问题咯，check 一下，没什么大问题的。

ok，这样就完成了 native 方法的动态注册

这里补充一点，jni 开发中 80% 会发生的错误：

```sh
Native method not found
```

比如我今天遇到了很多次这个错误:

```
UnsatisfiedLinkError: Native method not found: github.jp1017.hellojni.MainActivity.dynamicRegFromJni:()Ljava/lang/String;
```

该问题的发生原因很多，我这里报错是因为自己粗心，代码 **大小写** 写错，**类名** 写错等，所以这里提醒大家对待你的代码要像你的女朋友一样，千万不可马虎大意呀。

# 总结

上面的代码我给上传到了 github: [https://github.com/jp1017/HelloJni](https://github.com/jp1017/HelloJni)

jni 开发告一段落了，这几天总结了很多坑，详情请点击这里：

[Android Studio 下 jni 开发填坑记](https://github.com/jp1017/Android-Collection/issues/10)

安卓开源库收集整理中，欢迎 PR， star，地址：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！