title: ViewPager 嵌套百度地图事件冲突的解决方法
date: 2016-07-28 21:18:46
categories: Android
tags: [Android,事件分发,ViewPager,百度地图]
---

博客：	[安卓之家](http://jp1017.github.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)

![每日一景](https://drscdn.500px.org/photo/72751293/m%3D2048/49a274baba6d51c1242995a4d4654c70)


# 问题
---

主 Activity 由一个自定义的 ViewPager 控制，包含多个 fragment，其中一个 fragment 包含一个百度地图 View，当左右滑动百度地图的时候，地图不动，只能望着 ViewPager 左右滑动的脚步而叹息，导致事件冲突。

# 解决之路
---

这个问题从开始就有，只是感觉事件分发有些复杂，没深入研究，网上很多大神关于这个知识点有很多的分享，也收获了不少，曾经按照《开发艺术探索》里的方法解决，最终地图的事件解决，又导致其他 View 冲突，不能操作，这个真心没法忍啊。

这个问题一直在我的心里，始终没有放下，只要看到关于事件分发的文章，我都会看一下，看能否对此有帮助，然而，一无所获。

直到今天，晚上，有心人天不负，无意的，我看到一个群里的群主这么写到：

<!--more-->

![mobdevgroup](http://7xlah4.com1.z0.glb.clouddn.com/20160728212904.jpg)

在安卓界，这个群主的头像相信大家应该不会陌生，这里给出他的博客以表感谢：[移动平台资源整理和分享](http://mobdevgroup.com/)

没错，就是 `ViewGroup` 的 `requestDisallowInterceptTouchEvent` 方法

然后一顿 google，该方法的使用还是真多啊，看来有戏，首先进入这里：[用requestDisallowInterceptTouchEvent()方法防止viewpager和子view冲突](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2013/0803/1500.html)，这里说的很清楚：

>当用户按下的时候，我们告诉父组件，不要拦截我的事件（这个时候子组件是可以正常响应事件的），拿起之后就会告诉父组件可以阻止。

然后就开始实践了，我的代码如下：

```
    final MyViewPage pager = new MyViewPage(getBaseActivity());
    mMapView.setOnTouchListener(new View.OnTouchListener() {
        KLog.w("upupupupup    upupupupup");
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            switch (event.getAction()) {
                case MotionEvent.ACTION_MOVE:
                    KLog.w("upupupupup    actionmove");
                    pager.requestDisallowInterceptTouchEvent(true);
                    break;
                case MotionEvent.ACTION_UP:
                case MotionEvent.ACTION_CANCEL:
                    KLog.w("upupupupup    actionup");
                    pager.requestDisallowInterceptTouchEvent(false);
                    break;
            }
            return false;
        }
    });
```

然而，程序跑起来后，一个都没有打印。没运行？编译器没刷新？然后 clean，sync, rebuld,run,问题依旧，看来是没有运行了。

那自定义一个 MapView，发现 MapView 是 final，根本继承不了。

然后来到了这里：[百度地图和scrollview使用冲突问题](http://bbs.lbsyun.baidu.com/forum.php?mod=viewthread&tid=15245)，三楼的朋友给出了解决方案，既然地图不可继承，那就自定义地图容器，在容器里进行该操作，然后自定义好地图容器，把容器写到布局里，并把容器布局和 ViewPager 联系起来，运行后，方法是运行了，但是问题依旧。

既然能运行，那拦截的可能有问题咯，或者联系的这个 ViewPager 可能不是同一个，该如何继续？？？

又看到了这个：[运项目难点之ScrollView中嵌套百度地图（BaiduMap）的解决方案](http://blog.csdn.net/theone10211024/article/details/44649289)

该作者也遇到了我的这个问题，百度这个坑确实深啊，作者在无望的情况下，反编译了地图 jar 包，在下佩服的五体投地，最终作者发现拦截 `MapView.OnTouchListener.onTouch()`的是地图图层 `mMapView.getChildAt(0)`

于是，删掉地图容器类，回到最开始的地图布局，用地图图层处理事件，修改代码如下：

```
    final MyViewPage pager = new MyViewPage(getBaseActivity());
    mMapView.getChildAt(0).setOnTouchListener(new View.OnTouchListener() {
        KLog.w("upupupupup    upupupupup");
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            switch (event.getAction()) {
                case MotionEvent.ACTION_MOVE:
                    KLog.w("upupupupup    actionmove");
                    pager.requestDisallowInterceptTouchEvent(true);
                    break;
                case MotionEvent.ACTION_UP:
                case MotionEvent.ACTION_CANCEL:
                    KLog.w("upupupupup    actionup");
                    pager.requestDisallowInterceptTouchEvent(false);
                    break;
            }
            return false;
        }
    });
```

运行后，问题依然存在，和容器类的效果类似，该图层确实获取到了该事件，但是仍然被 ViewPager 拦截了，so，问题只能是执行 `requestDisallowInterceptTouchEvent` 的这个 pager 有问题，我们看到，这个 pager 我是直接 new 出来的，那么修改下这个 pager？

我注意到，onTouch 方法里的第一个参数 View，还没有用，那么就该在这里下手了，于是最终代码来了：

```
    mMapView.getChildAt(0).setOnTouchListener(new View.OnTouchListener() {
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            switch (event.getAction()) {
                case MotionEvent.ACTION_MOVE:
                    KLog.w("upupupupup    actionmove");

                    v.getParent().requestDisallowInterceptTouchEvent(true);
                    break;
                case MotionEvent.ACTION_UP:
                case MotionEvent.ACTION_CANCEL:
                    KLog.w("upupupupup    actionup");
                    v.getParent().requestDisallowInterceptTouchEvent(false);
                    break;
            }
            return false;
        }
    });
```

没错，把 `pager` 换成 `v.getParent()`，这样就解决了我这个历史遗留问题，希望能为后面遇到该问题的小伙伴提供些许思路，感谢大家的观看，谢谢！

# 2016.08.02 更新另外两种解决方案

1 文章发布当天，有朋友回复，说 `v.getParent()` 方法就是 mMapView，想来也是，于是这样解决也是可以的：

```
    mMapView.getChildAt(0).setOnTouchListener(new View.OnTouchListener() {
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            switch (event.getAction()) {
                case MotionEvent.ACTION_MOVE:
                    KLog.w("upupupupup    actionmove");

                    mMapView.requestDisallowInterceptTouchEvent(true);
                    break;
                case MotionEvent.ACTION_UP:
                case MotionEvent.ACTION_CANCEL:
                    KLog.w("upupupupup    actionup");
                    mMapView.requestDisallowInterceptTouchEvent(false);
                    break;
            }
            return false;
        }
    });
```

2 后来遇到百度地图单指不能滑动问题，多方咨询后无果，因此改用高德地图，没想到高德地图一切运行良好，当然也遇到滑动冲突问题，这个解决和百度地图的有些不同，但找到了通用方案：

[fragment中加载高德地图出现滑动冲突解决](http://blog.csdn.net/qq_31469369/article/details/51766871)

看代码就明白了，就是重写 ViewPager 的 canScroll 方法，判断下，如果当前滑动的是百度地图或高德地图，则返回 true， 地图正常滑动。

所以，这个通用方法还是很棒的，代码稍微修改如下：

```
	@Override
	protected boolean canScroll(View v, boolean checkV, int dx, int x, int y) {
		return v.getClass().getName().equals("com.baidu.mapapi.map.MapView")
				|| v.getClass().getName().equals("com.amap.api.maps.MapView")
				|| super.canScroll(v, checkV, dx, x, y);
	}
```

最后，非常感谢您的阅读，有任何疑问，可以后面评论，谢谢！

神奇的安卓开发网站：[http://androidcat.com/](http://androidcat.com/)

安卓开源库收集整理：[https://github.com/XXApple/AndroidLibs](https://github.com/XXApple/AndroidLibs)

分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！