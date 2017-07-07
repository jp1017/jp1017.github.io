title: jdk1.8新特性之lambda表达式及在Android Studio中的使用
date: 2015-12-20 16:26:45
categories: Android Studio
tags: [Android Studio,lambda]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

原帖见我的博客：[你好，我是博客](http://jp1017.gitcafe.io/2015/09/14/jdk1-8%E6%96%B0%E7%89%B9%E6%80%A7%E4%B9%8Blambda%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%8F%8A%E5%9C%A8Android-Studio%E4%B8%AD%E7%9A%84%E4%BD%BF%E7%94%A8%E4%B8%BE%E4%BE%8B/)

# Lambda表达式
---

Lambda，是不是听着很熟悉，没错，在高等数学中这货经常和我们打交道，哈哈，这是一个希腊字母了，排名第十一，大写是Λ，小写是λ，当然我们经常见的还是小写，风韵犹存哦。说了这么多，然并卵，和我们今天的主题有鸟关系？

<!--more-->

好了，是这样的。jdk1.8中新增的核心特性有lambda表达式（哈哈，原来你也在这里），函数式接口，流API，默认方法，新的Date，以及Time API。详细介绍请看这里：[Java 8简明教程](http://www.importnew.com/10360.html)。
下面给大家介绍的是Lambda表达式，该表达式允许我们把行为传到函数里。之前把行为传到函数里我们采用的是匿名内部类，该方法导致行为最重要的方法夹杂在中间，不够突出，详见举例中代码。

lambda表达式取代了匿名内部类，取消了模板，允许程序猿用函数式风格编写代码，使代码可读性更高，尽管刚开始你会看不懂，但是你应该尝试，毕竟这是新的东西，我已从中获益。

## 格式

基本格式是：() -> {}

有下面三种具体表达：

1. (params) -> expression
2. (params) -> statement
3. (params) -> {statement}

这个新的特性是激动人心的，那么有个问题，怎么用，是啊，很多东西我们都懂，但为什么还是过不好这一生，说远了，问题的关键是：用，得用，你还得会用。

# lambda使用举例
---

## as里的配置
要使用lambda，首先必须配置编译环境，这里使用的android studio，as默认的jdk版本是1.6，修改成1.8即可使用，这里确保你系统安装了jdk1.8，否则需要用到下面插件：[gradle-retrolambda](https://github.com/evant/gradle-retrolambda)

as里的配置有两种方法：
### 配置gradle脚本
在build.gradle脚本中添加下列代码：

``` bash
android {
   compileOptions {
       sourceCompatibility 1.8
       targetCompatibility 1.8
   }
}
```

当然写成下面的样子也是可以的

``` bash
android {
   compileOptions {
       sourceCompatibility JavaVersion.VERSION_1_8
       targetCompatibility JavaVersion.VERSION_1_8
   }
}
```

### 设置项目结构

按快捷键Ctrl+Shift+Alt+S进入项目结构设置，把app的jdk版本修改成1.8，注意你需要填写1.8，因为那个下拉菜单里没有这一选项，如下：

![1](http://7xlah4.com1.z0.glb.clouddn.com/2015091404.jpg)

这里采用的是第二种方法，然后项目自动同步。

同步后是会在build.gradle脚本下生成和上面一样的东东：

![2](http://7xlah4.com1.z0.glb.clouddn.com/2015091403.jpg)

## 举例
### 点击按钮触发事件

传统的点击事件，应用匿名内部类：

``` bash
Button button = (Button) findViewById(R.id.btn_insert);
    button.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            Toast.makeText(MainActivity.this, "hello", Toast.LENGTH_LONG).show();
        }
    });
```

通过上面设置jdk版本为1.8后，鼠标放到new View.OnClickListener()里会有下面提示：

![3](http://7xlah4.com1.z0.glb.clouddn.com/2015091401.jpg)

然后我们按快捷键Alt+Enter,是这样的

![4](http://7xlah4.com1.z0.glb.clouddn.com/2015091402.jpg)

继续回车后，见证奇迹的时刻到来lo。。。

使用lambda表达式之后是这样的

``` bash
Button button = (Button) findViewById(R.id.btn_insert);
button.setOnClickListener(v -> Toast.makeText(MainActivity.this, "hello", Toast.LENGTH_LONG).show());
```

一行代码就搞定了，清晰可见，把行为传到了函数里，这里注意v不可省略，是函数onClick的参数，当然就可以是任意名字，我还是建议就用一个字母表示，简单嘛，当然了首先你得知道她的意思，尽管她的很多行为你始终不会明白，哈哈。

### 实现Runnable接口

传统实现Runnable接口是这样的：

``` bash
new Thread(new Runnable() {
    @Override
    public void run() {
        Log.i("TAG", "haha");
    }
}).start();
```

使用lambda表达式之后是这样的：

``` bash
new Thread(() -> {
    Log.i("TAG", "haha");
}).start();
```

哇哦，是不是有种很清爽的感觉，乍一看，这是什么鬼，仔细分析后是用了lambda表达式() -> {},哈哈，简单的爱，这是程序员懒惰的一种体现，可以写出更简洁高校的代码，赞一个。

## 比较

既然lambda表达式即将正式取代Java代码中的匿名内部类，那么有必要对二者做一个比较分析。

第一个关键的不同点就是关键字 this。匿名类的 this 关键字指向匿名类，而lambda表达式的 this 关键字指向包围lambda表达式的类。

第二是编译方式。Java编译器将lambda表达式编译成类的私有方法。使用了Java 7的 invokedynamic 字节码指令来动态绑定这个方法。

# 总结
---

lambda表达式还有很多用法，比如迭代器，详细用法请参考这里：[Java8 lambda表达式10个示例](http://www.importnew.com/16436.html)

好了，希望你们喜欢！！！

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！