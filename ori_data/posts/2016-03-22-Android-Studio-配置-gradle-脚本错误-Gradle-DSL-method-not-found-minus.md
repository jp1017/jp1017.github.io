title: "Android Studio 配置 gradle 脚本错误:Gradle DSL method not found: 'minus()'"
date: 2016-03-22 11:00:55
categories: Android Studio
tags: [Android Studio,gradle]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)


这个坑的出现是今天打包 jni 的时候，出现的，错误：

```
Error: Gradle DSL method not found: 'minus()'"
```

方法 `minus()` 未找到，也定位不到相关代码，查看 gradle 配置发现了个问题：

![minus](http://7xlah4.com1.z0.glb.clouddn.com/20160322110607.jpg)

<!--more-->

图中，红框里的代码颜色不一样，哈哈哈，minus，减号，无疑了，就是这里的问题。

**解决方法**：修改 `arm64-v8a` 为 `arm64_v8a`，减号修改为下划线，搞定。



分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！