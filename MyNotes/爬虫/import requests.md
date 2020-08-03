一：
终端：
>>> import requests
>>> r=requests.get("http://www.baidu.com")
>>> r.status_code
>>> 200
>>> r.encoding ='utf-8'
>>> r.text

r.request.headers   查看User-Agent

 kv={'user-agent':'mozilla/5.0'}
 //定义一个浏览器名

url="https://www.xxxxx.com"

r=requests.get(url,headers=kv)
编辑器：
import requests
url="https://item.jd.com/100000822981.html"
try:
    kv={'user-agent':'Mooozilla/5.0'}
    r=requests.get(url,headers=kv)
    r.raise_for_status()
    r.encoding=r.apparent_encoding
    print(r.text[:1000])
except:
    print("爬去失败")
    
二：
>>> import requests
>>> kv={'wd':'Python'}
>>> r=requests.get("http://www.baidu.com/s",params=kv)
//附加在url上
>>> r.status_code
200
>>> r.request.url  
//查看提交url的地址
'http://www.baidu.com/s?wd=Python'
>>> len(r.text)	 
//查看返回的长度
430911
>>> 

三：保留网上图片
>>> import requests
>>> path="/home/liz/abc.jpf" 
//设置保留图片的路径abc为命名.jpg为图片格式
>>>url="http://img0.dili360.com/ga/M00/46/FC/wKgBy1iv0WmAaXa1AE7LkZQs2kI077.tub.jpg@!rw9"
>>> r=requests.get(url)
>>> r.status_code
200
>>> with open(path,'wb') as f:
...     f.write(r.content)
//网上图片是一个二进制代码，content是request中转化二进制的指令，这一行前注意缩进，如果在终端进行要加Tob，不然会显示error
>>>f.close()然后去保留的路径查看图片

编辑器：
import requests
import os	//引入一个os库
url="http://img0.dili360.com/ga/M00/46/FC/wKgBy1iv0WmAaXa1AE7LkZQs2kI077.tub.jpg@!rw9"
root="/home/liz/图片"创建一个根目录
path=root+url.split('/')[-1]//进行切片操作，对url最后一个‘/’之后的名字切片，是保存的图片名为原来的图片名
try:
    if not os.path.exists(root)://如果没有根目录，创建一个根目录
        os.mkdir(root)
    if nor os.path.exists(path)://如果没有路径，创建一个路径
        r=requests.get(url)
        with open(path,'wb') as f:
            f.write(r.content)
            f.close()
            print("文件保存成功")
        else:
            print("文件已存在")
except:
    print("爬去失败")

四：使用bs4解析html

>>> from bs4 import BeautifulSoup//引入bs4库中的BeautifulSoup
>>> import requests
>>> r=requests.get("https://python123.io/ws/demo.html")
>>> demo=r.text
>>> soup=BeautifulSoup(demo,"html.parser")//用html.parser对html进行解析
>>> r.text
>>> '<html><head><title>This is a python demo page</title></head>\r\n<body>\r\n<p class="title"><b>The demo python introduces several python courses.</b></p>\r\n<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:\r\n<a href="http://www.icourse163.org/course/BIT-268001" class="py1" id="link1">Basic Python</a> and <a href="http://www.icourse163.org/course/BIT-1001870001" class="py2" id="link2">Advanced Python</a>.</p>\r\n</body></html>'
>>> soup.title//查看其中的title标签
<title>This is a python demo page</title>
>>> tag=soup.a
>>> tag//查看a标签
>>> <a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>
>>> soup.a.parent.name //查看a标签的父级标签
>>> 'p'
>>> soup.a.parent.parent.name //查看a标签父级的父级
>>> 'body'
>>> tag=soup.a
>>> tag.attrs // 查看a标签的属性
 >>> {'href': 'http://www.icourse163.org/course/BIT-268001', 'class': ['py1'], 'id': 'link1'} //标签属性是一个字典
>>> tag.attrs['class']//因为是字典，所以用字典来提取其中class属性的信息
>>> ['py1']
>>> type(tag.attrs) // 查看标签属性的类型
<class 'dict'> //标签的属性的类型是字典
>>> type(tag) //查看标签的类型
<class 'bs4.element.Tag'> //标签的类型是标签，bs4中将标签定义了一个特殊的类型
>>> soup.a
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a>
>>> soup.a.string  //查看标签中的字符串
'Basic Python'

![](/home/liz/图片/2019-02-10 14-42-33 的屏幕截图.png)
>>> soup.head
\<head><title>This is a python demo page</title></head>
>>> soup.head.contents //查看head标签的子标签
[<title>This is a python demo page</title>]、
>>> soup.body.contents //查看body的子标签
['\n', <p class="title"><b>The demo python introduces several python courses.</b></p>, '\n', <p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>, '\n']
>>> len(soup.body.contents) //查看标签一共有多少个儿子节点
5
>>> soup.body.contents[3] //查看其中第三个节点，节点从0开始
<p class="course">Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:
<a class="py1" href="http://www.icourse163.org/course/BIT-268001" id="link1">Basic Python</a> and <a class="py2" href="http://www.icourse163.org/course/BIT-1001870001" id="link2">Advanced Python</a>.</p>
>>> soup.a.next_sibling  //返回按照html文本顺序的下一个平行节点标签
' and '
>>> soup.a.previous_sibling//返回按照html文本顺序的上一个平行节点标签
'Python is a wonderful general-purpose programming language. You can learn Python from novice to professional by tracking the following courses:\r\n'







###### 用chardet解决乱码问题

import requests

r=requests.get('http://www.xxxxxx.com')

print chardet.detect(r.content)

r.encoding=chardet.detect(r.content)['encoding']

print r.text