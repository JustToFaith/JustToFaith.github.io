```python
setting中
IMAGES_STORE='PATH'

item中
yield item 回管道

pipelines中

def get_media_requests(self,item,info):
    image_link = item['imagelink']#图片链接
    yield scrapy.Request(image_link)

```

