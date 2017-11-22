---
title: Python中的爬虫技巧
date: 2017-02-06 13:57:31
categories: Python
tags: [Python, crawl]
---

网页爬虫是一种非常有用的获取数据的方式，之前每次需要爬虫时都上网查一些资料，遂想写点东西，把用过的这些技巧都记录下来，以便日后方便使用。

<!-- more -->


## 普通的HTML爬虫

最普通的HTML爬虫分三步，第一步请求网页，得到网页的HTML文件；第二步解析网页；第三步从解析后的网页中提取数据。

#### 请求网页

请求网页好用的方式是 Python 中的 `Requests` 库，通过命令 `pip install requests` 来安装。

`Requests` 库的宣言是：

> HTTP for human.

给人类用的HTTP库。

简单一点的用法就是直接请求URL：

``` python
import requests

url = 'http://example.com'
r = requests.get(url)
```

这样以GET方式request一个url，得到一个Response类的结果，存储在对象r中。

但很多时候我们直接请求会失败，需要我们伪装成浏览器的样子，这时就需要加上headers(浏览器头)。并且直接用requests来请求，多次请求后，会出现请求次数太多被拒绝的问题，我们最好通过session来完成请求。

``` python
import requests

my_headers = {
    'User-Agent' : 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.95 Safari/537.36',
}
url = 'http://example.com'

s = requests.Session()
r = s.get(url, headers=my_headers)
```

#### 解析网页

在请求到网页数据后，我们需要将得到的数据进行解析。

一般用到的解析工具有 `BeautifulSoup` 和 `xpath(lxml)`。

我一般会用 `BeautifulSoup`，文档在[这里](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/)，安装方式也是通过pip来安装：`pip install bs4`。

使用如下：

``` python
from bs4 import BeautifulSoup

# ... (request a url)

soup = BeautifulSoup(r.text, 'html.parser')
# or soup = BeautifulSoup(r.text, 'lxml')
```

`r.text` 是需要被解析的网页内容，`html.parser` 和 `lxml`是解析器。

#### 提取数据

提取数据的方式有好几种，可以通过 `soup.find()` 来寻找指定标签，或者通过CSS选择器 `soup.select()` 来提取。我个人倾向于用后者，更加灵活准确。

具体用法参见[官方文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/)中相应内容。


## AJAX网页爬虫

现在随着前后端分离已经成为趋势，越来越多的网页数据不是写在HTML中，而是通过AJAX请求来得到的，这样我们通过请求HTML已经不能得到我们想要的东西，我们可以通过直接请求AJAX的URL来达到我们的目的。

这时我们需要花一些时间来观察，看看网页请求的 Resource 中，哪个是我们需要的。

使用Chrome浏览器，右键->检查元素，选到 `Network`，并刷新网页，这时左边的 `name` 栏中将会出现所有该网页请求的资源，依次查看每个资源的 `preview`，看哪个资源是数据，找到后，直接用 `requests` 请求该URL即可，一般这种资源都是JSON格式，在python中就是个字典。


## 其他爬虫手段

有的网页的数据既不是写在HTML中，也不是通过Ajax请求到的，这时之前的两种方法就不能爬到我们需要的数据，我们需要完全模拟浏览器行为才能爬取。使用的工具为 `Selenium + PhantomJS`。

`Selenium` 是一个强大的网络数据采集工具，最初是为网站自动化测试而开发的。近几年，他还被广泛用于获取精确的网站快照，因为他们可以直接运行在浏览器上。`Selenium` 可以让浏览器自动加载页面，获取需要的数据，甚至页面截屏，或者判断网站上某些动作上是否发生。

`PhantomJS` 是一个无头的浏览器，`PhantomJS` 会把网站加载到内存并执行页面上的 JavaScript，但是不会向用户展示网页的图形化界面，可以用来处理 cookie、JavaScript 及 header 信息，以及任何你需要浏览器协助完成的事情。

`Selenium` 可以通过pip安装： `pip install selenium`， `PhantomJS` 可以去官方网站下载得到。

具体用法如下：

``` python
from bs4 import BeautifulSoup
from selenium import webdriver

# 本地的phantomjs路径
PHANTOM_PATH = '/Users/patrick_psq/Downloads/phantomjs-2.1.1-macosx/bin/phantomjs'
# 需要爬取的URL
url = r'http://example.com'

# PhantomJS 浏览器地址
driver = webdriver.PhantomJS(executable_path=PHANTOM_PATH)
# 目标网页地址
driver.get(url)
# 解析html 存入一个bs对象中
bs_obj = BeautifulSoup(driver.page_source, 'lxml')
```


## 更多

爬虫的技巧还有很多，不同的网页都要用不同的方式去分析，比如还有模拟登陆、切换ip、识别验证码等等，日后遇到这些问题时，再继续学习。
