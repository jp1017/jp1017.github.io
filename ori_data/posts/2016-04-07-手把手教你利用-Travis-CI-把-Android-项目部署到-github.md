title: 手把手教你利用 Travis CI 把 Android 项目部署到 GitHub
date: 2016-04-07 20:17:45
categories: Android
tags: [Android,CI,GitHub]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![每日一景](http://7xlah4.com1.z0.glb.clouddn.com/20160407213750.jpg)

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="http://music.163.com/outchain/player?type=2&id=28815250&auto=1&height=66"></iframe>

本文不涉及 `CI` 是什么以及优势等，不知道的还请自行 google，这里会手把手教你如何操作。

闲话少说，开始吧。

一共三步：
1. 开启 `Travis CI`
2. 编写 部署文件 `.travis.yml`
3. 部署到 `GitHub`

这里会教你如何操作以及如何解决遇到的问题

<!--more-->

# 开启 `Travis CI`
---

首先登录 [Travis CI](https://travis-ci.org/)，没有登录的话，用 github 授权登录即可，登录后，点击右上角你的账户，会同步你 GitHub 仓库，把你要进行 CI 的仓库打开即可：

![rep](http://7xlah4.com1.z0.glb.clouddn.com/20160407204911.jpg)

# 部署文件 `.travis.yml`
---

我是在 `android studio` 下操作的，所以直接新建一个文件，重命名为 .travis.yml 即可，这是一个 yml 文件，有一些需要知道的语法，很简单的几个，自行 google

最基本的写法如下：

```
language: android  
android:
  components:
    - tools
    - build-tools-23.0.2
    - android-23
    - extra-android-m2repository
    - extra-android-support

before_install:
  - chmod +x gradlew
  
script:  
  - ./gradlew assembleRelease
```

上面的内容还是很好理解的，有几个 tag：android 语言、用到的一些组件、增加 gradlew 脚本可执行权限及运行该脚本
该文件的详细说面，这里有详细介绍：[Building an Android Project](https://docs.travis-ci.com/user/languages/android)

编写好后，commit push 即可完成基本 CI。



# 部署到 GitHub
---

我们的目的是部署到 GitHub，方便我们使用，那么还需要下面的一些操作。

由于 travis 是 ruby 编写，需要下载 ruby 并安装 travis，ruby 下载链接：[https://www.ruby-lang.org/zh_cn/downloads/](https://www.ruby-lang.org/zh_cn/downloads/)

然后在你的终端运行命令：

```
gem install travis -v 1.8.2 --no-rdoc --no-ri
```

此处可能会有问题，首先 windows 用户提示 gem 不是系统命令等错误，这个配置下环境变量，把ruby/bin目录加入环境变量即可。

还可能遇到这个问题：[connection was forcibly closed by the remote host](Errno::ECONNRESET: An existing connection was forcibly closed by the remote host)，这个就是 gem 仓库访问不到，也就是你需要翻墙，而大部分翻墙也不好使，这就需要添加国内的 gem 源，这里有两个：

[RubyGems 镜像 - 淘宝](https://ruby.taobao.org/)
[RubyGems 镜像- Ruby China](https://gems.ruby-china.org/)

使用方法里面也有说明，以淘宝为例：

```
$ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
$ gem sources -l
*** CURRENT SOURCES ***

https://ruby.taobao.org
# 请确保只有 ruby.taobao.org
```

此处可能会遇到证书错误：[gem install SSL 错误](https://ruby-china.org/topics/29323)

里面说了很多，其实解决方法就是找到证书，配置下就好了.

[关于 Windows 下证书无法验证问题 (certificate verify failed)](https://github.com/ruby-china/rubygems-mirror/wiki)

ubuntu　下遇到证书错误需要重新刷新下证书：

>sudo update-ca-certificates --fresh

2016.09.16　ubuntu安装时遇到错误：

>can't find header files for ruby at /usr/lib/ruby/include/ruby.h

```
 sudo gem install travis -v 1.8.2 --no-rdoc --no-ri

Building native extensions.  This could take a while...
ERROR:  Error installing travis:
	ERROR: Failed to build gem native extension.

    current directory: /var/lib/gems/2.3.0/gems/ffi-1.9.14/ext/ffi_c
/usr/bin/ruby2.3 -r ./siteconf20160916-2426-1ewzf6n.rb extconf.rb
mkmf.rb can't find header files for ruby at /usr/lib/ruby/include/ruby.h

extconf failed, exit code 1

Gem files will remain installed in /var/lib/gems/2.3.0/gems/ffi-1.9.14 for inspection.
Results logged to /var/lib/gems/2.3.0/extensions/x86_64-linux/2.3.0/ffi-1.9.14/gem_make.out
```

解决办法：https://github.com/jasmine/jasmine/issues/755#issue-54955239
需要安装　`ruby-dev`

>apt-get install gcc ruby ruby-dev libxml2 libxml2-dev libxslt1-dev


好了，这样在安装 travis 就可以了

安装好后就可以部署到 GitHub：
首先登录：

```
travis login --auto
```

**重要提醒**：这条命令在windows下使用cmd，在输入密码时都显示明文密码，并且回车后无响应，我留下了这个：

[can't travis login](https://github.com/nukc/how-to-use-travis-ci/issues/1)
[can't travis login](https://github.com/travis-ci/travis.rb/issues/390)

我是开启了 bash，在里面操作成功，遇到同样问题的朋友请更换操作系统或开启bash。

然后部署：

```
travis setup releases
```

会提示输入用户名，密码，文件名等，完成后就搞定了。

最后就是提交push代码到GitHub了，push后就可以看到 travis 网站上对应的项目开始集成。
错误在所难免，细心点，用力点，都会解决了。

详细安装说明在这里:[https://github.com/travis-ci/travis.rb#installation](https://github.com/travis-ci/travis.rb#installation)

这样就可以使用了，当然如果你喜欢，还可以发布到fir.im

# 自动发布APK到fir.im
---

很简单，添加几行就可以：

```
before_install:
- gem install fir-cli
after_deploy:
- fir p app/build/outputs/apk/app-release.apk -T $FIR_TOKEN -c "`git cat-file tag $TRAVIS_TAG`"
```

即在环境构建阶段安装fir-cli，在发布成功后通过fir命令行工具将apk上传到fir。

其中$FIR_TOKEN可以在fir.im的用户->API Token中找到，然后在Travis CI控制台中创建环境变量FIR_TOKEN并粘贴即可。

git cat-file tag $TRAVIS_TAG将当前发布tag所包含的附加信息一同上传，方便参看附加信息；$TRAVIS_TAG变量是Travis CI每次运行自动附带的环境变量


安卓开源库收集整理中，欢迎 PR， star，地址：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！