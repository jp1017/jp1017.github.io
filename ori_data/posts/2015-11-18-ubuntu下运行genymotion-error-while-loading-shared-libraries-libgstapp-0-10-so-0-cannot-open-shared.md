title: 'ubuntu下运行genymotion: error while loading shared libraries: libgstapp-0.10.so.0: cannot open shared'
date: 2015-11-18 19:03:25
categories: ubuntu
tags: [ubuntu,genymotion]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

# 问题
---

有半个月没有用模拟器了，百度地图需要真机调试，今天来了一发genymotion，没想到给我来这么一出：

```bash
genymotion: error while loading shared libraries: libgstapp-0.10.so.0: cannot open shared object file: No such file or directory
```

<!--more-->

就是这样子：

![genymotion](http://7xlah4.com1.z0.glb.clouddn.com/20151105genymotion.png)

# 解决
---

既然没有这个库，就去安装呗，去新立得下搜索了下，没有，然后google了下，在[Stack Overflow](https://stackoverflow.com/questions/26869830/genymotion-error-in-ubuntu-14-04-lts)下找到了需要下面的两个库：

>libgstreamer0.10-dev 
> libgstreamer-plugins-base0.10-dev

搜索安装可以了，中间需要很多依赖，问题不大，装上，搞定！


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！