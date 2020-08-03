- 使用**app.url_map**查找整个flask中路由的信息

  ```python
  if __name__ == '__main__':
      # 通过url_map可以查找整个flask中的路由信息
      print(app.url_map)
      app.run(debug=True)
  ```

- 路由**@app.route()**的使用

  ```python
  @app.route("/", methods=["GET", "POST"])
  ```

- 一个路径对应两个视图函数  - **可以**

  1. 如果两个视图函数的路径相同，并且请求方式相同。那么如果访问那一个路径是只能访问排在上面的第一个视图函数。
  2. 如果两个视图函数路径相同，但是请求方式不同。通过一个路径，不同的请求方式可以访问不同的视图函数。

- 一个视图函数对应两个路径  - **可以**

- 跳转页面**redirect**

  ```python
  from flask import redirect
  
  
  @app.route("/login")
  def login():
  	url = "/"
  	return redirect(url)
  ```

  将当前页面挑战到其他页面

- 使用 **url_for**进行反解析

  ```python
  from flask import url_for, redirect
  
  
  @app.route("/login")
  def login():
      # 使用url_for的函数，通过视图函数的名字找到视图函数对应的url路径 
      url = url_for("index")
      return redirect(url)
  ```

  index是一个视图函数，因为它绑定了一个路径，所以在使用url_for的时候，它会自动寻找到其绑定的路径，这样如果需要改index函数的时候，就不用再重复修改login函数，提高了效率

- 路由提取参数与自定义路由转换器

  | int       | 接受整数                       |
  | --------- | ------------------------------ |
  | **float** | **同int， 但是接受浮点数**     |
  | **path**  | **和默认的相似，但也接受斜线** |

  ```python
  # 转换器
  # 127.0.1.1：5000/goods/123
  @app.route("/goods/<int:goods_id>")
  def goods_detail(goods_id):
      '''定义的视图函数 '''
      return "goods detail page"
  ```

  转换器就是在对应的路径后面提取参数，上示例是提取一个整形变量，并命名为goods_id,并将其参数传到下面函数中，函数也要定义一个变量去接收提取的参数。我们就把<int>叫做转换器

  **如果不加类型，冒号之前的不加。就默认为字符串类型。(除了“/”以外的所有字符)**
  
  - 自定义万能转换器
  
    1. 定义自己的转换器，以类的方式
    2. 将自定义的转换器添加到flask应用中
    3. 在路由函数中设置
  
    ```python
    from werkzeug.routing import BaseConverter
    
    
    
    # 1. 定义自己的转换器
    class RegexConverter(BaseConverter):
    
        def __init__(self, url_map, regex):
            # 调用父类的初始化方法
            super(RegexConverter, self).__init__(url_map)
            # 将正则表达式的参数保存到对象的属性中，flask会去使用这个属性来进行路由的正则匹配
            self.regex = regex
    
    
    # 2. 将自定义的转换器添加到flask的应用中
    app.url_map.converters["re"] = RegexConverter
    
    
    @app.route("/send/<re(r'1[3456789]\d{9}'):mobile>")  # : 后的是自定义的，但必须和下面传参相同 
    def send_sms(mobile):
        return "send sms to %s " % mobile
    
    
    ```
  
  - 普通理解转换器
  
    ```python
    from werkzeug.routing import BaseConverter
    
    
    # 1.自定义转换器
    class MobileConverter(BaseConverter):
        def __init__(self, url_map):
            super(MobileConverter, self).__init__(url_map)
            self.regex = r'1[3456789]\d{9}'
            
    app.url_map.converters['re'] = RegexConverter
    app.url_map.converters['mobile'] = MobileConverter
    
    
    @ap.route("/send/<mobile:mobile_num>")
    def send_sms(mobile_num):
        return "send sms to %s" % mobile_num
    
    ```
  
    *首先定义一个class对象，然后在class中初始化。因为类的构造不是我们自己去执行的，是flask去执行的。所以在构造的时候第一个参数是固定的**url_map**，但是接受过来之后我们不用，父类会用。我们就需要调用父类的方法。调用当前的类，以self对象进行寻找，找到父类之后，调用父类的初始化方法，并把url_map传给父类，让他进行构造。**self.regex**是不变的，后面是正则表达式。最后我们需要将正则表达式添加到app.url_map.converters中，并给字典命名。最后在视图函数的路由器中，将其传入进去*
  
  - 转换器的进阶使用
  
    ```python
    from werkzeug.routing import BaseConverter
    
    
    def RegexConverter(BaseConverter):
        def __init__(self, url_map, regex):
            # 调用父类的初始化方法
            super(RegexConverter, self).__init__(url_map)
            # 使用正则表达式的参数保存到对象的属性中，flask回去使用这个属性来进行路由的正则匹配
            self.regex = regex
            
        def to_python(self, value):  # value是在路径进行正则表达式中提取的参数
            return "abc"
        
        def to_url(self, value):
            # 使用url_for的方法的时候被调用
            return "5678"
        
    app.url_map.converters['mobile'] = MobileConverter
    
    
    @ap.route("/send/<mobile:mobile_num>")
    def send_sms(mobile_num):
        return "send sms to %s" % mobile_num
    
    
    @app.route("/index")
    def index():
        url = url_for("send_sms", mobile_num='1234')  # mobile_num必须和上面需要跳转函数的正则表达式后的参数一致
        return redirect(url)
    ```
  
    *调用**to_python** 这个函数可以修改提起的返回值，会导致从提取出来的参数，会传到这个 to_python中，并把它的返回值，返回给视图函数作为参数。如上代码，不管提取的号码是多少，返回的始终都是abc*
  
    *当我们使用url_for进行跳转页面的时候，url_for后接的参数是那个页面url的函数名字，然后它会自动根据那个函数名找到之后的路径。但是由于需要跳转的那个函数后没有完整的路径，后面有一段是正则表达式，所有不能完成跳转。所以后面要加一个参数(mobile_num)，这个参数要与需要跳转函数定义的参数一致。这个参数会返回到跳转函数定义一个提取正则表达式的一个类中，并传到to_url中，在经过to_url处理后返回一个值，并由此值作为路径的一部分进行跳转。*
  
    
  











