---
title : 07-Scrapy框架
date : 2021-6-20
tags: 网络爬虫
---



# Scrapy框架


#### 什么是框架？
就是一个集成了很多功能并且具有很强通用性的一个项目模板。
#### 如何学习框架？
专门学习框架封装的各种功能的详细用法。
#### 什么是scrapy？
爬虫中封装好的一个明星框架。功能：高性能的持久化存储，异步的数据下载，高性能的数据解析，分布式

#### scrapy框架的基本使用

##### 环境的安装：

	-mac or linux: pip install scrapy
	-windows:
		-pip install wheel
		-下载并安装twisted:			https://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted
	
	pip install 下载的whl文件名
	-pip install pywin32
	-pip install scrapy
	测试：在终端里录入scrapy指令，没有报错即表示安装成功。
	-创建一个工程：scrapy startproject xxxPro
	-cd xxxPro
	-在spiders子目录中创建一个爬虫文件
		-scrapy genspider spiderName www.xxx.com
	执行工程：
		-scrapy crawl spiderName

#### scrapy数据解析

-scrapy持久化存储
	-基于终端指令：
		-要求：只可以将parse方法的返回值存储到本地的文本文件中	
		-注意：持久化存储对应的文本文件的类型只可以为：'json','jsonlines','csv','xml'
		-指令： scrapy crawl xxx -o filePath
		-好处：简洁高效便捷
		-缺点：局限性比较强（数据只可以存储到指定后缀的文本文件中）
	-基于管道：
		-编码流程：
			-数据解析
			-在item类中定义相关的属性
			-将解析的数据封装存储到Item类型的对象
			-将item类型的对象提交给管道进行持久化存储的操作
			-在管道类的process_item中要将其接受到的Item对象中存储的数据进行持久化存储操作。
			-在配置文件中开启管道

​		好处：通用性强。
​		缺点：编码流程繁琐
​	-面试题：将爬取到的数据一份存储到本地一份存储到数据库
​		-管道文件中的一个管道类对应的是将数据存储到一种平台
​		-爬虫文件提交的item只会给管道文件中第一个被执行的管道类接收
​		-process_item中的return item表示将item传递给下一个即将被执行的管道类

```python
import scrapy
#需求：获取糗事百科中的段子
class QiubaiSpider(scrapy.Spider):
    name = 'qiubai'
    #allowed_domains = ['www.xxx.com']
    start_urls = ['http://www.qiushibaike.com/text/']

    def parse(self, response):
        #解析：作者的名称+段子内容
        div_list=response.xpath('//*[@class="col1 old-style-col1"]/div')
        for div in div_list:
            #xpath返回的是列表，但是列表元素一定是Selector类型的对象
            #extract可以将Selector对象中data参数存储的字符串提取出来
            #author=div.xpath('./div[1]/a[2]/h2/text()')[0].extract()
            author = div.xpath('./div[1]/a[2]/h2/text()').extract_first()
            #列表调用了extract之后，则表示将列表中每一个Selector对象中data对应的字符串提取了出来
            content=div.xpath('./a[1]/div/span//text()').extract()
            content = ''.join(content)
            print(author,content)
```



#### 基于spider的全站数据爬取

-就是将网站中某板块下的全部页码对应的页面数据进行爬取
-需求：爬取校花网中的图片的名称
-实现方法：
	-将所有页面的Url添加到start_urls列表（不推荐）
	-自行手动进行请求发送（推荐）
		-手动请求发送：
			yield scrapy.Request(url,callback):callback专门用作于数据解析

```python
import scrapy
class XiaohuaSpider(scrapy.Spider):
    name = 'xiaohua'
    start_urls=['http://www.521609.com/meinvxiaohua/']
    #生成一个通用的url模板
    url='http://www.521609.com/meinvxiaohua/list12%d.html'
    page_num=2
    def parse(self,response):
        li_list=response.xpath('/html/body/div[4]/div[2]/div[2]/ul/li')
        for li in li_list:
            img_name=li.xpath('./a[2]/b/text()').extract_first()
            print(img_name)
        if self.page_num<=4:
            new_url=format(self.url%self.page_num)
            self.page_num+=1
            #手动请求发送：callback回调函数是专门用作于数据解析
            yield scrapy.Request(url=new_url,callback=self.parse)
```



#### 五大核心组件

##### 引擎（Scrapy）

用来处理整个系统的数据流处理，触发事务（框架核心）

##### 调度器（Scheduler）

用来接收引擎发过来的请求，压入队列中，并在引擎再次请求的时候返回。可以想象成一个url。

##### 下载器（Downloader）

用于下载网页内容，并将网页内容返回给蜘蛛（Scrapy下载器是建立在twisted这个高效的异步模型上的）

##### 爬虫（Spiders）

爬虫是主要干活的，用于从特定的网页中提取自己需要的信息，即所谓的实体（Item）。用户也可以从中提取数据。

##### 项目管道（Pipeline）

负责处理爬虫从网页中抽取的实体，主要功能是持久化实体、验证实体的有效性、清除不需要的信息。



#### ImagesPipeline

##### 基于scrapy爬取字符串类型的数据和爬取图片类型的数据区别？

​	-字符串：只需要基于xpath进行解析且提交管道进行持久化存储
​	-图片:xpath解析出图片的src的属性值。单独的对图片地址发起请求获取图片二进制类型的数据

##### ImagesPipeline:
​	-只需要将img的src的属性值进行解析，提交到管道，管道就会对图片的src进行请求发送获取图片的二进制类型的数据，且还会帮我们进行持久化存储。

-需求：爬取站长素材中的高清图片
-使用流程：
	-数据解析（图片的地址）
	-将存储图片地址的item提交到指定的管道类
	-在管道文件中自定制衣柜基于ImagesPipeLine的一个管道类
		-get_media_request
		-file_path
		-item_completed
	-在配置文件中：
		-指定图片存储的目录：IMAGES_STORE='./imgs_bobo'
		-指定开启的管道：自定制的管道类



#### 中间件

-下载中间件：
	-位置：引擎和下载器之间
	-作用：批量拦截到整个工程中发起的所有的请求和响应
	-拦截请求：
		-UA伪装：process_request
		-代理IP：process_exception:return request                                                                                                                                                                                                                                                                
	-拦截响应：
		-篡改响应数据或者响应对象
		-需求：爬取网易新闻中的新闻数据（标题和内容）
			-1.通过网易新闻的首页解析出五大板块对应的详情页url（没有动态加载）
			-2.每一个板块对应的新闻标题都是动态加载出来的（动态加载）
			-3.通过解析出每一条新闻详情页的Url获取详情页的页面源码，解析出新闻内容



#### CrawlSpider

类，Spider的一个子类
全站数据爬取的方式
	-基于Spider：手动请求
	-基于CrawlSpider
CrawlSpider的使用：
	-创建一个工程
	-cd XXX
	-创建爬虫文件（CrawlSpider）：
		-scrapy genspider -t crawl xxx www.xxx.com
		-链接提取器：
			-作用：根据指定的规则（allow）进行指定链接的提取
		-规则解析器：
			-作用：将链接提取器提取到的链接进行指定规则（callback的解析）

##### 需求：爬取sun网站的编号新闻标题、新闻内容，标号

​		-分析：爬取的数据没有在同一张页面中。
​		-1.可以使用链接提取器提取所有的页码链接
​		-2.让链接提取器提取所有的新闻详情页的链接

