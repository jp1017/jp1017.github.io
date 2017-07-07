title: Android Studio中常用插件及浅释
date: 2015-12-20 14:24:17
categories: Android Studio
tags: Android Studio
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![美景](https://drscdn.500px.org/photo/105625329/m%3D2048/5bfda86d1b46da4fdd8857fe2bb1f0ef)

插件可以来这个仓库查找：[Android Studio Plugins](http://plugins.jetbrains.com/category/?androidstudio&category_id=all)

这里给出几个平时常用到的as插件，方便我们的开发。点击标题就直接可以进入插件的github源码查看。

如何自己开发插件，请参考鸿洋大神的博客：[自己编写Android Studio插件 别停留在用的程度了](https://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650820316&idx=1&sn=49d4e6b68b114a2e8a8e88f3d1b0cd9e&scene=1&srcid=0601lzX1UwNhWeitdRa3Bmfx#rd)

# [.ignore](https://github.com/hsz/idea-gitignore)
---

as第一大插件，版本控制必备，.gitignore内容写法，来这里看看：[git使用之二——.gitignore文件详解 ](http://jp1017.gitcafe.io/2015/12/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%BA%8C%E2%80%94%E2%80%94-gitignore%E6%96%87%E4%BB%B6%E8%AF%A6%E8%A7%A3/)

<!--more-->

当然，还可以按照作者的指南来操作咯，哈哈。香赞。

# [ECTranslation](https://github.com/Skykai521/ECTranslation)
---

可以将英文翻译为中文

<img src="https://raw.githubusercontent.com/Skykai521/ECTranslation/master/img/translation_img.png" width="600"/>

# [eventbus3-intellij-plugin](https://github.com/kgmyshin/eventbus3-intellij-plugin)
---

EventBus3 事件管理

<img src="https://raw.githubusercontent.com/kgmyshin/eventbus3-intellij-plugin/master/art/cap.gif" width="600"/>

# [eventbus-intellij-plugin](https://github.com/kgmyshin/eventbus-intellij-plugin)
---

EventBus 事件管理

<img src="https://raw.githubusercontent.com/kgmyshin/eventbus-intellij-plugin/master/art/cap.gif" width="600"/>

# [PermissionsDispatcher](https://github.com/shiraji/permissions-dispatcher-plugin)
---

IntelliJ plugin for supporting PermissionsDispatcher

<img src="https://raw.githubusercontent.com/shiraji/permissions-dispatcher-plugin/master/website/images/pd.gif" width="600"/>

# [Android Methods Count](http://www.methodscount.com/plugins)
---

展示安卓依赖库里方法数，支持的仓库包括：`Maven Central`, `jCenter`, `JitPack`

![1](http://www.methodscount.com/images/methods-count-plugin-1.png)
![2](http://www.methodscount.com/images/methods-count-plugin-2.png)

# [Genymotion](https://www.genymotion.com/#!/download)
---

速度快，运行流畅的安卓模拟器

来这里吧，专门给你准备的：[Eclipse和Android Studio下安装Genymotion模拟器插件](http://www.ebaina.com/bbs/forum.php?mod=viewthread&tid=8418&extra=page%3D1)

# [ButterKnife Zelezny](https://github.com/avast/android-butterknife-zelezny)
---

Android Studio plug-in for generating ButterKnife injections from selected layout XML.

插件下载如下：

![1](http://7xlah4.com1.z0.glb.clouddn.com/20150928202523.png)

要配合一个库com.jakewharton:butterknife:7.0.1使用，把该库添加到build.gradle脚本里即可。

![2](http://7xlah4.com1.z0.glb.clouddn.com/20150928194919.png)

使用如下：

比如我们在activity的布局里定义了一个文本框，三个按钮，共四个id，然后我们来注解一下：鼠标放setContentView(R.layout.activity_main);下的activity_main任意位置，alt+insert，然后注解：

![3](http://7xlah4.com1.z0.glb.clouddn.com/20150928194920.gif)

# [Android Studio Prettify](https://github.com/Haehnchen/idea-android-studio-plugin)
---

Android Studio plugin with some tools and usability improvements, Generator for inflater and activity setContentView view variables.

如果你布局里有多个id，在activity里findViewById()会手写很多次，即使有ide辅助，但是还是略慢，这个插件就来释放你双手，作者的例子：

![Prettify](http://7xlah4.com1.z0.glb.clouddn.com/2015101834.gif)

![Prettify2](http://7xlah4.com1.z0.glb.clouddn.com/2015101833.png)

当然如果快速注解的话就用上面的ButterKnife咯。

# [ADB WIFI](https://github.com/layerlre/ADBWIFI)
---

通过wifi调试你的安卓app，释放usb数据线，实现调试无处不在。。。

使用方法：
确保你的手机和电脑在同一wifi下，首先用usb连接手机很电脑，第一次还是需要的，后面连接完成后可以拔掉。然后连接他们， Tools → Android → ADB WIFI → ADB USB to WIFI 成功后会在右上角有个对话框，提示成功。然后拔掉你的数据线，调试无处不在模式开启。。。

![连接](http://7xlah4.com1.z0.glb.clouddn.com/201510211.jpg)

# [GsonFormat](https://github.com/zzz40500/GsonFormat)
---

根据JSONObject格式的字符串,自动生成实体类参数。
最新的1.2.0版本新增处女座模式 →_→ 是不是很贴心！

 处女座模式就是给json每个key都可以配置生成的filedName，可能因为服务端的原因，或者历史的原因，导致服务器返回的字段名诡异，或是歧义的缩写。这个在之前的版本是不支持这个。

作者给出的例子：

有如下json数据：

```xml
{
    "name": "王五",
    "gender": "man",
    "age": 15,
    "height": "140cm",
}
```

生成实体类操作如下，win和linux下的快捷键是alt+insert

![GsonFormat](http://7xlah4.com1.z0.glb.clouddn.com/2015101831.gif)

# [LeakCanary](https://github.com/square/leakcanary)
---

良心企业Square最近刚开源的一个非常有用的工具，使用方法请看我的另一片文章：[Android Studio 插件之内存泄露检测LeakCanary使用](http://jp1017.gitcafe.io/2015/12/20/Android-Studio-%E6%8F%92%E4%BB%B6%E4%B9%8B%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E6%A3%80%E6%B5%8BLeakCanary%E4%BD%BF%E7%94%A8/)

# [codota](https://www.codota.com/)
---

该网站搜集了大量的代码，号称超过700W的代码实例。
它提供了chrome插件和as插件。

按照同样的方式安装codota插件之后，重启AS。使用快捷键ctrl + k，即可打开搜索界面，如果你的快捷键有冲突，随便打开一个界面，然后右键就可以看到Search Cotoda选项。

# [Android Code Generator](https://github.com/tmorcinek/android-codegenerator-plugin-intellij)
---

如果你的xml布局里有n个id，需要用findViewById找到的话，手动的话，很累，这个插件就是释放你的双手，轻轻一点，轻松生成代码，然后你复制粘贴到你的代码就ok，我们来看：

![Android Code Generator](http://7xlah4.com1.z0.glb.clouddn.com/2015101101.gif)

# [Android Postfix completion](https://github.com/takahirom/android-postfix-plugin)
---

该插件可以快速书写log、toast等代码

![log](http://7xlah4.com1.z0.glb.clouddn.com/20151011172328.png)

我们来具体操作：

![toast](http://7xlah4.com1.z0.glb.clouddn.com/2015101102.gif)

# [Android Selectors Generate](https://github.com/inmite/android-selector-chapek)
---

Android Studio plugin which automatically generates drawable selectors from appropriately named resources.

自动生成选择器，这玩意好用，很赞，但是要注意drawable下文件后缀哦，告诉美工小妹妹命名好哦，哈哈。

文件后缀是这样的：

![文件后缀](http://7xlah4.com1.z0.glb.clouddn.com/2015101850.png)

使用方法：

1 右击drawable文件夹：

![右击](http://7xlah4.com1.z0.glb.clouddn.com/2015101851.png)

2 选择Generate Android Selectors

![selectors](http://7xlah4.com1.z0.glb.clouddn.com/2015101852.png)

3 自动生成选择器

![选择器](http://7xlah4.com1.z0.glb.clouddn.com/2015101853.png)

# [Android File Grouping](https://github.com/dmytrodanylyk/folding-plugin)
---

去官网学习吧，用处不是很大，方便阅读。

# [FindBugs-IDEA](https://github.com/andrepdo/findbugs-idea/tree/master)
---

顾名思义，就是帮你找程序bug咯，自己研究去吧，给力，感恩作者。

# [Android Parcelable code generator](https://github.com/mcharmas/android-parcelable-intellij-plugin/)
---

安卓下，推荐用Parcelable来实现数据序列化，如果需要实现Serilizeable接口的，也有插件，[SerializableParcelableGenerator](https://github.com/bunnyblue/SerializableParcelableGenerator)

使用也很简单，进入要序列化的bean类里，windows，linux下直接快捷键alt+insert，mac下右键Generator, 可以看到有个选项Parcelable，然后直接点击，就序列化完成咯。

![Parcelable](http://7xlah4.com1.z0.glb.clouddn.com/20151230012.gif)

# [Android Drawable Importer](https://github.com/winterDroid/android-drawable-importer-intellij-plugin)
---

最常用的功能就是生成不同尺寸的图标，

我这里有个需求，美工妹妹要陪男朋友，然后只给我一套xxh的图标，那么这个工具就是来解放你们的，手把手的教：

![Drawable](http://7xlah4.com1.z0.glb.clouddn.com/20151230011.gif)

# [android-material-design-icon-generator-plugin](https://github.com/konifar/android-material-design-icon-generator-plugin)
---

This plugin help you to set material design icon to your project.

![material](http://7xlah4.com1.z0.glb.clouddn.com/20160216capture.gif)

# [AndroidStudioSuperPlugin](https://github.com/b2b2244424/AndroidStudioSuperPlugin)
---

这个是今天（2016年植树节）早上发现的，是几个插件的集成，包括：

+ Android Studio Prettify
+ GsonFormat
+ Android Code Generator
+ SelectorChapek
+ Android Parcelable Generator
+ folding-plugin
+ Lifecycle-Sorter

有了这个，可以删掉相关的插件咯，谢谢，哈哈哈

# [CodeGlance](https://github.com/Vektah/CodeGlance)
---

Intelij IDEA plugin for displaying a code mini-map similar to the one found in Sublime

![CodeGlance](http://7xlah4.com1.z0.glb.clouddn.com/20160317.png)


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！
