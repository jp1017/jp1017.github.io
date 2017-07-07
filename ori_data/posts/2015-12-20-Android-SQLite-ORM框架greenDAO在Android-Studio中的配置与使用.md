title: Android SQLite ORM框架greenDAO在Android Studio中的配置与使用
date: 2015-12-20 14:37:13
categories: Android
tags: [Android,greenDAO]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

# 说明
---

greenDAO是安卓中处理SQLite数据库的一个开源的库，详情见其官网：[我是官网](http://greendao-orm.com/)

详细使用，官网也有详细说明，这里稍加修饰

好了，我们开始吧 ^_^

<!--more-->

这里是在as下操作，有一个安卓项目，一个java项目（module）用于生成DAO

# 新建存放greenDAO的生成目录
---

在 ***/src/main目录下新建一个与 java 同层级的java-gen目录，用于存放由 greenDAO 生成的 Bean、DAO、DaoMaster、DaoSession类:

![1](http://7xlah4.com1.z0.glb.clouddn.com/2015090803.jpg)

![2](http://7xlah4.com1.z0.glb.clouddn.com/2015090804.jpg)

# 配置 Android工程（app）的 build.gradle脚本
---

如下图分别添加 sourceSets 与dependencies：
 
![3](http://7xlah4.com1.z0.glb.clouddn.com/2015090805.jpg)

# 新建一个java工程（module）用于生成DAO（数据库）
---

通过 File -> New -> New Module -> Java Library -> 填写相应的包名与类名 -> Finish，如下图：

![4](http://7xlah4.com1.z0.glb.clouddn.com/2015090806.jpg)

![5](http://7xlah4.com1.z0.glb.clouddn.com/2015090807.jpg)

![6](http://7xlah4.com1.z0.glb.clouddn.com/2015090808.jpg)

# 配置该模块工程的 build.gradle，添加 dependencies：
---

![7](http://7xlah4.com1.z0.glb.clouddn.com/2015090809.jpg)

# 编写该java工程类
---

![8](http://7xlah4.com1.z0.glb.clouddn.com/2015090816.jpg)

# 生成DAO
---

此处可以修改gradle脚本执行，这里直接用界面了，如下：
设置java运行项目
![9](http://7xlah4.com1.z0.glb.clouddn.com/2015090817.jpg)

![10](http://7xlah4.com1.z0.glb.clouddn.com/2015090810.jpg)

![11](http://7xlah4.com1.z0.glb.clouddn.com/2015090811.jpg)
点击运行
![12](http://7xlah4.com1.z0.glb.clouddn.com/2015090812.jpg)

这样，DAO就生成了，请看：

![13](http://7xlah4.com1.z0.glb.clouddn.com/2015090814.jpg)

刚开始出现了一个错误，gradle1.2.3找不到：

![14](http://7xlah4.com1.z0.glb.clouddn.com/2015090813.jpg)

修改成1.3.0的就ok了，这里修改：

![15](http://7xlah4.com1.z0.glb.clouddn.com/2015090815.jpg)

# 使用
---

使用也很简单，只要创建数据可，拿到DaoSession对象，然后就可以增删改查了，下面是个简单的insert介绍:

```java
    //创建数据库
    private void buildDatabase() {
        DaoMaster.DevOpenHelper helper = new DaoMaster.DevOpenHelper(this, "user-db", null);
        mDaoMaster = new DaoMaster(helper.getWritableDatabase());
        mDaoSession = mDaoMaster.newSession();
    }
    
    //插入数据举例
        for (int i = 0; i < 10; i++) {
        User user = new User();
        user.setNumber(i + "");
        user.setPassword("#####" + i + "*****");
        users.add(user);
        mDaoSession.insert(user);
    }
```

运行后，可以用re管理器查看数据库：

![greendao数据](http://7xlah4.com1.z0.glb.clouddn.com/20151223094821.jpg)

详细使用，去项目里看看吧，很简单的哦，just do it .

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！