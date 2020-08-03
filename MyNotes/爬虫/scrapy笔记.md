###### 一:logging模块

import scrapy

class Tx1Spider(scrapy.Spider):

​    name = 'tx1'

​    allowed_domains = ['.itcast.cn']

​    start_urls = ['http://www.itcast.cn/channel/teacher.shtml']

​    def parse(self, response):

​        \# 处理start_url地址对应的响应

​        \# ret1=response.xpath("//div[@class='tea_con']//h3/text()").extract()   #.extract()提取其中文字

​        \# print(ret1)

​        \# 分组

​        li_list=response.xpath("//div[@class='tea_con']//li")

​        for li in li_list:

​            item={}

​            item["name"]=li.xpath(".//h3/text()").extract_first()

​            item["title"]=li.xpath(".//h4/text()").extract_first()

​            print(item)

​            yield item #把item传到pipelines

​        \# 找到下一个url地址

​        next_url=response.xpath("//a[@id='next']/@href").extact_fisrt

​        if next_url != "jacascript:;":

​            next_url="http://hr.content.com/"+next_url

​            \# yield返回一个url请求,因为处理方式相同,所以返回self.parse

​            \# yirld scrapy.Request(

​            \#     mext_url,

​            \#     callback=self.parse

​            \# )

​            如果处理方式不一样,就在定义一个parse,

​            yirld scrapy.Request(

​                mext_url,

​                callback=self.parse1,

​                meta={"item":item}         #mate字典的形式,可以添加一个建

​            )

​    def parse1(self,response):

​        response.meta["item"]

###### scrapy.Request常用方法

scrapy.Request(url [,callback,method='GET',headers,body.cookies,meta,dont_filter=False])

callback:指定传入的url交给那个解析函数去处理

meta:实现在不同的解析函数中传递数据,meta默认会携带部分信息,比如下载延迟,请求深度等

dont_filter:让scrapy的去重不会过滤当前url,scrapy默认url去重的功能,对需要重复的请求的url有重要用途

注意:cookies不能放到headers里面去,在url不变但是内容会变的url地址,我们通常要重复请求这个url地址,以获得最新信息(想百度贴吧,url不变但是内容时刻发生变化)

