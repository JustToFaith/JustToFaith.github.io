#### 创建虚拟环境

- virtualenv venv			创建一个虚拟的环境，名字叫venv
- cd venv           			   进入虚拟环境
- Scripts\activate             激活虚拟环境，相当于进入Scripts然后activate
- Scripts\activate            退出虚拟环境



#### Flask 创建 app 对象

- 初始化参数
  - 初始化参数
    -  import_name: 导入路径（寻找静态目录与模板目录位置的参数）
    - static_url_path: 访问静态资源的url前缀，默认值是static
    - static_folder:  静态文件的目录，默认‘static’
    - template_folder:  模板文件的目录，默认‘templates’



#### Flask 配置参数设置

- 使用配置文件

  - 先创建一个文件，叫config.cfg 。里面写 ：

    ```
    DEBUG = True
    ```

  - 然后再程序中写：

    ```python
    app.config.from_pyfile("config.cfg")
    ```

- 使用对象配置参数

  ```python
  class Config(object):
      DEBUG = True
      
  app.config.from_object(Config)
  ```

- 直接操作config

  ```python
  app.config["DEBUG'] = True
  ```

我们一般使用类的方式配置参数，如果参数少就直接操作config



#### 读取配置参数

- 直接从全局对象app的config字典中取值, app.config.get / app.config["xxx"]

  ```
  class Config(object):
      DEBUG = True
      ITCAST = "python"
  app.config.from_object(Config)
  
  
  @app.route('/')
  def hello_world():
      text = app.config.get("ITCAST")
      return text
  ```

  

- 通过current_app获取参数, current_app.get()

  ```
  from flask import Flask, current_app
  
  
  class Config(object):
      DEBUG = True
      ITCAST = "python"
  app.config.from_object(Config)
  
  
  @app.route('/')
  def hello_world():
      text = current_app.get("ITCAST")
      return text
  ```

  

#### app.run()的使用说明

```python
app.run(host='0.0.0.0', port=5000, debug=true)
```

**host**表示绑定ip，**port**表示绑定端口，使用**debug**调试功能