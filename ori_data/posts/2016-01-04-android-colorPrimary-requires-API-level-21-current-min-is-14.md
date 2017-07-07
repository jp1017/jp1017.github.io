title: 'android:colorPrimary requires API level 21 (current min is 14)'
date: 2016-01-04 22:02:26
categories:
tags:
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

今天用了下toolbar，需要关注三点：style, layout, activity，详细点这里:[android：ToolBar详解（手把手教程）](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1118/2006.html)

在style遇到了个问题，下面是最开始的style文件：

```xml
    <style name="AppTheme.Base" parent="Theme.AppCompat.Light">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
        <item name="android:colorPrimary">@color/primary</item>
        <item name="android:colorPrimaryDark">@color/primary_dark</item>
        <item name="android:windowBackground">@color/comment_bg</item>
    </style>
```

<!--more-->

然后报错：

```java
android:colorPrimary requires API level 21 (current min is 14)
```

stackflow给出解决方案，[Material design backward compatibility android:colorAccent requires API level 21 when using appcompat7](https://stackoverflow.com/questions/26947276/material-design-backward-compatibility-androidcoloraccent-requires-api-level-21)

修改成下面就可以：

```xml
    <style name="AppTheme.Base" parent="Theme.AppCompat.Light">
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
        <item name="colorPrimary">@color/primary</item>
        <item name="colorPrimaryDark">@color/primary_dark</item>
        <item name="android:windowBackground">@color/comment_bg</item>
    </style>
```

colorPrimary，colorPrimaryDark都是5.0的md开始有的。因此直接`android:colorPrimary`是错误的，当然这个可以放到values-v21/style.xml下，这个是5.0以上读取的style

如有不妥，请指正，谢谢！


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！