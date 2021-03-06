---
layout: post
author: LIU,HONGYANG
tags: [Java]
---



本文主要讲述自己采用Java语言爬取网络信息的过程

主要是用的组件是[Jsoup](https://jsoup.org/)



```xml
<dependency>
    <groupId>org.jsoup</groupId>
    <artifactId>jsoup</artifactId>
    <version>1.11.3</version>
</dependency>
```



```java
public class HtmlParser{

    public static void main(String[] args){
    
        String url = "https://search.jd.com/Search?keyword=java";
        
        //解析网页
        Document document = Jsoup.parse(new URL(url),30000);

        Element element = document.getElementById("J_goodsList");
        
        System.out.println(element.html());
        
        Element elements = element.getElementByTag("li");
        
        for(Element element1: elements){
            String img = el.getElementByTag("img").eq(0).attr("src");
        
        }
        
    }
}
```





##### Demo1

使用Java获取a标签内容：



````java
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import javax.swing.event.DocumentEvent;
import java.io.IOException;

public class lianjia{
    public static void main(String[] args){

        try{

            Document doc = Jsoup.connect("http://jsoup.org").get();
            Element link = doc.select("div.content").first();
            System.out.println(link.select("a").text());
            
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
````



讲解:



```java
Document doc = Jsoup.connect("http://jsoup.org").get();
Element link = doc.select("div.content").first();
System.out.println(link.select("a").text());
```



其中Jsoup获取网络link, `div.content` 用来获取div标签下class=content的节点。`text()`用来获取文本





##### Demo2



```java
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import javax.swing.event.DocumentEvent;
import java.io.IOException;

public class lianjia{
    public static void main(String[] args){

        try{

            Document doc = Jsoup.connect("https://bj.lianjia.com/ershoufang/").get();
            Elements elements = doc.select("ul.sellListContent");


            for(Element item: elements){

                String title = item.select("div.title").select("a").first().text();
                System.out.println(title);

                String address = item.select("div.address").select("div.houseInfo").first().text();
                System.out.println(address);

                String totalPrice = item.select("div.priceInfo").select("div.totalPrice").select("span").first().text();
                System.out.println(totalPrice);

                String unitPrice = item.select("div.priceInfo").select("div.unitPrice").select("span").first().text();
                System.out.println(unitPrice);

            }


        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



讲解：

Demo2的作用主要是通过使用Jsoup来连接链家网，获取网络信息的内容。

这里要注意的是.first()返回的是单个Element而去掉.first()则是多个Elements

多个Elements需要使用迭代来进行遍历

` for(Element item: elements){}`



##### Demo3

```java
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import javax.swing.event.DocumentEvent;
import java.io.IOException;

public class lianjia{
    public static void main(String[] args){

        try{

            Document doc = Jsoup.connect("https://bj.lianjia.com/ershoufang/").get();

            
            Element element1 = doc.select(" #content > div.leftContent > ul > li:nth-child(1) > div.info.clear > div.title > a").first();
            System.out.println(element1.text());



            Element element2 = doc.select("#content > div.leftContent > ul > li:nth-child(1) > div.info.clear > div.address > div.houseInfo").first();
            System.out.println(element2.text());
            


        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



讲解:

最后一个demo是为了验证浏览器的`selector`而使用的



![image-20200331133618804](https://tva1.sinaimg.cn/large/00831rSTgy1gdd1xuv8wtj30so0iyqh5.jpg)





以Google浏览器为例，用户仅仅需要使用Copy selector即可选择内容，将元素打印，非常方便

```java
 Element element1 = doc.select(" #content > div.leftContent > ul > li:nth-child(1) > div.info.clear > div.title > a").first();
System.out.println(element1.text());
```

