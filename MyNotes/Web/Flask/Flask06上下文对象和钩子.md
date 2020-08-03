  ## 请求钩子

请求钩子是通过装饰器的形式实现，Flask支持如下四种请求钩子：

before_first_request ：在处理第一个请求前运行

@app.before_first_request ：

before_request ：在每次请求前运行

after_request(response) ：如果没有未处理的异常抛出，在每次请求后运行（有异常不执行）

teardown_request(response) ：在每次请求后运行，即使有未处理的异常抛出（有异常也执行） 

```python
# -*- coding: utf-8 -*-
from flask import Flask

app = Flask(__name__)


@app.route('/index')
def index():
    return 'Hello World!'


@app.before_first_request
def handle_before_first_request():
    """在第一次请求处理之前被执行"""
    print("handle_before_first_request 被执行")


@app.before_request
def handle_before_request():
    """在每次请求之前都被执行"""
    print("handle_before_request 被执行")


@app.after_request
def handle_after_request(response):
    """在每次请求（视图函数处理）之后都被执行，前提是视图函数没有出现异常"""
    print("handle_after_request 被执行")
    return response


@app.teardown_request
def handle_teardown_request(response):
    """在每次请求（视图函数处理）之后都被执行，无论视图函数是否出现异常，都被执行"""
    print("handle_teardown_request 被执行")
    return response
# 后面两个参数必须接受参数，必须有个返回值
# 在正常模式下，teardown_request会执行，在debug模式下不执行。工作在生成模式下


if __name__ == '__main__':
    app.run()

```





## 请求上下文







## 应用上下文







## flask_scrip脚本扩展的应用

```python
# -*- coding: utf-8 -*-
from flask import Flask
from flask_scrip import Manager  # 启动命令的管理类


app = Flask(__name__)

# 创建Manager管理类的对象
manager = Manager(app)


@app.route("/index")
def index():
    return "hello word"


if __name__ == "__main__":
    # app.run(debug=True)
    # 通过管理对象来启动flask
    manager.run()

```

通过在终端执行**python xxx.py runserver**

-p 绑定ip

-h 绑定端口

-d 开启debug