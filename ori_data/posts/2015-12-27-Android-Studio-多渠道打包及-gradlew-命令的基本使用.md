title: Android Studio 多渠道打包、自动版本号及 gradlew 命令的基本使用
date: 2015-12-27 19:16:07
categories: Android Studio
tags: [Android Studio,gradlew]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

# Android Studio 多渠道打包
---

Android Studio 真可谓神器，详细请点这里：[打造安卓开发航空母舰](https://github.com/jp1017/Android-Development-Aircraft-Carrier)

这里介绍其多渠道打包：

1 建立多渠道

这里介绍一种简单的，直接as操作：

直接上图咯，在项目结构你添加flavor就好了

![1](http://7xlah4.com1.z0.glb.clouddn.com/20151228161552.png)

确定后，项目会自动同步，完成后，渠道就配置好了。

<!--more-->

当然，也可以直接在gradle脚本里操作：

```gradle
android {
    productFlavors {
        dev {
            manifestPlaceholders = [channel: "dev"]
        }
        official {
            manifestPlaceholders = [channel: "official"]
        }
        xiaomi {
            manifestPlaceholders = [channel: "xiaomi"]
        }
        wandoujia {
            manifestPlaceholders = [channel: "wandoujia"]
        }
        "360" {
            manifestPlaceholders = [channel: "360"]
        }
    }
}
```

项目同步好后，会发现Build Variant会多了很多渠道

![variant](http://7xlah4.com1.z0.glb.clouddn.com/201512292204.gif)

2 打包
---

上面的各种variant，你需要选择一个,然后build和run的时候只会构建运行这一个variant，全部打包的话，这里采用命令行，我知道的as全部打包是需要签名的，大家有知道的不需要签名的方法麻烦告知。

```gradle
gradlew build
```

这样就会把所有的包打好，每种渠道的debug和release版本都会打包。

![2](http://7xlah4.com1.z0.glb.clouddn.com/20151228213639.png)

上面几个包一共用了半分钟多点。这个包的文件名带时间版本号等信息是怎么来的呢，当然这得益于gradle强大的功能，后面会讲到的。

而as打包需要就是build --> Generated signed APK，这样你选择下就可以把所有渠道打包。

最近发现了个[**下一代Android打包工具**](https://github.com/mcxiaoke/packer-ng-plugin)，1000个渠道包只需要5秒钟

# 自动版本号
---

昨天 2016.03.02 刚发现的好东西，[更优雅的 Android 发布自动版本号方案](http://www.race604.com/android-auto-version/)

这里简单总结下，配合 git 获取软件版本号和版本名

+ 版本号 versionCode

使用 Git 中 commit 的数量来作为版本号——versionCode

```sh
def cmd = 'git rev-list HEAD --first-parent --count'  
def gitVersion = cmd.execute().text.trim().toInteger()

android {  
  defaultConfig {
    versionCode gitVersion
  }
}
```

+ 版本名 versionName

使用 git describe，获取从当期 commit 到距离它最近的 tag 的描述。默认都是 annoted tag，如果要指所有的类型的 tag 的话，就加 --tags 参数。

```sh
def cmd = 'git describe --tags'  
def version = cmd.execute().text.trim()

android {  
  defaultConfig {
    versionName version
  }
}
```

最终的脚本详见后文。

# gradlew 命令
---

gradlew 是什么东西呢，和gradle貌似不大一样，肯定有关系。没错，他就是 gradle wrapper，意思是gradle的一个包装，大家可以理解为在这个项目本地就封装了gradle，比如我的项目是HelloWord, 在HelloWord/gradle/wrapper/gralde-wrapper.properties文件中声明了它指向的目录和版本，比如我的内容是：

```sh
#Thu Dec 28 20:02:55 CST 2015
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip
```

如果你项目没有该文件的话，那么gradlew会到~/.gradle/wrapper/dists/gradle-2.10-all下寻找，或者你新建给文件，内容复制到里面。其实你会发现是同一个目录咯。里面会有个随机数的文件夹，里面就是gradle，只要下载成功即可用grdlew wrapper的命令代替全局的gradle命令。

常用命令如下：(linux下是./gradlew，该脚本在项目下，windows直接gradlew即可)

+ ./gradlew -v 版本号，首次运行，没有gradle的要下载的哦。

![gradlev](http://7xlah4.com1.z0.glb.clouddn.com/20151228213320.png)

+ ./gradlew clean 删除HelloWord/app目录下的build文件夹

+ ./gradlew build 检查依赖并编译打包

这里注意的是 ./gradlew build 命令把debug、release环境的包都打出来，生成的包在目录HelloWord/app/build/outputs/apk/下。如果正式发布只需要打release的包，该怎么办呢，下面介绍一个很有用的命令 **assemble**, 如

- ./gradlew assembleDebug 编译并打Debug包

- ./gradlew assemblexiaomiDebug 编译并打xiaomi的debug包，其他类似

- ./gradlew assembleRelease 编译并打Release的包

- ./gradlew assemblexiaomiRelease 编译并打xiaomi的Release包，其他类似

- ./gradlew installRelease Release模式打包并安装

- ./gradlew uninstallRelease 卸载Release模式包

# 补充
---

1 `gradlew build 和 gradle build 有区别吗?`

>使用gradle wrapper是gradle官方推荐的build方式，而gradlew正是运行了wrapper task之后生成的（运行wrapper task是Android Studio自动做的）。使用gralde wrapper的一个好处就是每个项目可以依赖不同版本的gradle，构建的时候gradle wrapper会帮你自动下载所依赖的版本的gradle。而如果你使用gradle build的话，同时你又有多个项目使用不同版本的gradle，那就需要你手动在自己的机器上配置多个版本的gradle，稍微麻烦一些

2 自定义apk包名

gradle脚本大法好：

```sh
def releaseTime() {
    return new Date().format("yyyy-MM-dd", TimeZone.getTimeZone("UTC"))
}

def gitVersionCode() {
    def cmd = 'git rev-list HEAD --first-parent --count'
    cmd.execute().text.trim().toInteger()
}

def gitVersionTag() {
    def cmd = 'git describe --tags'
    def version = cmd.execute().text.trim()

    def pattern = "-(\\d+)-g"
    def matcher = version =~ pattern

    if (matcher) {
        version = version.substring(0, matcher.start()) + "." + matcher[0][1]
    } else {
        version = version + ".0"
    }

    return version
}
    
    
    
//自定义apk安装包名
applicationVariants.all { variant ->
    variant.mergedFlavor.versionCode = gitVersionCode()
    variant.mergedFlavor.versionName = gitVersionTag()
    variant.outputs.each { output ->
        output.outputFile = new File(
                output.outputFile.parent + "/${variant.buildType.name}",
                "HelloWord-${variant.buildType.name}-v${variant.versionName}-${variant.productFlavors[0].name}-${releaseTime()}.apk".toLowerCase())
    }
}
```

参考文章：
[Android Studio系列教程五--Gradle命令详解与导入第三方包](http://stormzhang.com/devtools/2015/01/05/android-studio-tutorial5/)
[Android打包的那些事](http://www.jayfeng.com/2015/11/07/Android%E6%89%93%E5%8C%85%E7%9A%84%E9%82%A3%E4%BA%9B%E4%BA%8B/)
[更优雅的 Android 发布自动版本号方案](http://www.race604.com/android-auto-version/)


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！