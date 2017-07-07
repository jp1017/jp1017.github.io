title: 安卓开发问题之Unable to instantiate application com.android.tools.fd.runtime.BootstrapApplication
date: 2016-03-11 20:06:32
categories: Android
tags: Android
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

# 问题
---

这个问题出现在程序 Launcher3 运行中，系统端需要我这里修改 Launcher3 的一些东西，修改了给他，出现如下错误：

```java
--------- beginning of /dev/log/main
I/Process ( 1706): Sending signal. PID: 1706 SIG: 9
--------- beginning of /dev/log/system
I/ActivityManager(  731): Process com.inst.launcher3 (pid 1706) has died.
W/art     (  444): Could not get current activity
I/ActivityManager(  731): Start proc com.inst.launcher3 for activity com.inst.launcher3/.Launcher: pid=1729 uid=10034 gids={50034,}
I/art     ( 1729): Failed to open oat file from /system/priv-app/launcher3_INST.odex or /data/dalvik-cache/system@priv-app@launche.
E/art     ( 1729): Failed to open file: /data/dalvik-cache/system@priv-app@launcher3_INST.apk@classes.dex
E/art     ( 1729): Failed to open locked oat file: /data/dalvik-cache/system@priv-app@launcher3_INST.apk@classes.dex
W/art     ( 1729): Failed to open dex file: /system/priv-app/launcher3_INST.apk
D/AndroidRuntime( 1729): Shutting down VM
E/AndroidRuntime( 1729): FATAL EXCEPTION: main
E/AndroidRuntime( 1729): Process: com.inst.launcher3, PID: 1729
E/AndroidRuntime( 1729): java.lang.RuntimeException: Unable to instantiate application com.android.tools.fd.runtime.BootstrapApplication
```

<!--more-->

```java
E/AndroidRuntime( 1729):        at android.app.LoadedApk.makeApplication(LoadedApk.java:516)
E/AndroidRuntime( 1729):        at android.app.ActivityThread.handleBindApplication(ActivityThread.java:4317)
E/AndroidRuntime( 1729):        at android.app.ActivityThread.access$1500(ActivityThread.java:135)
E/AndroidRuntime( 1729):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1256)
E/AndroidRuntime( 1729):        at android.os.Handler.dispatchMessage(Handler.java:102)
E/AndroidRuntime( 1729):        at android.os.Looper.loop(Looper.java:136)
E/AndroidRuntime( 1729):        at android.app.ActivityThread.main(ActivityThread.java:5017)
E/AndroidRuntime( 1729):        at java.lang.reflect.Method.invoke(Native Method)
E/AndroidRuntime( 1729):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:779)
E/AndroidRuntime( 1729):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:595)
E/AndroidRuntime( 1729): Caused by: java.lang.ClassNotFoundException: Didn't find class "com.android.tools.fd.runtime.BootstrapApp]
E/AndroidRuntime( 1729):        at dalvik.system.BaseDexClassLoader.findClass(BaseDexClassLoader.java:56)
E/AndroidRuntime( 1729):        at java.lang.ClassLoader.loadClass(ClassLoader.java:511)
E/AndroidRuntime( 1729):        at java.lang.ClassLoader.loadClass(ClassLoader.java:469)
E/AndroidRuntime( 1729):        at android.app.Instrumentation.newApplication(Instrumentation.java:975)
E/AndroidRuntime( 1729):        at android.app.LoadedApk.makeApplication(LoadedApk.java:511)
E/AndroidRuntime( 1729):        ... 9 more
E/AndroidRuntime( 1729):        Suppressed: java.io.IOException: Unable to open dex file: /system/priv-app/launcher3_INST.apk
E/AndroidRuntime( 1729):                at dalvik.system.DexFile.openDexFileNative(Native Method)
E/AndroidRuntime( 1729):                at dalvik.system.DexFile.openDexFile(DexFile.java:296)
E/AndroidRuntime( 1729):                at dalvik.system.DexFile.<init>(DexFile.java:80)
E/AndroidRuntime( 1729):                at dalvik.system.DexFile.<init>(DexFile.java:59)
E/AndroidRuntime( 1729):                at dalvik.system.DexPathList.loadDexFile(DexPathList.java:263)
E/AndroidRuntime( 1729):                at dalvik.system.DexPathList.makeDexElements(DexPathList.java:230)
E/AndroidRuntime( 1729):                at dalvik.system.DexPathList.<init>(DexPathList.java:112)
E/AndroidRuntime( 1729):                at dalvik.system.BaseDexClassLoader.<init>(BaseDexClassLoader.java:48)
E/AndroidRuntime( 1729):                at dalvik.system.PathClassLoader.<init>(PathClassLoader.java:65)
E/AndroidRuntime( 1729):                at android.app.ApplicationLoaders.getClassLoader(ApplicationLoaders.java:57)
E/AndroidRuntime( 1729):                at android.app.LoadedApk.getClassLoader(LoadedApk.java:326)
E/AndroidRuntime( 1729):                at android.app.LoadedApk.makeApplication(LoadedApk.java:508)
E/AndroidRuntime( 1729):                ... 9 more
E/AndroidRuntime( 1729):        Suppressed: java.lang.ClassNotFoundException: com.android.tools.fd.runtime.BootstrapApplication
E/AndroidRuntime( 1729):                at java.lang.Class.classForName(Native Method)
E/AndroidRuntime( 1729):                at java.lang.BootClassLoader.findClass(ClassLoader.java:781)
E/AndroidRuntime( 1729):                at java.lang.BootClassLoader.loadClass(ClassLoader.java:841)
E/AndroidRuntime( 1729):                at java.lang.ClassLoader.loadClass(ClassLoader.java:504)
E/AndroidRuntime( 1729):                ... 12 more
E/AndroidRuntime( 1729):        Caused by: java.lang.NoClassDefFoundError: Class "Lcom/android/tools/fd/runtime/BootstrapApplicatid
E/AndroidRuntime( 1729):                ... 16 more
W/ActivityManager(  731):   Force finishing activity com.inst.launcher3/.Launcher
W/ActivityManager(  731): Activity pause timeout for ActivityRecord{64d7f5f8 u0 com.inst.launcher3/.Launcher t16 f}
```

从错误日志里可以看出是：运行时错误，应用不能实例化， google 了下，原来遇到此问题的还不少呢，

[Unable to instantiate application com.android.tools.fd.runtime.BootstrapApplication ?Android](https://stackoverflow.com/questions/33967703/unable-to-instantiate-application-com-android-tools-fd-runtime-bootstrapapplicat)

# 解决
---

stackflow上给出的三种解决方法：

**1/3**:

Changing:

`classpath 'com.android.tools.build:gradle:2.0.0-alpha1'`
By:

`classpath 'com.android.tools.build:gradle:1.2.3'`
**2/3**:

Changing:

`buildToolsVersion '23.0.2'`
By:

`buildToolsVersion "21.1.2"`
**3/3**: (in <project folder>/.idea/gradle.xml)

And:

`<option name="gradleHome" value="$APPLICATION_HOME_DIR$/gradle/gradle-2.8" />`
By:

`<option name="gradleHome" value="$APPLICATION_HOME_DIR$/gradle/gradle-2.4" />`


我只用了第一个方法就搞定了：
Changing:

`classpath 'com.android.tools.build:gradle:2.0.0-beta5'`
By:

`classpath 'com.android.tools.build:gradle:1.5.0'`

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！