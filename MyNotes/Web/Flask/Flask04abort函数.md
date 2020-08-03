## abort 函数

这个函数用于抛出异常

```python
from flask import Flask, abort, Response


app = Flask(__name__)


@app.route("/login",['GET'])
def login():
    name = ''
    pwd = ''
    if name != "zhangsan" or pwd != "abc":
        # 可以使用abort函数立即终止视图函数的执行
        # 并返回给前端特定的信息
        # 1. 传递状态码信息，必须是标准的http状态码
        abort(404)
        # 2. 传递响应体信息
        resp = Response("login failed")
        abort(resp)
        
    return "login success"
        
if __name__ == "__main__":
    app.run(debug=True)

```

**abort**可以终止视图函数的执行，并返回异常状态码或者响应体信息



## 设置响应信息的方法

```python
# -*- coding: utf-8 -*-
from flask import Flask, make_response


app = Flask(__name__)


@app.route("/index")
def index():
    # 1. 使用元组，返回自定义的响应信息
    #           响应体    状态码  响应头
    # return "index page", 400, [("Itcast", "python"), ("city", "shenzhen")]
    # return "index page", 400, {"itcast": "pythob", "city": "shenzhen"}
    # 一般已知的状态码都有一个状态信息，如果自定义一个状态码，可以自定义一个状态信息，字符串状态码后面跟一个空格加状态信息
    # return "index page", "666 good", {"itcast": "pythob", "city": "shenzhen"}
    # return "index page", "666 good"

    # 2. 使用make_response 来构造响应信息
    resp = make_response("index page 2")  # 括号里的是返回的页面信息
    resp.status = "999 itcast"  # 设置状态吗
    resp.headers["city"] = "sz"  # 设置响应头
    return resp


if __name__ == "__main__":
    app.run(debug=True)

```





## 返回json数据的方法

```python
# -*- coding: utf-8 -*_
from flask import Flask, jsonify
import json


app = Flask(__name__)


@app.route("/index")
def index():
    # json 就是字符串
    data = {
        "name": "python",
        "age": 18
    }
    # json.dumps(字典)   将python字典转换为json字符串
    # json.loads(字符串)     将json字符串转换为python字典
    # json_str = json.dumps(data)

    # return json_str, 400

    # jsonify 帮助转为json数据，并设置响应头 Content-Type 为application/json
    return jsonify(data)
    # return jsonify({"name": "python", "age": 18})


if __name__ == "__main__":
    app.run(debug=True)

```

1. 直接使用原始json方法，将dict转化为json字符串再返回
2. 用flask中的jsonify，它会自动将dict转化为json字符串并返回





