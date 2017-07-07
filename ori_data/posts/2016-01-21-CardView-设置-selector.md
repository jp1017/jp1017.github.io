title: CardView 设置 selector
date: 2016-01-21 14:43:00
categories: Android
tags: [Android,CardView]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

# 问题来源
---

最近用了下RecyclerView,很好用，每个item我设置的是CardView包含一个TextView,

```xml
<android.support.v7.widget.CardView
    xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="8dp"
    android:id="@+id/cv_item"
    card_view:cardCornerRadius="4dp">

    <TextView
        android:id="@+id/text_view"
        android:padding="20dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</android.support.v7.widget.CardView>
```

这样就可以显示cardview了，问题是给item添加一个selector

<!--more-->

# 解决

1 类似Button直接在drawable下新建个card_selector.xml布局，然后设置CardView背景：

```xml
android:background="@drawable/card_selector"
```

然，，没有什么用。。。

2 google了下，我来到了这里:[Android-L CardView Visual Touch Feedback](https://stackoverflow.com/questions/24475150/android-l-cardview-visual-touch-feedback)

给CardView添加个前景：

```xml
android:foreground="?android:attr/selectableItemBackground"
```

这样就可以了，在5.0以上的设备上有点击有波纹效果，5.0以下无波纹，只有前景色变化

3 自定义CardView前景

[Android-L CardView Visual Touch Feedback](http://stackoverflow.com/a/30192562/5188560) 也给出了方法，分为5.0之前和之后两种设置，因为5.0之前没有ripple，所以5.0之前采用[inset](https://developer.android.com/reference/android/graphics/drawable/InsetDrawable.html) 代替。

+ 设置CardView自定义的前景：

```xml
android:foreground="@drawable/card_foreground"
```

+ 5.0之后

`drawable-v21/card_foreground.xml`

```xml
<ripple xmlns:android="http://schemas.android.com/apk/res/android" 
        android:color="#20000000"
        android:drawable="@drawable/card_foreground_selector" />
```
        
`drawable-v21/card_foreground_selector.xml`

```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true">
        <shape android:shape="rectangle">
            <solid android:color="#18ffc400"/>
        </shape>
    </item>
    <item android:state_focused="true" android:state_enabled="true">
        <shape android:shape="rectangle">
            <solid android:color="#0f000000"/>
        </shape>
    </item>
</selector>
```

效果：

![ripple](http://7xlah4.com1.z0.glb.clouddn.com/201601215.0ripple.gif)

+ 5.0之前

`drawable/card_foreground.xml`

```xml
<inset xmlns:android="http://schemas.android.com/apk/res/android" 
    android:drawable="@drawable/card_foreground_selector"
    android:insetLeft="2dp"
    android:insetRight="2dp"
    android:insetTop="4dp"
    android:insetBottom="4dp"/>
```

`drawable/card_foreground_selector.xml`

```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true">
        <shape android:shape="rectangle">
            <solid android:color="#1838f508"/>
            <corners android:radius="@dimen/card_radius" />
        </shape>
    </item>
    <item android:state_focused="true" android:state_enabled="true">
        <shape android:shape="rectangle">
            <solid android:color="#0f000000"/>
            <corners android:radius="@dimen/card_radius" />
        </shape>
    </item>
</selector>
```

效果：

![4.4](http://7xlah4.com1.z0.glb.clouddn.com/201601214.4.gif)

+ 直接设置selector也是可以的：

```xml
android:foreground="@drawable/card_foreground_selector"
```

这样的话，5.0之前和之后效果一样，并不影响5.0之前的效果，但是5.0之后的ripple效果木有(木有设置，当然木有→_→)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！