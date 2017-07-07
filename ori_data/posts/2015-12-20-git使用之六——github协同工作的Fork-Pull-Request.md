title: git使用之六——github协同工作的Fork+Pull Request
date: 2015-12-20 15:26:42
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

网上你看到某个大神的项目很炫，很灿烂，你看了看，发现有bug或者你参与开源项目，需要参与进去，那么你来对了地方，这里带你如何和大神一起工作。

# Fork
---

网上有这个开源项目：[FastAndroid](https://github.com/huntermr/FastAndroid)
很好，最近要写个demo，大家有兴趣的欢迎加入我们。

<!--more-->

首先fork（派生）到自己的github：

![fork](http://7xlah4.com1.z0.glb.clouddn.com/20150926104337.png)

fork后进入到自己的仓库：

![ff](http://7xlah4.com1.z0.glb.clouddn.com/20150926104456.png)

由于我之前fork过，作者修改了内容，所以我这里的仓库是不会更新的，那么怎么保持更新呢，请来我之前的博文：

[git使用之五——Github上fork项目后与原项目保持同步](http://jp1017.gitcafe.io/2015/09/18/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%BA%94%E2%80%94%E2%80%94Github%E4%B8%8Afork%E9%A1%B9%E7%9B%AE%E5%90%8E%E4%B8%8E%E5%8E%9F%E9%A1%B9%E7%9B%AE%E4%BF%9D%E6%8C%81%E5%90%8C%E6%AD%A5/)

# 修改
---

下面自己的仓库里有东西了，你可以按照自己的想法开发咯，哈哈，自行发挥吧。

这里介绍下在android studio下的使用。

打开android studio，在初始界面，右侧选择第三个Check out project from Version Control，然后点Github

![cc](http://7xlah4.com1.z0.glb.clouddn.com/20150926105007.png)

之后as会要求你填写你github的用户名和密码

![user](http://7xlah4.com1.z0.glb.clouddn.com/20150926105040.png)

确认无误，as需要你填写clone的远程仓库地址：

![ee](http://7xlah4.com1.z0.glb.clouddn.com/20150926105745.png)

仓库地址github右侧：

![ori](http://7xlah4.com1.z0.glb.clouddn.com/20150926105500.png)

方式有https或ssh，都是可以的，ssh需要提前配置密钥的，怎么配置，来这里配置：

[git使用之一——git的基本使用](http://jp1017.gitcafe.io/2015/09/17/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%B8%80%E2%80%94%E2%80%94git%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/)

点击右侧test，测试下仓库的正确性，正确弹出正确对话框：

![right](http://7xlah4.com1.z0.glb.clouddn.com/20150926105759.png)

之后点击clone，就clone到本地咯，as会提示你时候打开，我们确定。

![open](http://7xlah4.com1.z0.glb.clouddn.com/20150926105900.png)

当然clone到本地你可以直接一条命令搞定，这里演示的是as下的使用。

这里你新建一个分支，这不会影响到管理者的项目，这里的分支都是你自己的本地分支，push也是push到你自己的库分支去。修改好项目后，commit到本地仓库，push到你的远程仓库，as下如何操作，来这里：[git使用之七——Android Studio下git的正确使用](http://jp1017.gitcafe.io/2015/09/20/git%E4%BD%BF%E7%94%A8%E4%B9%8B%E4%B8%83%E2%80%94%E2%80%94Android%20Studio%E4%B8%8Bgit%E7%9A%84%E6%AD%A3%E7%A1%AE%E4%BD%BF%E7%94%A8/)

好了，你的远程仓库已经更新了，下面发起请求合并。

# Pull Request
---

回到你的github界面，发起请求：

![request](http://7xlah4.com1.z0.glb.clouddn.com/20150926134621.png)

新建请求：

![new](http://7xlah4.com1.z0.glb.clouddn.com/20150926134647.png)

由于我没有修改内容，所以，下面的Comparing changes空白：

![poll](http://7xlah4.com1.z0.glb.clouddn.com/20150926134707.png)

这样，项目管理者就会看到你的请求，合适的话，他就会合并咯，哈哈！

有不对的地方欢迎指出，共同进步。

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！