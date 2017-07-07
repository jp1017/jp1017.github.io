title: Activity A 调用Activity B 里的方法探索
date: 2015-12-18 21:33:34
categories: Android
tags: [Android, Activity]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

问题：Activity A 调用Activity B 里的方法探索

# 设置Activity B 里的方法为static
---

如果被调用的方法不是很复杂，可以直接设置为static，那么该方法属于类，而不是属于对象，这样可以直接Activity B.方法调用。

这里要注意，在activity里，里面的各种View不要设置为static。

<!--more-->

# 通过一个公有static的变量传递类引用
---

声明一个类BFactory，里面有静态变量public static Bctivity b；在B中调用 BFactory.b = this;

这样在A中就可以直接调用BFactory.b.function();了。

您有更好的方法，还请告诉我，谢谢！


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！