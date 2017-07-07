title: 微信热修复tinker及tinker server快速接入
date: 2016-11-25 19:44:08
categories: [Android]
tags: [tinker]
---

博客：	[安卓之家](http://jp1017.github.io/)
掘金：	[jp1017](http://gold.xitu.io/user/5675e9d560b2f42a127e4916)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![来自unsplash的美图](https://images.unsplash.com/photo-1479030160180-b1860951d696?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max&s=58049462f876ab4785ae452fcce306ce)

当前热修复方案很多，今天研究了下微信的tinker，使用效果还是不错的，配合tinker server服用更佳。下面介绍两者的使用，以便大家快速接入。

<img src="http://7xlah4.com1.z0.glb.clouddn.com/2016-11-25-16-23-56-729_tinker.sample..png" width="320"/> <img src="http://7xlah4.com1.z0.glb.clouddn.com/2016-11-25-18-57-32-149_tinker.sample..png" width="320"/>


# tinker 接入指南

## 安装tinker gradle插件

1 在项目的build.gradle中, 添加tinker-patch-gradle-plugin的依赖

```
buildscript {
    dependencies {
        classpath ('com.tencent.tinker:tinker-patch-gradle-plugin:1.7.5')
    }
}
```

<!--more-->

2 然后在app的gradle文件app/build.gradle，我们需要添加tinker的库依赖以及apply tinker的gradle插件.

//apply tinker插件
>apply plugin: 'com.tencent.tinker.patch'

```
dependencies {
    //可选，用于生成application类
    provided('com.tencent.tinker:tinker-android-anno:1.7.5')
    //tinker的核心库
    compile('com.tencent.tinker:tinker-android-lib:1.7.5')
}
```

## 配置tinker task

配置基础包, tinkerid, dexMode等，详见gradle配置: [tinker task 配置](https://github.com/jp1017/tinker-sample-android/blob/master/app/build.gradle)

我做了如下修改:

1 修改tinkerid为版本号, 跳过了需要commit一次的坑:smile:
```
def getTinkerIdValue() {
    //版本作为id
    return android.defaultConfig.versionName
}
```
2 移动备份文件到/tinker/bakApk/下, 防止clean掉基础包文件

3 重命名备份文件, 比如`base-app-debug-v1.0.1-2016-1125.apk`, 当然自动生成的是`app-debug-v1.0.1-2016-1125.apk`, 需要手动添加前缀作为基础包, 后面多次编译不会把基础包覆盖掉, 也不会像官方demo里那样以秒命名产生很多文件...

4 修改tinker message 为 `I am the patch apk-v版本号`

5 修改patchVersion为版本号, 这个在tinker server需要

```
-configField("patchVersion", "1.0.7")
+configField("patchVersion", android.defaultConfig.versionName)
```


**注意** 里面有些修改的地方, 包名修改为你的包名等, 我用todo做了标记

## 生成 Application

如果你有Application类, 那么需要自定义一个DefaultApplicationLike, 让tinker帮你生成Application

正如项目里的`public class SampleApplicationLike extends DefaultApplicationLike {`

并对类添加注解, 比如添加如下注解:

```
@DefaultLifeCycle(
application = "tinker.sample.android.app.SampleApplication",             //application name to generate
flags = ShareConstants.TINKER_ENABLE_ALL)
```

编译后, 会生成一个SampleApplication, 用这个作为你的Application, 写入清单文件

好了, tinker到这里就配置好了, 下面开始打补丁

## 打补丁包

1 命令行

打debug补丁: `./gradlew tinkerPatchDebug`

打release补丁: `./gradlew tinkerReleaseDebug`

这里需要注意, 命令在linux和mac下最好是`./gradlew`, 意思是当前项目的gradlew, 如果写成`gradlew`可以会去下载gradle等, 因为那是全局的, 比如AS2.2.2带的版本是2.14.1
而我现在的是最新版本3.2.1, 可输入`./gradlew -v` 和 `gradlew -v`　查看
而windows就可以是`gradlew`

**注意** debug和release配置的基包不同, 和他们一一对应, 另外, release还需要配置mapping文件.

2 双击对应task

就是去gradle projects里找到对应task, 双击执行就可以, 如下图:

![gradle](http://7xlah4.com1.z0.glb.clouddn.com/20161125%20125123tinker%20gradle.png)

比如, 打debug补丁, 双击`tinkerPatchDebug`就可以了

下一次打补丁时就可以从快捷栏选择，然后点击右侧运行, 如下图:

![patch](http://7xlah4.com1.z0.glb.clouddn.com/20161125130855tinker1.png)

## 安装及卸载补丁

### 加载补丁
第二个参数是补丁包存放路径, 名称任意, 可以不以 `.bak` 结尾

>TinkerInstaller.onReceiveUpgradePatch(getApplicationContext(), patchPath);

还可以自定义加载成功等交互, 请参考 `SampleResultService`, 别忘记添加进清单

### 清除补丁

当补丁出现异常或者某些情况，我们可能希望清空全部补丁，调用方法为：

>Tinker.with(context).cleanPatch();

当然我们也可以选择卸载某个版本的补丁文件：

>Tinker.with(context).cleanPatchByVersion();

在升级版本时我们也无须手动去清除补丁，框架已经为我们做了这件事情。需要注意的是，在补丁已经加载的前提下清除补丁，可能会引起crash。这个时候更好重启一下所有的进程。

### 查看补丁是否加载

>boolean isPatched = tinker.isTinkerLoaded();

# tinker server　接入及使用

tinker server　提供tinker补丁包下发及监控等, 使用也是很简单

## gradle 配置环境

1 gradle远程仓库依赖jcenter

```
repositories {
    jcenter()
}
```

2 再添加sdk库的dependencies依赖:
```
dependencies {
    compile("com.tencent.tinker:tinker-server-android:0.3.0")
}
```

3 在 TinkerPatch 平台中得到的 AppKey 以及 AppVersion，将他们写入 buildConfig 中:

比如:

```
buildConfigField "String", "APP_KEY", "\"f938475486f91936\""
buildConfigField "String", "APP_VERSION",  "\"3.0.0\""
```
平台链接: [tinkerpatch.com](http://tinkerpatch.com/)

新增app后可以得到AppKey, 至于AppVersion, 就是补丁的版本, 我这里都是版本号, 可以参考这个issue: [关于AppVersion问题](https://github.com/simpleton/tinker_server_client/issues/2)

4 清单配置网络及sd卡读写权限

```
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```


## 代码初始化

>TinkerServerManager.installTinkerServer(getApplication(), Tinker.with(getApplication()), 3);

后面的3表示每隔3小时请求一次服务器, 检查是否有更新包

## 请求更新补丁

1 主动请求更新

> TinkerServerManager.checkTinkerUpdate(true);

2 获取新增参数

>TinkerServerManager.getDynamicConfig(new ConfigRequestCallback() {...

下面来一个该demo的tinker server 截图:

![tinker_server](http://7xlah4.com1.z0.glb.clouddn.com/20161125183501tinker_server.png)

# 参考

更多使用及问题请参考官方文档:

[Tinker -- 微信Android热补丁方案](https://github.com/Tencent/tinker/wiki)

[Tinker 接入指南](https://github.com/Tencent/tinker/wiki/Tinker-%E6%8E%A5%E5%85%A5%E6%8C%87%E5%8D%97)

[Tinker API概览](https://github.com/Tencent/tinker/wiki/Tinker-API%E6%A6%82%E8%A7%88)

[Tinker 自定义扩展](https://github.com/Tencent/tinker/wiki/Tinker-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%89%A9%E5%B1%95)

[Tinker 常见问题](https://github.com/Tencent/tinker/wiki/Tinker-%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

[Tinker Platform 平台使用文档](http://tinkerpatch.com/Docs/api)

代码就是Tinker官方示例，我做了一些修改，可点击这里查看: [https://github.com/jp1017/tinker-sample-android](https://github.com/jp1017/tinker-sample-android)




最后，非常感谢您的阅读，有任何疑问，可以后面评论，谢谢！

神奇的安卓开发网站：[http://androidcat.com/](http://androidcat.com/)

安卓开源库收集整理：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！