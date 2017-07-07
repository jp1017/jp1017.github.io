title: Android 读取txt文件并以utf-8格式转换成字符串
date: 2015-12-09 20:04:16
categories: [Android]
tags: [Android]
---


博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

# 使用EncodingUtils
---

今天用到了城市选择三级联动的库，用的这个：https://github.com/yangjinbo2014/CityPicker

需要读取txt里的城市信息，转换成字符串处理。

项目里用的代码是这样的:

<!--more-->

```java
    InputStream inputStream = mContext.getResources().getAssets().open("address.txt");
	byte[] arrayOfByte = new byte[inputStream.available()];
	inputStream.read(arrayOfByte);
	String address = EncodingUtils.getString(arrayOfByte, "utf-8");
```

EncodingUtils工具类在org.apache.http.legacy.*包里，而这个包在sdk23成了一个jar包里，不推荐用了，因此推荐用下面的方法，java下读取流的转换。

# 使用InputStreamReader
---

直接上代码了：

```java
	public static String getString(InputStream inputStream) {
		InputStreamReader inputStreamReader = null;
		try {
			inputStreamReader = new InputStreamReader(inputStream, "utf-8");
		} catch (UnsupportedEncodingException e1) {
			e1.printStackTrace();
		}
		BufferedReader reader = new BufferedReader(inputStreamReader);
		StringBuilder sb = new StringBuilder("");
		String line;
		try {
			while ((line = reader.readLine()) != null) {
				sb.append(line);
				sb.append("\n");
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
		return sb.toString();
	}
```

可以把这个放到字符串处理的工具类里，好了，就这样了，搞定！



分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！