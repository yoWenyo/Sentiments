# 关于使用Scrapy框架的感想

## 框架爬取网页内容，步骤

### 1.创建Scrapy项目 xxx
  
```cmd
    scrapy startproject xxx
```

### 2.编写items.py文件，定义item容器

```Python
    name = scrapy.Field() //name:是需要内容名称，后面需要在spider里面实例化items，name就是保存的变量，类的名字，最好是爬虫名字例如 DmozItem
```

### 3.编写爬虫，spider爬虫类

>#### 主要内容
>
>* 1.用于下载的初始URL
>* 2.如何跟进网页中的链接
>* 3.如何分析页面中的内容
>* 4.提取生成的Item的方法
>&emsp;在 项目文件夹/spiders 里面，编写 项目名_spider.py 例如:dmoz_spider.py 内容包括：DomzSpider 类。  
>
>#### 具体参考代码如下  

```python
import scrapy

# 验证完毕，可以导入items类进行类实例化传输结果
from tutorial.items import DmozItem

class DmozSpider(scrapy.Spider):
    name = "dmoz"
    allowed_domains = ['dmoztools.net']
    # 限定爬虫访问的域名，防止范围过于宽泛
    start_urls = [
        'http://www.dmoztools.net/Computers/Programming/Languages/Python/Books/',
        'http://www.dmoztools.net/Computers/Programming/Languages/Python/Resources/'
    ]

    def parse(self, response):
        # 测试爬虫可以正常爬取网页代码，合并保存下来
        # filename = response.url.split('/')[-2]
        # with open(filename, 'wb') as f:
        #     f.write(response.body)

        # 接下来开始使用 xpath 对代码进行筛选，得到自己想要的内容
        sel = scrapy.selector.Selector(response)
        items = []
        sites = sel.xpath(
            '//div/div[@class="title-and-desc"]')
        for site in sites:
            # 下面的验证爬虫正常工作，打印出来需要的内容，之后就可以传入实例化的item
            # title = site.xpath('a/div/text()').extract()
            # link = site.xpath('a/@href').extract()
            # desc = site.xpath('div/text()').extract()
            # print(title, link, desc)
            item = DmozItem()
            item['title'] = site.xpath('a/div/text()').extract()
            item['link'] = site.xpath('a/@href').extract()
            item['desc'] = site.xpath('div/text()').extract()
            items.append(item)
        return items
```

## xpath

### 简介

>#### 在scrapy 中使用基于 xpath 和 css 的表达式机制 scrapy selectors。 selector 是一个选择器，包含四个基本方法
>
>* 1.xpath(): 传入xpath表达式，返回该行表达式对应所有节点的selector list 列表
>* 2.css():传入css表达式，返回该表达式对应的所有节点的selector list列表
>* 3.extract():序列化该节点为Unicode字符串并返回list
>* 4.re():根据传入的正则表达式对数据进行提取，返回Unicode字符串list列表

### 使用方法

>#### 使用案例  
>
>* /html/head/title:选择HEML文档中\<head>标签内的\<titel>元素
>* /html/head/title/text():选择HEML文档中\<head>标签内的\<titel>元素包含的内容, 即 : \<title>这个地方的内容\</title>
>* //td:选择所有的\<td> 元素
>* //div[@class="mine"]:选择所有的\<div class="mine"> 的标签
>* /a/@href:选择\<a> 元素里面的href属性的值
>* /a/text():选择\<a> 元素里面包含的内容，即第二条的概念
>
> 注意: 此外在第二次进行xpath解析时，第二次xpath的参数开头不需要带 "/"

## 运行爬虫

> 在cmd里面，到项目所在文件夹，运行下面的命令
>
> ```cdm
> scrapy crawl -o items.json -t json
> ```
>
> -o items.json 输出文件到 items.json  
> -t json 使用json 格式输出获取到的内容(items.py定义的)

## 参考链接

[Spiders官网文档][link1]  
[简书简介][link2]  
[小甲鱼B站教学视频][link3]  

[link1]:https://docs.scrapy.org/en/latest/topics/spiders.html
[link2]:https://www.jianshu.com/p/8e78dfa7c368
[link3]:https://www.bilibili.com/video/av4050443?p=54