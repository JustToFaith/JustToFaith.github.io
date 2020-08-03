## 蓝图

当我们把程序模块化划分时，每一个视图或者功能作为一个模块。在不同的模块我们需要导入app和视图函数。但是在执行app.py的过程中，由于不同模块之间相互导入，造成了循环导入的错误。所以为了解决这个方法，换一个方式使用装饰器，定义一个后补充来执行相同的功能。

例如：

```python
main.py
from Flask import flask
from goods import get_goods
from user import register

app = Flask(__name__)

# 用这样的方式直接使用路由函数，第一个括号里加路径，第二个括号里加视图函数名
app.route("/get_goods")(get_goods)
app.route("/register")(register)

@app.route("/")
def index():
    return "index page"

if __name__ == "__main__":
    app.run()
    
    
goods.py

def get_goods():
    return "get goods page"



```

上面为不使用蓝图进行模块划分，下面使用蓝图来进行模块划分。



### 蓝图

1. 创建蓝图对象

   ```python
   from flask import Blueprint
   # Blueprint创建一个蓝图对象，必须制定两个参数，admin表示蓝图的名称，__name__表示蓝图所在的模块
   admin = Blueprint('admin', __name__)
   ```

2. 注册蓝图路由

   ```python
   @admin.route('/')
   def admin_index():
       return 'admin_index'
   ```

3. 在程序实例中注册该蓝图

   ```
   app.register_blueprint(admin, url_prefix='/admin')
   ```

   

例子：

```python
orders.py
# -*- coding:utf-8 -*-
from Flask import Blueprint

# 创建一个蓝图对象，蓝图就是一个小模块的抽象的概念, template_folder是模本目录
app_orders = Blueprint("app_orders", __name__, template_folder="templates")

@app_orders.route("/get_orders")
def get_orders():
return "get order page"


main.py
# -*- coding:utf-8 -*-
from flask import Flask
from orders import app_orders

# 注册蓝图     url_prefix是加前缀，最后在浏览器中的路径是前缀加上一个自己的路径
app.register_blueprint(app_orders, url_prefix("/orders"))

if __name__ == "__main__":
	app.run()

```