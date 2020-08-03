### 用Shell检查是否xpath定点

scrapy shell "www/xxxx.com" 模仿请求相应
response.xpath("   ")
response.xpath("    ").extract() 转换
item_list=response.xpath("  ").extract() 用列表存起来
for item in item_list:
	print item    打印出来

xpath标签后家/text以为取文本，不含标签