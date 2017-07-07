title: 安卓 jni 开发 —— Android Studio 打包 so 的坑
date: 2016-03-18 15:06:12
categories: Android Studio
tags: [Android Studio,jni]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

>安卓开发坑无限
我心依旧，不变
向前

# jni 开发的坑
---

这两天要搞安卓下的**串口读写**，这块涉及到了 jni 开发，我找了两个项目，导入 as 修改后上传到了 github

[**AndroidSerialPortSample**](https://github.com/jp1017/AndroidSerialPortSample)

[**AndroidSerialPort**](https://github.com/jp1017/AndroidSerialPort)

jni 坑尤其多，编译器不好选择，windows 下用 vs，as，vs双管齐下可好？卡死了。

mbp 用户该如何搞，虚拟机开 windows，呵呵。

有个大神就是用 atom 开发 jni，然后 gdb 调试，这才是真爱啊，膜拜下。

<!--more-->

# as 打包 so
---

关于 as 的打包，之前总结过：[**Android Studio 多渠道打包、自动版本号及 gradlew 命令的基本使用**](http://t.cn/RGda73p)

今天的问题主要是 jni 下的打包，遇到了几个坑。

1 **打包特定平台 so 到 apk 包**

开发多 jni 的同学，都知道，as 默认会把七个平台 —— `armeabi`, `armeabi-v7a`, `arm64-v8a`, `mips`, `mips64`, `x86`, `x86_64` 全部打到 apk 包里，拆开包就会在 libs 文件夹下看到各平台都有：

![armeabi](http://7xlah4.com1.z0.glb.clouddn.com/20160318153423.jpg)

我在这里遇到了一个`问题`：
由于我还用到了百度导航，因此还有百度导航的 so 文件（只有 armeabi 平台），又因为我的测试手机是 arm64，导致 apk 安装运行后，百度导航初始化失败。

`原因`呢，也很简单：因为 apk 包里有 arm64-v8a, 因此手机会把该文件夹下的 so 放到 系统 libs 文件夹下使用，而该文件夹只有 jni 开发生成串口的 so，没有百度导航的 so，因此 app 运行后找不到百度导航所需要的 so，导致百度导航初始化失败。关于安卓开发下 so 的正确使用姿势可以访问这里：

[关于Android的.so文件你所需要知道的](http://www.jianshu.com/p/cb05698a1968)

`解决方法`，自然就是只打包 armeabi 的 so 到 apk。上面的文章里也给出了解决方法，配置下 gradle 脚本即可：

```gradle
    splits {
        abi {
            enable true
            reset()
            include 'armeabi'   //只打 armeabi 
            universalApk true //true 打一个通用包，包含全部七个平台
        }
    }

    // map for the version code
    project.ext.versionCodes = ['armeabi': 1, 'armeabi-v7a': 2, 'arm64-v8a': 3, 'mips': 5, 'mips64': 6, 'x86': 8, 'x86_64': 9]

    android.applicationVariants.all { variant ->
        // assign different version code for each output
        variant.outputs.each { output ->
            output.versionCodeOverride =
                    project.ext.versionCodes.get(output.getFilter(com.android.build.OutputFile.ABI), 0) * 1000000 + android.defaultConfig.versionCode
        }
    }
```

这样配置好后，就解决了我们的问题，只会把 `armeabi` 平台的 so 打包进 apk。这样运行 apk 后，程序运行一切正常。

2 **自定义包名**

上面配置好后，打包的 apk 的名字是这样的：`app-armeabi-debug.apk`，现在我需要自定义这个包名

要求：只打包 `armeabi` 平台 so 进 apk，并且 apk 包名自定义为 `导航-release--armeabi-v1.3.7-Google_Play-2016-03-18`

感谢给我评论的朋友，刚才找到了解决方法：

**利用 `flavor` 和 `abiFilter` 解决**

部分 gradle 脚本如下:

```gradle
def releaseTime() {
    return new Date().format("yyyy-MM-dd", TimeZone.getTimeZone("UTC"))
}

android {
    ******

    splits {
        abi {
            enable true
            reset()
            include 'armeabi', 'armeabi-v7a', 'arm64-v8a', 'x86', 'x86_64'  //select ABIs to build APKs for
            universalApk true //generate an additional APK that contains all the ABIs
        }
    }

    //自定义apk安装包名
    applicationVariants.all { variant ->
        variant.outputs.each { output ->
            output.outputFile = new File(
                    output.outputFile.parent + "/${variant.buildType.name}",
                    "AndroidSerial-${variant.buildType.name}-${variant.versionCode}-${variant.versionName}-${variant.productFlavors[0].name}-${releaseTime()}.apk".toLowerCase())
        }
    }

    productFlavors {
        armeabi {
            ndk {
                abiFilter "armeabi"
            }
        }
        armeabi_v7a {
            ndk {
                abiFilter "armeabi-v7a"
            }
        }
        
        ******
    }
}
```

详细的配置请看上面的两个 github 项目

这样配置后，生成的包如下：

![abiFilter](http://7xlah4.com1.z0.glb.clouddn.com/20160322112346.jpg)

可以看出，效率很低啊，只有适配连接的手机 cpu 类型的包符合要求。

3 **其他坑**

在解决第一个问题的时候，掉入了另一个坑，用到了一个插件：

[android-native-dependencies](https://github.com/nhachicha/android-native-dependencies)

该插件确实可以只打特定平台的包，并且可以一个，两个，多个平台，但是会把项目里其他的 so 都删除。也就是我用该插件后，可以只打包串口的 so，但同时蛋疼的是把我百度导航的 so 都删除了，这可如何是好啊，因此没有用该插件咯

如果你用过该插件，遇到了哪些坑，欢迎交流

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！