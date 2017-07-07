title: git使用之二——.gitignore文件详解
date: 2015-12-20 14:56:43
categories: git
tags: [git,gitignore]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)


[git使用之一——git的基本使用](http://jp1017.gitcafe.io/2015/12/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%B8%80%E2%80%94%E2%80%94git%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/)
[git使用之二——.gitignore文件详解](http://jp1017.gitcafe.io/2015/12/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%BA%8C%E2%80%94%E2%80%94-gitignore%E6%96%87%E4%BB%B6%E8%AF%A6%E8%A7%A3/)
[git使用之三——.git文件夹详解](http://jp1017.gitcafe.io/2015/12/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%B8%89%E2%80%94%E2%80%94-git%E6%96%87%E4%BB%B6%E5%A4%B9%E8%AF%A6%E8%A7%A3/)
[git使用之四——windows下github桌面版的安装](http://jp1017.gitcafe.io/2015/12/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E5%9B%9B%E2%80%94%E2%80%94windows%E4%B8%8Bgithub%E6%A1%8C%E9%9D%A2%E7%89%88%E7%9A%84%E5%AE%89%E8%A3%85/)
[git使用之五——Github上fork项目后与原项目保持同步](http://jp1017.gitcafe.io/2015/12/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%BA%94%E2%80%94%E2%80%94Github%E4%B8%8Afork%E9%A1%B9%E7%9B%AE%E5%90%8E%E4%B8%8E%E5%8E%9F%E9%A1%B9%E7%9B%AE%E4%BF%9D%E6%8C%81%E5%90%8C%E6%AD%A5/)
[git使用之六——github协同工作的Fork+Pull Request](http://jp1017.gitcafe.io/2015/12/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E5%85%AD%E2%80%94%E2%80%94github%E5%8D%8F%E5%90%8C%E5%B7%A5%E4%BD%9C%E7%9A%84Fork-Pull-Request/)
[git使用之七——Android Studio下git的正确使用](http://jp1017.gitcafe.io/2015/12/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%B8%83%E2%80%94%E2%80%94Android-Studio%E4%B8%8Bgit%E7%9A%84%E6%AD%A3%E7%A1%AE%E4%BD%BF%E7%94%A8/)


# 文件作用
---

一般来说,每个Git项目中都需要一个“.gitignore”文件，这个文件的**作用**就是告诉Git哪些文件不需要添加到版本管理中。

<!--more-->

项目开发中，很多文件都是不需要加入版本管理的，比如java字节码文件.class，安卓虚拟机文件.dex和一些包含密码的配置文件等。

这个文件的内容是一些**规则**，Git会根据这些规则来判断是否将文件添加到版本控制中。

下面我们看看常用的规则：

1. 所有空行或者以注释符号 ＃ 开头的行都会被 Git 忽略。
2. 可以使用标准的 glob 模式匹配。
3. 匹配模式最后跟反斜杠（/）说明要忽略的是目录。
4. 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

所谓的glob模式是指shell所使用的简化了的正则表达式。星号（*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9]表示匹配所有 0 到 9 的数字）。

``` bash
/build/    #过滤整个build文件夹
*.class    #过滤所有.class文件
/.idea/libraries    #过滤具体文件
```

不难吧，#后面的就是注释咯。被过滤掉的文件就不会出现在git库中了，如果push到github上，github库里是没有这些文件的，别人fork后也不会产生不必要的后果，当然本地库中还有，只是push的时候不会上传。

需要注意的是，.gitignore还可以指定要将哪些文件**添加**到版本管理中：

``` bash
!*.apk    #添加*.apk文件到git库里
```

区别是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。

这种规则有时也是需要的，比如我们只需要管理/app/目录中的README文件，这个目录中的其他文件都不需要管理。那么我们就需要使用：

``` bash
/app/
!/app/README
```

如果只有过滤规则没有添加规则，那么就需要把/app/目录下除了README以外的所有文件都写出来！听着就麻烦啊，这是辩证法思想的体现，一阴一阳之谓道的完美表达。

# 使用
---

在android Studio下有个插件.ignore，排名第一的就是这货了，下载来，重启as，然后就可以了

工作项目首次加入到git版本控制后，会自动生成项目的.gitignore和各模块的.gitignore文件，加一句，如果没有，自己手动添加的话，linux下随意添加，但是window有问题了，该文件死活创建不了，其实你只要在最后价格.就可以，就是命名为：**.gitignore.** 然后确定就ok了。每个模块都有一个.gitignore文件哦。

# 姿势

这个文件的作用很大，过滤的规则写的好，减少不必要的麻烦，项目加入git版本后生成的.gitignore文件里是有内容的，但是我们还有必要润色修饰下，来自这里：[What should be in my .gitignore for an Android Studio project?](http://stackoverflow.com/questions/16736856/what-should-be-in-my-gitignore-for-an-android-studio-project)

那么好办了，修改下就ok咯

各module的.gitignore的姿势：

``` bash
/build
*.iml
```

项目下的.gitignore的姿势：
``` bash
.DS_Store
.captures

#built application files
*.apk
*.ap_

# files for the dex VM
*.dex

# Java class files
*.class

# generated files
bin/
gen/

# Local configuration file (sdk path, etc)
local.properties

# Windows thumbnail db
Thumbs.db

# OSX files
.DS_Store

# Eclipse project files
.classpath
.project

# Android Studio
*.iml
.idea
.gradle
build/

#NDK
obj/
```

欢迎补充修改，谢谢

# 注意

**确保push之前，.gitignore文件已经配置好，否则，后面可能出现各种奇葩问题，谨记！！！**


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！