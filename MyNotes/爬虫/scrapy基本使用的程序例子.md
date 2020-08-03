###### 一:

\# -*- coding: utf-8 -*-

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

​            yirld scrapy.Request(

​                mext_url,

​                callback=self.parse

​            )

​            \# 如果处理方式不一样,就在定义一个parse,

​    \#         yirld scrapy.Request(

​    \#             mext_url,

​    \#             callback=self.parse1

​    \#         )

​    \# def parse1(self,response):