title: java中String和InputStream的转换
date: 2015-12-20 16:22:57
categories: java
tags: [java,InputStream]
---

博客：	[安卓之家](http://jp1017.gitcafe.io/)
微博：	[追风917](http://weibo.com/1321395433/profile?topnav=1&wvr=6)
CSDN：	[蒋朋的家](http://blog.csdn.net/u010331406)
简书：	[追风917](http://www.jianshu.com/users/8cb49b5ad78b/latest_articles)
博客园：	[追风917](http://www.cnblogs.com/jp1017/)

InputStream提供的是字节流的读取，而非文本读取，用Reader读取出来的是char数组或者String ，使用InputStream读取出来的是byte数组。
Reader类及其子类提供的字符流的读取char，inputStream及其子类提供字节流的读取byte，所以 FileReader类是将文件按字符流的方式读取，FileInputStream则按字节流的方式读取文件；InputStreamReader可以将读如stream转换成字符流方式，是reader和stream之间的桥梁。

<!--more-->

1 String –> Inputstream

```java
public InputSteam getInputStreamFromString(String str){
    InputSteam in=new ByteArrayInputStream(str.getBytes());
}
```

2 InputStream –> String

```java
public static String convertStreamToString(InputStream is) {      
    StringBuilder sb = new StringBuilder();      
    String line = null;      
    try {
        BufferedReader reader = new BufferedReader(new InputStreamReader(is));      		
        while ((line = reader.readLine()) != null) {      
            sb.append(line);      
        }   
    } catch (IOException e) {
        e.printStackTrace();      
    } finally {      
        try {
            is.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    return sb.toString();      
}
```


分享是一种美德，更是一种生活方式！！

>也许你会说我是一个梦想者，但我不是唯一的一个。

>悦分享，越快乐^_^

欢迎交流，转载请注明出处，谢谢！