## cookie的使用

**make_response**



**set_cookie(key, value='', max_age=None)**

设置cookie，max_age是设置过期时间，默认无max_age是过期时间是关闭浏览器时过期，单位：秒

**delete_cookie(key)**

删除cookie

**request.cookies.get**

获取cookie，需要导入flask中的request



**设置cookie的方式有两种，一种是用set_cookie,还有一种是直接在headers中设置**



```python
# -*- coding: utf-8 -*-
from flask import Flask, make_response, request


app = Flask(__name__)


@app.route('/set_cookie')
def set_cookie():
    resp = make_response("success")
    # 设置cookie， 默认有效期是临时cookie，浏览器关闭就失效
    resp.set_cookie("itcast","python")
    resp.set_cookie("itcast2", "python2")
    # 通过max_age设置有效期，单位秒
    resp.set_cookie("itcast3", "python3", max_age=3600)
    # 通过设置请求头的方式设置cookie
    resp.headers["Set-Cookie"] = "itcast4=python4; Expires=Wed, 12-Jun-2019 15:09:20 GMT; Max-Age=3600; Path=/"

    return resp


@app.route("/get_cookie")
def get_cookie():
    c = request.cookies.get("itcast2")
    return c


@app.route("/delete_cookie")
def delete_cookie():
    resp = make_response("delete success")
    # 删除cookie
    resp.delete_cookie("itcast")
    return resp


if __name__ == '__main__':
    app.run()

```





## session

from flask import session



需要设置secret_key

```python
# -*- coding: utf-8 -*-
from flask import Flask, session


app = Flask(__name__)
# flask的session需要用到的密钥字符串
app.config["SECRET_KEY"] = "abc123de"

# 在flask中，默认把session是保存在cookie中，
# 但是在cookie中我们看到的cookie字符串并不是我们所设置的值，而是通过了secret_key把session混淆后得出的。
# 这样就能防止别人解密的到，session。如果cookie中的session被改动，则服务器不会认可改session


@app.route("/login")
def login():
    # 设置session数据
    session["name"] = "python"
    session["moblie"] = "1230420384098"
    return "login success"


@app.route("/index")
def index():
    # 获取session数据
    name = session.get("name")
    return "hello {}".format(name)


if __name__ == "__main__":
    app.run(debug=True)
    
    
# 可以在没有cookie的时候使用session。因为正常情况下session_id是保存在cookie中的，如果没有cookie则可以把session_id设置在路径中

```

1. 在flask中，默认把session是保存在cookie中，但是在cookie中我们看到的cookie字符串并不是我们所设置的值，而是通过了secret_key把session混淆后得出的。这样就能防止别人解密的到，session。如果cookie中的session被改动，则服务器不会认可改session。
2. 可以在没有cookie的时候使用session。因为正常情况下session_id是保存在cookie中的，如果没有cookie则可以把session_id设置在路径中。但是这样session不会长时间保存，只能在页面间跳转的时候不用重复登陆，但是当浏览器关闭之后，session就失效不能使用。