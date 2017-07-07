title: git使用之五——Github上fork项目后与原项目保持同步
date: 2015-12-20 15:19:15
categories: git
tags: [git,github]
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

# 问题由来
---

前几天在github上fork了个项目，今天发现了问题，原作者更新了代码，我的没变样，于是再次fork，仍然没有更新，郁闷了，网上搜索，发现遇到这问题的小伙伴不少呢，然后在此记录下，以便后面遇到同样问题的小伙伴可以迅速解决问题。

<!--more-->

# 解决
---

1 重新fork
---

这里的fork不是说你直接点fork，这样是没用的，你要从你的仓库里删除fork来的项目，然后重新fork，这个姿势不大好。。。

2 正确方式
---

官方解决方案：[Syncing a fork](https://help.github.com/articles/syncing-a-fork/)

下面是翻译后的具体操作：

方法是在本地建立两个库的中介，把两个远程库都clone到本地，然后拉取原项目更新到本地，合并更新，最后push到你的github就完成。

## clone

我准备在e盘下建立这个中介，就是本地仓库了，进入e盘，右键，git bash打开控制台，clone自己的远程仓库：

``` bash
$ git clone https://github.com/jp1017/FastAndroid.git
```

然后cd fastandroid，进入仓库，执行命令：git remote -v 

``` bash
$ git remote -v 
```

![z](http://7xlah4.com1.z0.glb.clouddn.com/20150926162833.png)

我们可以看到，只有我们自己的远程仓库，下面clone原项目到该仓库

``` bash
$ git remote add hunter https://github.com/huntermr/FastAndroid.git
$ git remote -v
```

![z](http://7xlah4.com1.z0.glb.clouddn.com/20150926155727.png)

这个hunter名字随便取哦，这次有两个远程分支咯，我们继续

## fetch

然后把原项目更新的内容fetch到本地：git fetch hunter

``` bash
$ git fetch hunter
```

查看下分支：

``` bash
$ git branch -av
```

![z](http://7xlah4.com1.z0.glb.clouddn.com/20150926163813.png)

一个本地分支master，三个远程分支，画红线的就是要合并的哦

## merge

``` bash
$ git checkout master
$ git merge hunter/master
```

![z](http://7xlah4.com1.z0.glb.clouddn.com/20150926164157.png)

## push

``` bash
$ git push -u origin master
```

merge后本地的仓库已经是最新的咯，然后push到你的github就同步完成。

3 github桌面版
---

这个需要借助github桌面版，windows下的安装参考我的另一片博文：[git使用之四——windows下github桌面版的安装](http://jp1017.gitcafe.io/2015/09/18/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E5%9B%9B%E2%80%94%E2%80%94windows%E4%B8%8Bgithub%E6%A1%8C%E9%9D%A2%E7%89%88%E7%9A%84%E5%AE%89%E8%A3%85/)


安装好后，clone到本地，然后点下sync，这样本地的就更新了哦，然后点下github，就同步了。

![z](http://7xlah4.com1.z0.glb.clouddn.com/20150926152116.png)

当然也可以使用 `sourcetree` 客户端，这个更好用哦。

搞定。

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！