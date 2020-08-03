### 获取请求参数

from flask import request

就是 Flask中表示当前请求的 request对象，request对象中保存了一次HTTP请求的一切信息。

|  属性   |              说明              |      类型      |
| :-----: | :----------------------------: | :------------: |
|  data   | 记录请求的数据，并转化为字符串 |       *        |
|  form   |      记录请求中的表单数据      |   MultiDict    |
|  args   |      记录请求中的查询参数      |   MultiDict    |
| cookies |     记录请求中的cookie信息     |      Dict      |
| headers |       记录请求中的报文头       | EnvironHeaders |
| method  |     记录请求使用的HTTP方法     |    GET/POST    |
|   url   |       记录请求的url地址        |     string     |
|  files  |       记录请求上传的文件       |       *        |

```python
# -*- coding: utf-8 -*-
from flask import Flask, request


app = Flask(__name__)


# 接口 api

# 127.0.0.1:5000/index?city=shenzheng  问号后面的叫做查询字符串 QueryString
@app.route("/index", methods=['GET', 'POST'])
def index():
    # request中包含了前端发过来的所有请求数据
    # form和data是用来提取请求题数据
    # 通过request.form可以直接提取请求中的表单格式的数据，是一个类字典的对象
    # 通过get方法只能拿到多个重名参数的第一个
    name = request.form.get("name")  # 在不确定返回值中是否一定有name时，不用[]提取参数，用get方法可以保证在没有返回值的情况下，程序不会蹦，提高程序的健壮性
    age = request.form.get("age")
    name_lis = request.form.getlist("name")  # 当有重名多个值时用getlist这个方法，可以提取全部重名的参数

    # args是用来提取url中的参数，查询字符串
    city = request.args.get("city")
    print(request.data)
    return "Hello name={}, age={}, city={}, name_list={}".format(name, age, city, name_lis)
 

if __name__ == "__main__":
    app.run(debug=True)

```



### 保存文件

```python
# -*- coding: utf-8 -*-
from flask import Flask, request


app = Flask(__name__)


@app.route("/upload", methods=["POST"])
def upload():
    """接送前端发送过来的文件"""
    file_obj = request.files.get("pic")
    if file_obj is None:
        return "未上传文件"

    # # 将文件保存到本地
    # # 1. 创建一个文件夹
    # f = open("./demo.png", 'wb')
    # # 2. 想文件写内容
    # data = file_obj.read()
    # f.write(data)
    # # 3. 关闭文件
    # f.close()

    # 直接使用上传的文件对象保存
    file_obj.save("./demo1.png")

    return "上传成功"


if __name__ == "__main__":
    app.run(debug=True)

```

