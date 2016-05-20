Scrapy 的安装和基础使用
---

官方地址：[http://scrapy.org/](http://scrapy.org/)

>	Scrapy，Python开发的一个快速,高层次的屏幕抓取和web抓取框架，用于抓取web站点并从页面中提取结构化的数据。Scrapy用途广泛，可以用于数据挖掘、监测和自动化测试。

>	Scrapy吸引人的地方在于它是一个框架，任何人都可以根据需求方便的修改。它也提供了多种类型爬虫的基类，如BaseSpider、sitemap爬虫等，最新版本又提供了web2.0爬虫的支持。
>

*	安装前准备

```shell
yum install libxml2-devel libxslt-devel
pip install Scrapy
wget wget https://pypi.python.org/packages/source/T/Twisted/Twisted-15.2.1.tar.bz2#md5=4be066a899c714e18af1ecfcb01cfef7
tar -jxvf Twisted-15.2.1.tar.bz2
python ./setup.py install
```

*	创建项目

```
scrapy startproject tutorial
```

* 创建任务

```python
import scrapy
class DmozSpider(scrapy.spiders.Spider):
    name = "dmoz"
    allowed_domains = ["dmoz.org"]
    start_urls = [
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/",
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Resources/"
    ]
    def parse(self, response):
        filename = response.url.split("/")[-2]
        with open(filename, 'wb') as f:
            f.write(response.body)
```


*	启动爬虫

```shell
scrapy crawl dmoz
```