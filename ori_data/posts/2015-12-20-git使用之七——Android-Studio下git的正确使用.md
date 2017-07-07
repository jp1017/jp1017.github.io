title: git使用之七——Android Studio下git的正确使用
date: 2015-12-20 15:27:12
categories: git
tags: [git,Android Studio]
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

# 前言
---

as是安卓开发的神器，那么她里面怎么能少得了git。下面讲的是as下的git的使用，当然单纯的命令行也是可以完美使用git的，她本来就是为命令行而生的，详细使用参考我之前博文：[git使用之一——git的基本使用](http://jp1017.gitcafe.io/2015/09/17/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%B8%80%E2%80%94%E2%80%94git%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/)

<!--more-->

# 使用
---

1 问题

你新建一个项目后，会在右上角出现这一对话框：

![11](http://7xlah4.com1.z0.glb.clouddn.com/2015092411.png)

as在提醒你该项目没有加入VCS版本控制，那么为什么要加入版本控制呢？是啊，为什么呢？呵呵。。。

2 配置

我这里用的是win10，没有git的去[**官网**](http://git-scm.com/download/)下载安装，安装后，这么配置下就ok的

![12](http://7xlah4.com1.z0.glb.clouddn.com/2015092413.png)

2 初始化

这个初始化就相当于git init，当然你是可以在终端这么用的，我们这里是去到设置界面，配置下，File -> Settings -> Version Control，选取Unregistered roots：下的选项，然后点右侧绿色的“+”，这样就把项目添加到git下了。

![13](http://7xlah4.com1.z0.glb.clouddn.com/2015092412.png)

这里注意的是此处的git最好是在当前项目下，如果不是，切换到当前目录，git init下，然后再来这里添加。

3 介绍

有必要介绍下as下git的面纱，来个图吧，全部搞定。

![14](http://7xlah4.com1.z0.glb.clouddn.com/2015092414.png)

哇哦，此图内容非常丰富，相信看过都懂了，没有进入vcs的文件是红底的，.gitignore文件屏蔽掉的文件和文件夹是灰色的，关于.gitignore的正确写法，请参考这里：[git使用之二——.gitignore文件详解](http://jp1017.gitcafe.io/2015/09/17/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%BA%8C%E2%80%94%E2%80%94-gitignore%E6%96%87%E4%BB%B6%E8%AF%A6%E8%A7%A3/)

as对于vcs的操作还是很给力的，下面的Version Control视图内容很赞，后面我们详细介绍，这里不仅对git，对subversion同样友好。

git的基本使用是add到暂缓区，commit到本地仓库，push到远程仓库，下面逐一讲解。

4 添加

下面把鼠标放到左侧project任意文件上，然后Ctrl+A，选中所有文件，右键，git -> add

![15](http://7xlah4.com1.z0.glb.clouddn.com/2015092415.png)

哇哦，全绿，哈哈，比整天盯着股票爽多了，嘎嘎。这个就相当于git add命令，把文件添加到了暂缓区了，这个暂缓区是区别subversion的，我们来看，放个图：

![git](http://7xlah4.com1.z0.glb.clouddn.com/20150924git.jpg)

变绿的效果图，有个女孩就喜欢绿色，哈哈。

![166](http://7xlah4.com1.z0.glb.clouddn.com/2015092416.png)

5 提交

提交方法就很多了，Ctrl+K或者点这里进入提交界面：

![17](http://7xlah4.com1.z0.glb.clouddn.com/2015092417.png)

提交界面在这里：

![18](http://7xlah4.com1.z0.glb.clouddn.com/2015092418.png)

这样修改后commit，成功后，回到编辑界面，看到project项目的目录颜色正常了哦

![19](http://7xlah4.com1.z0.glb.clouddn.com/2015092419.png)

然后我们看下面的version control视图：

![21](http://7xlah4.com1.z0.glb.clouddn.com/2015092421.png)

我们修改一个文件后，文件颜色会成蓝色，我们看，我在MainActivity.java文件下添加个todo，我们看：

![20](http://7xlah4.com1.z0.glb.clouddn.com/2015092420.png)

6 push

push的主要作用是保存到远程仓库，用来团队协作或者网络备份用。

push之前呢，先把远程仓库建好，这里选择oschina（快速稳定，私人库免费等）托管，仓库建好后才可以push哦。我们现在新建一个

![18](http://7xlah4.com1.z0.glb.clouddn.com/2015092422.png)

oschina还是很贴心的，新建后，会教你怎么托管到这里，我们这里复制下这个仓库的地址，后面使用

![18](http://7xlah4.com1.z0.glb.clouddn.com/2015092423.png)

直接Ctrl+Shift+K进入push界面
define remote，配置远程仓库

![24](http://7xlah4.com1.z0.glb.clouddn.com/2015092424.png)

填写name（默认origin，可随意配置） 和url（刚才复制的仓库名）

![25](http://7xlah4.com1.z0.glb.clouddn.com/2015092425.png)

需要你配置oschina的帐号和密码

![26](http://7xlah4.com1.z0.glb.clouddn.com/2015092426.png)

设定一个master密码

![27](http://7xlah4.com1.z0.glb.clouddn.com/2015092427.png)

ok，now push...

![28](http://7xlah4.com1.z0.glb.clouddn.com/2015092428.png)

成功后有提示框

![29](http://7xlah4.com1.z0.glb.clouddn.com/2015092429.png)

回到远程oschina仓库，看到push成功

![30](http://7xlah4.com1.z0.glb.clouddn.com/2015092430.png)

7 pull

pull，update更新的姿势是这样的

![31](http://7xlah4.com1.z0.glb.clouddn.com/2015092431.png)

到这里，基本应用就结束了，多多练习，多多操作，你就会玩了。

8 branch

分支在右下角：

![bran](http://7xlah4.com1.z0.glb.clouddn.com/20150924085410.png)

这里包括分支的基本操作，新建，切换，删除等，我们新建一个分支jp，之后该视图这样：

![new branch](http://7xlah4.com1.z0.glb.clouddn.com/20150924085010.png)

会有个新建分支成功的提示消息，一般成功的提示都是绿底，失败的都是红底的哦。

我们来看下该视图其他标签：

Local change和console

![local](http://7xlah4.com1.z0.glb.clouddn.com/20150924090604.png)

![console](http://7xlah4.com1.z0.glb.clouddn.com/20150924085753.png)

as是不是很贴心哦，哈哈。


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！