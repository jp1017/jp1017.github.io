title: Android Studio 使用之文件模板
date: 2016-04-05 15:05:04
categories: Android Studio
tags: Android Studio
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![每日一景](http://7xlah4.com1.z0.glb.clouddn.com/QQ%E6%88%AA%E5%9B%BE20160405180054.jpg)

# 作用
---

Android Studio 如何使用，来这里:[打造安卓开发航空母舰](https://github.com/jp1017/Android-Development-Aircraft-Carrier)

今天说的是`文件模板`在 Android Studio 中的使用，先聊下其作用。这里涉及安卓开发编码规范。

我们知道，安卓开发中常用的注释有文件注释，类注释，方法注释，类的成员变量注释，常量注释，xml文件注释等，而文件模板可以方便我们处理文件注释和类注释。我们先来段 `Application.java` 的源码开头部分：

<!--more-->

```java
/*
 * Copyright (C) 2006 The Android Open Source Project
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package android.app;

import java.util.ArrayList;

import android.annotation.CallSuper;
import android.content.ComponentCallbacks;
import android.content.ComponentCallbacks2;
import android.content.Context;
import android.content.ContextWrapper;
import android.content.Intent;
import android.content.res.Configuration;
import android.os.Bundle;

/**
 * Base class for those who need to maintain global application state. You can
 * provide your own implementation by specifying its name in your
 * AndroidManifest.xml's &lt;application&gt; tag, which will cause that class
 * to be instantiated for you when the process for your application/package is
 * created.
 * 
 * <p class="note">There is normally no need to subclass Application.  In
 * most situation, static singletons can provide the same functionality in a
 * more modular way.  If your singleton needs a global context (for example
 * to register broadcast receivers), the function to retrieve it can be
 * given a {@link android.content.Context} which internally uses
 * {@link android.content.Context#getApplicationContext() Context.getApplicationContext()}
 * when first constructing the singleton.</p>
 */
public class Application extends ContextWrapper implements ComponentCallbacks2 {
    ******
}
```

这个大家都熟悉吧，一个 java 文件最前面是文件注释，类似C语言里的注释，用`/*  */`来注释，然后是包名，引用的包名，下面就是类的注释，用的是文档注释 `/**   */`，最后就是类的具体代码了。

# 使用
---

Android Studio 给我们提供了方便，我们可以很方便的完成这些操作：

1 打开设置界面，Settings-->Editor-->File and Code Templates，然后点击上方的 Files，再点击 Class(没有的话，点击上面绿色的“+”号添加)

![settings](http://7xlah4.com1.z0.glb.clouddn.com/20160405152547.jpg)

右侧修改如下：

```
#parse("File Header.java")
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end
#parse("Class Header.java")
public class ${NAME} {
}
```

2 切换到 Includes,修改 Class Header 如下：

```
/**
 * 文 件 名: ${NAME}
 * 创 建 人: 蒋朋
 * 创建日期: ${DATE} ${HOUR}:${MINUTE}
 * 邮   箱: jp19891017@gmail.com  
 * 博   客: http://jp1017.github.io
 * 修改时间：
 * 修改备注：
 */
```

当然，这个是我的信息，你要修改成你自己的咯。

3 点击上面绿色的“+”号，重命名为 File Header,并输入下面内容:

```
/*
******************************* Copyright (c)*********************************\
**
**                 (c) Copyright 2015, 蒋朋, china, qd. sd
**                          All Rights Reserved
**
**                           By(***公司)
**                         
**-----------------------------------版本信息------------------------------------
** 版    本: V0.1
**
**------------------------------------------------------------------------------
********************************End of Head************************************\
*/
```

保存，点击ok，现在新建一个类，看下效果：

```
/*
******************************* Copyright (c)*********************************\
**
**                 (c) Copyright 2015, 蒋朋, china, qd. sd
**                          All Rights Reserved
**
**                           By()
**                         
**-----------------------------------版本信息------------------------------------
** 版    本: V0.1
**
**------------------------------------------------------------------------------
********************************End of Head************************************\
*/

package com.inst.instdriver.app;

/**
 * 文 件 名: MyApplication
 * 创 建 人: 蒋朋
 * 创建日期: 16-4-5 18:06
 * 邮   箱: jp19891017@gmail.com
 * 博   客: http://jp1017.github.io
 * 修改时间：
 * 修改备注：
 */
public class MyApplication {
}
```

好了，就到这里了。


安卓开源库收集整理中，欢迎 PR， star，地址：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！