---

layout: post
title: " Python 网络爬虫技术概述"
subtitle: "\"Introduction!\""
date: 2016-03-21
author: "fibears"
header-img: "img/in-post/header/post-bg-robot.jpg"
tags:
- Python
- Tool

---


## Python 基础知识

学习一门语言，最重要的是带着目的去学习。当你有了一定的目标后，你在学习的过程中将会事半功倍。比如我最近一直在关心汽车的价格，我打算从网上爬取一些价格数据和用户评论数据来分析当前汽车市场的状况。那么自然而然的，我联想到**Python**强悍的爬虫功能，进而探究如何利用这个工具实现上述的目标。

当你明确目标后，接下来就要开始学习了。那么问题来了？我们要如何快速有效地掌握这个工具呢？我认为，首先你需要快速了解并掌握**Python**语言的基本语法，在此我安利一个特别好的教程——[廖雪峰Python教程](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000)。当你认真阅读完这篇教程后，我相信你已经对**Python**这门语言有了一个大概的认识。接下来，直奔主题，勿忘初衷！本文主要介绍一些和爬虫相关的知识点，比如数据载体 HTML 的基础知识、**Python**中相应的爬虫库以及正则表达式的使用方法。

## HTML 基础知识

### HTML

浏览网页时，我们通过浏览器所看到的网站是由HTML语言所构成的。HTML是一种建立网页文件的语言，通过标记式的指令(Tag)，将影像、声音、图片、文字等信息显示出来。

HTML语言使用标记对的方法编写文件，通常使用“<标志名>内容</标志名>”来表示标志的开始和结束。

以下是 HTML 的一些常用标记符号：

- \<html>…\</html>：表示网页的开始与结束

- \<body>…\</body>：网页的主体部分
 
- \<p>…\</p>：创建一个段落

- \<br>…\</br>：创建一个回车换行

- \<div>…\</div>：用于排版大块的HTML段落

- \<h1>\</h1>…\<h6>\</h6>：不同层级的标题

- \<img src=“…”>：处理图像，将文件名赋值给标志对的src属性

- \<a href=“…”>…\</a>：链接标记符

- \<a name=“…”>…\</a>：创建一个新标签

### URL

URL，即[统一资源定位符](http://baike.baidu.com/link?url=V8utLRCzlp15ieAoLCYg2usXhaor5Ij9reO6wNmq-4-UXNZD8DKd_mAiz3fEGu_-xrtHvPEhYUcYs53-tmIl4_)(Uniform Resource Locator)，它是对互联网上资源的位置和访问方法的一种简洁表示。互联网上的每个文件都有一个唯一的 URL ，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

URL通常由三部分组成：

- 模式/协议: http(超文本传输协议), https, ftp....
- 服务器或IP地址
- 路径和文件名

### HTTP

超文本传输协议(HyperText Transfer Protocol)是互联网上应用最为广泛的一种网络协议。设计HTTP的初衷是为了提供一种发布和接受HTML的方法。通过HTTP或HTTPS协议请求的资源由URL标识。

通常，由 HTTP 客户端发起一个请求，创建一个到服务器指定端口（默认是80端口）的 TCP 连接。HTTP 服务器则在那个端口监听客户端的请求。一旦收到请求，服务器会向客户端返回一个状态，比如"HTTP/1.1 200 OK"，以及返回的内容，如请求的文件、错误消息、或者其它信息。

HTTP/1.1 协议中定义了操作资源的八种不同方法，其中最基本的方法有4种：GET, POST, PUT, DELETE，它们分别对应着对这个资源的查、改、増、删4个操作。

- GET 请求的数据会附在 URL 之后（就是把数据放置在 HTTP 协议头中），以 ? 分割 URL 和传输数据，参数之间以 & 相连。
- POST 把提交的数据则放置在是 HTTP 包的包体中。

	备注：[GET和POST之间的差别](http://www.cnblogs.com/hyddd/archive/2009/03/31/1426026.html)
	
### Cookie

Cookie 是指某些网站为了辨别用户身份、进行 session 跟踪而储存在用户本地终端上的数据。Cookie 是由服务器端生成，发送给 User-Agent (一般是浏览器)，浏览器会将 Cookie 的 key/value 保存到某个目录下的文本文件内，下次请求同一网站时就发送该Cookie 给服务器(前提是浏览器设置为启用 cookie)。比如很多网站需要登录后才能访问，此时我们需要先模拟登录网页并将cookie保存下来，接下来再利用该 cookie 去抓取其他网页的内容。

	
## 网络爬虫技术简介

当我们访问网页时，首先通过浏览器向服务器发送请求(Request)，服务器接到我们的请求后会返回一个响应(Response)。网络爬虫技术就是通过程序模拟访问`URL`地址，并获得其返回的内容。因此网页抓取的一般逻辑是这样的：

1. 准备好对应的 HTTP 请求(Request)并提交
2. 获得返回的 HTTP 响应(Response)和网页源码，然后再从源码中提取出我们想要的数据

## Python 爬虫库的用法

**Python**中用于网页抓取的库是`urllib*`和`Requests`，本文中主要介绍前者的一些简单使用方法。对于简单的静态网页，我们只需将相应的 URL 传入 urlopen 函数中就能把网页扒下来：

```python
# 载入urllib2库
import urllib2
# 构建目标网址的URL，并将其传入urlopen函数中
url = "http://www.baidu.com"
response = urllib2.urlopen(url)
# 将扒下来的网页信息打印出来
print response.read()
```

但是现在大多数网页都是动态网页，需要我们动态地传递参数给它。因此我们需要事先构建一个 Request 对象，然后将需要上传的数据和链接打包好传递给 urlopen 函数。

```python
import urllib
import urllib2

# GET 方法
# 从源码中获取需要传递数据的关键字
Data = urllib.urlencode({"UserName":"xxxx","Passwd":"xxxx"})
# 直接将 Data 附在链接后面
url = 'http://idstar.xmu.edu.cn/amserver/UI/Login?goto=http://i.xmu.edu.cn' + "?" +Data
request = urllib2.Request(url)
response = urllib2.urlopen(request)
print response.read()

# POST 方法
# 从源码中获取需要传递数据的关键字
Data = urllib.urlencode({"UserName":"xxxx","Password":"xxxx"})
# 将头文件、Data和URL打包成request然后发送给urlopen函数
headers = {'User-Agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3)'}
url = 'http://event.wisesoe.com/Logon.aspx'
request = urllib2.Request(url, Data, headers)
response = urllib2.urlopen(request)
print response.read()

```
> **Reference:** [urllib2](https://docs.python.org/2/library/urllib2.html); [urllib2高级用法](http://cuiqingcai.com/954.html)


## 正则表达式

正则表达式(regex)提供了一种灵活的在文本中搜索或匹配字符串模式的方法。我们通常可以利用正则表达式这个工具从网页中提取出我们想要的信息。**Python**中的`re`模块负责正则化处理字符串表达式。该模块中的函数可以分为三个大类：模式匹配、替换和拆分。

> **正则表达式常用函数**
> 
> findall: 返回字符串中所有的非重叠匹配模式。
> 
> match: 从字符串起始位置匹配模式
> 
> search: 扫描整个字符串以匹配模式
> 
> split: 根据找到的模式将字符串拆分为数段
> 
> sub: 将字符串中所有的模式替换为指定表达式
> 
> 
由于正则表达式涉及的内容非常多，有兴趣的读者可以参考以下几个链接===>
[re](https://docs.python.org/2/library/re.html);
[正则表达式30分钟入门教程](http://deerchao.net/tutorials/regex/regex-1.htm);
[Learn Regex The Hard Way](http://regex.learncodethehardway.org/book/)

## XPath 

上一节，我们简单介绍了正则表达式的概念，它所涉及的内容非常多，如果一个正则匹配稍有差池，那么程序可能就处在死循环之中。而且对于初学者而言，要写一个比较通用的正则表达式还是挺有难度的。但是没关系，我们可以利用强大的 `lxml` 解析器来解析网页内容。

lxml 是一个用来处理 XML 的第三方 Python 库，它在底层封装了用 C 语言编写的 libxml2 和 libxslt，并以简单强大的 Python API，兼容并加强了著名的 ElementTree API。据说安装 lxml 时会遇到很多问题(我表示没印象= =！)，安装过程中遇到问题的小伙伴可以参考下[官方说明文档](http://lxml.de/installation.html)。

XPath 是一门在 XML 文档中查找信息的语言，在 XPath 语境中，XML 文档被视作节点树，节点树的根节点也被称作文档节点。XPath 将节点树中的节点(Node)分为七类：元素(Element)，属性(Attribute)，文本(Text)，命名空间(Namespace)，处理指令(Processing-instruction)，注释(Comment)和文档节点(Document nodes)。XPath 使用路径表达式来选取 XML 文档中的节点或者节点集。下面是几个常用的路径表达式：

> nodename : 选取此节点的所有子节点
> / : 从根节点选取
> 
> // : 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置
> 
> . : 选取当前节点
> 
> .. : 选取当前节点的父节点
> 
> @ : 选取属性
> 
> \* : 匹配符，可用来选取未知的 XML 元素
> 
> 逻辑运算符
> 
> contains() & matches() : 模糊匹配函数
> 

```python
# Example
import urllib2
# 载入解析器
from lxml.html import parse

url = 'http://event.wisesoe.com/Logon.aspx'
parsed = parse(urllib2.urlopen(url))

result = parsed.xpath('//div[@class="save-operation"]/a/@href')

```

[XPath REFERENCE](http://plasmasturm.org/log/xpath101/)














