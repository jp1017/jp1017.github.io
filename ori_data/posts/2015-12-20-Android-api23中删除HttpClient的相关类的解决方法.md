title: Android api23中删除HttpClient的相关类的解决方法
date: 2015-12-20 16:28:40
categories: Android
tags: [Android,HttpClient]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

原文链接请点击这里查看：[原文链接](http://jp1017.gitcafe.io/2015/09/08/Android-api23%E4%B8%AD%E5%88%A0%E9%99%A4HttpClient%E7%9A%84%E7%9B%B8%E5%85%B3%E7%B1%BB%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)

# 由来
---

前几天找到了个安卓开发的mvp框架：[FastAndroid](https://github.com/huntermr/FastAndroid)，今天用greendao重新生成了数据库，详细过程见这里：

[点我就可以查看](http://jp1017.gitcafe.io/2015/09/08/Android-SQLite-ORM%E6%A1%86%E6%9E%B6greenDAO%E5%9C%A8Android%20Studio%E4%B8%AD%E7%9A%84%E9%85%8D%E7%BD%AE%E4%B8%8E%E4%BD%BF%E7%94%A8/)

<!--more-->

我把api升级到23后，编译发现在NetCenter.java中有几个错误，如下图所示：

![错误提示](http://7xlah4.com1.z0.glb.clouddn.com/2015090801.png)

在api23的源文件里确实找不到这几个类，在api22上使用的也是给加了删除线，也就是这个早晚要费，没想到来的这么快。。。

既然问题来了，就要解决，毕竟问题就是答案啊。

# 解决方法

1 把api降级，这对于我是不能忍受的

2 既然废除了，总会留下痕迹，果然，在sdk里留给了我们一个jar包

![jar包所在目录](http://7xlah4.com1.z0.glb.clouddn.com/2015090802.png)

只要把该jar包导入就ok了，最好同步或者clean下，哈哈！

3 发现了新东西，gradle脚本里添加：

```gradle
android {
    useLibrary "org.apache.http.legacy"
}
```


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！