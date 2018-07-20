---
title: selenuim-phantomjs-java无法获取动态加载的元素
date: 2018-07-19 15:21:15
tags: [Java,phantomjs,selenuim,异常]
toc: true
---
iframe是js动态加载的，获取的页面没有iframe元素...
<!--more-->
### 问题现象

>1. <font color=red>**无法获取页面元素<最直观现象>**</font>
>2. iframe是js动态加载的，获取的页面没有iframe元素
>3. 富文本编辑框拿不到
>4. driver.findElement(By.id("iframe_id")) 抛出异常，无法找到元素
>5. driver.switchTo().frame(driver.findElement(By.id("iframe_id"))); 这个就更不用想了，连iframe_id都拿不到switch毛啊。


明明从chrome浏览器检查元素的时候可以看到iframe元素，
但是查看源代码的时候iframe就没有了...
![握草](https://user-images.githubusercontent.com/21979120/42928177-33126f12-8b69-11e8-9021-bea1f550974d.jpg)
通过查一些资料发现：
> 所谓源码，就是别人服务器发送到浏览器的原封不动的代码。
> 你那些在源码中找不到的代码，那是在浏览器执行js动态生成的。

好！既然知道了问题所在，那我们就...
搜一下有没有现成的解决方案：
> "jQuery如何获取动态添加的元素"
> "js如何获取动态添加的元素"
> ...

**都说要用**<font color=red>**jQuery on** </font>**函数，然后...**
<font color=red>**就走了大量的弯路，根本不好用好么！！！**</font>
<font color=red>**不好用！！！**</font>
<font color=red>**不好用！！！**</font>
<font color=red>**不好用！！！**</font>
![我已经用了洪荒之力了](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1532591012&di=0509f2e1fd1ec8db460ccaf5403a2d05&imgtype=jpg&er=1&src=http%3A%2F%2Fpic.kekenet.com%2F2016%2F1216%2F81441481888940.jpg)
<font color=red>**说实话，我有点方了...**</font>
<br><br>

### 解决方案
走投无路之下，我只能死马当活马医：
直接给最底层元素赋值:
```prettyprint
$("#textarea_id").val("hello world");
```
**哇，没想到成了，对你没有看错，成了！！！！**
![叉会腰](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=1444007451,2024641395&fm=27&gp=0.jpg)

<br><br>
### 完成代码
平生最恨贴代码不贴jar包的人！！！

```prettyprint
import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.NoSuchElementException;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.phantomjs.PhantomJSDriver;
import org.openqa.selenium.phantomjs.PhantomJSDriverService;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
.........
......
..

public static void setTextarea(WebDriver driver,String target){
    	String url = "https://bukemiaoshudeurl.com/"+target;
    	driver.get(url);
    	Thread.sleep(5000);
    	JavascriptExecutor jse = (JavascriptExecutor) driver ;
    	String script = "$(\"#textarea_id\").val(\"hello world\");\r\n" +
    			"$(\"#textarea_submit\").trigger(\"click\"); ";
    	jse.executeScript(script);
    }
```
maven 的pom.xml

```prettyprint
<dependency>
	    <groupId>org.seleniumhq.selenium</groupId>
	    <artifactId>selenium-java</artifactId>
	    <version>2.53.0</version>
	</dependency>
	<dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-remote-driver</artifactId>
        <version>2.53.0</version>
    </dependency>
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-server</artifactId>
        <version>2.53.0</version>
    </dependency>

	<dependency>
	    <groupId>com.codeborne</groupId>
	    <artifactId>phantomjsdriver</artifactId>
	    <version>1.2.1</version>
	    <exclusions>
	        <exclusion>
	            <groupId>org.seleniumhq.selenium</groupId>
	            <artifactId>selenium-remote-driver</artifactId>
	        </exclusion>
	        <exclusion>
	            <groupId>org.seleniumhq.selenium</groupId>
	            <artifactId>selenium-java</artifactId>
	        </exclusion>
	    </exclusions>
	</dependency>
```
兄弟只能帮你到这了，遇到问题不要慌，就是怼！

<br><br>
### 后续的坑
如果观察仔细的小伙伴可能会发现，我通过js提交了表单，页面直接重定向到主页了
但是，现在直接获取url或者源码发现**还是原来的提交页面**？？？
![黑人问号](https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1782530970,3939566258&fm=27&gp=0.jpg)
```prettyprint
public static void setTextarea(WebDriver driver,String target){
    	String url = "https://bukemiaoshudeurl.com/"+target;
    	driver.get(url);
    	Thread.sleep(5000);
    	JavascriptExecutor jse = (JavascriptExecutor) driver ;
    	String script = "$(\"#textarea_id\").val(\"hello world\");\r\n" +
    			"$(\"#textarea_submit\").trigger(\"click\"); ";
    	jse.executeScript(script);
      System.out.println(driver.getCurrentUrl());
      System.out.println(driver.getPageSource());
    }
```
试了很多办法，我没办法用当前页面的js获取到未来要跳转页面的内容啊.（懵逼脸）

经过一番努力...

只需要在执行完js后等待几秒就好了，<font color=red>对！是什么都不用做，等待页面加载完就可以了</font>...
感觉自己是在跟**空气**斗智斗勇...
![吐血](https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=3721664201,306620058&fm=27&gp=0.jpg)
```prettyprint
public static void setTextarea(WebDriver driver,String target){
    	String url = "https://bukemiaoshudeurl.com/"+target;
    	driver.get(url);
    	Thread.sleep(5000);
    	JavascriptExecutor jse = (JavascriptExecutor) driver ;
    	String script = "$(\"#textarea_id\").val(\"hello world\");\r\n" +
    			"$(\"#textarea_submit\").trigger(\"click\"); ";
    	jse.executeScript(script);
      Thread.sleep(10000);
      System.out.println(driver.getCurrentUrl());
      System.out.println(driver.getPageSource());
    }
```
