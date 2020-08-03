在Flask中，有一些特殊的变量和方法是可以在模板文件中直接访问的。

**config对象**：

config 对象就是Flask的config对象，也就是app.config 对象。

```html
{{config.SQLALCHEMY_DATABASE_URI }}
```



**request对象**：

就是Flask中表示当前请求的request对象，request对象中保存了一次HTTP请求的一切信息。

request常用的属性如下：

| 属性    | 说明                           | 类型           |
| ------- | ------------------------------ | -------------- |
| data    | 记录请求的数据，并转换为字符串 | *              |
| form    | 记录请求中的表单数据           | MultiDict      |
| args    | 记录请求中的查询参数           | MultiDict      |
| cookies | 记录请求中的cookie信息         | Dict           |
| headers | 记录请求中的报文头             | EnvironHeaders |
| method  | 记录请求使用的HTTP方法         | GET/POST       |
| url     | 记录请求的URL地址              | string         |
| files   | 记录请求上传的文件             | *              |

```html
{{ request.url }}
```

**url_for方法**：

url_for()会返回传入的路由函数对应的URL，所谓路由函数就是被app.route()路由装饰器装饰的函数。如果我们定义的路由函数是带有参数的，则可以将这些参数作为命名参数传入。

```html
{{ url_for('index') }}
{{ url_for('post', post_id=1024) }}
```

**get_flashed_messages方法**：

返回之前在Flask中通过flash()传入的信息列表。把字符串对象表示的消息加入到一个消息队列中，然后通过调用`get_flashed_messages()`方法取出。

```html
{% for message in get_flashed_messages() %}
	{{ message }}
{% endfor %}
```

 flash实际上是把信息放到了session中，所以我们在使用的时候要设置session

