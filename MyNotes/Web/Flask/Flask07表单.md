## 表单

使用 **Flask-WTF** 表单扩展，可以帮助进行CSRF验证，帮助我们快速定义表单模板，而且可以帮助我们在视图中验证表的数据。

**pip install Flask-WTF**安装

### Web表单

Web表单是web应用程序的基本功能

它是HTML页面中负责数据采集的部件。表单中有三个部分组成：表单标签、表单域、表单按钮。表单允许用户输入数据，负责HTML页面数据采集，通过表单将用户输入的数据提交给服务器。

在Flask中，为了处理表单，我们一般使用Flask-WTF扩展，它封装了WTForms，并且它有验证表单数据的功能。

#### WTForms支持的HTML标准字段

| 字段对象            | 说明                                |
| :------------------ | :---------------------------------- |
| StringField         | 文本字段                            |
| TextAreaField       | 多行文本字段                        |
| PasswordField       | 密码文本字段                        |
| HiddenField         | 隐藏文本字段                        |
| DateField           | 文本字段，值为datetime.data格式     |
| DateTimeField       | 文本字段，值为datetime.datetime格式 |
| IntegerField        | 文本字段，值为整数                  |
| DecimalField        | 文本字段，值为decimal.Decimal       |
| FloatField          | 文本字段，值为浮点数                |
| BooleanField        | 复选框，值为True和False             |
| RadioField          | 一组单选框                          |
| SelectField         | 下拉列表                            |
| SelectMultipleField | 下拉列表，可选多个值                |
| FileField           | 文本上传字段                        |
| SubmitField         | 表单提交按钮                        |
| FormField           | 把表单作为字段嵌入另一个表单        |
| FieldList           | 一组指定类型的字段                  |





#### WTForms常用验证函数

| 验证函数     | 说明                                     |
| :----------- | :--------------------------------------- |
| DataRequired | 确保字段中有数据                         |
| EqualTo      | 比较两个字段的值，常用于比较两次密码输入 |
| Length       | 验证输入的字符串长度                     |
| NumberRange  | 验证输入的值在数字范围内                 |
| URL          | 验证URL                                  |
| AnyOf        | 验证输入值在可选列表中                   |
| NoneOf       | 验证输入值不在可选列表中                 |







### 模板宏

1. 内部使用

```html

```

2. 外部使用

   首先在外面建一个html文件，然后将定义宏写在里面。然后在主html文件引用

   ```html
   {# filename:macro_input.html #}
   
   
   {% macro input4(type="text", value="", size=30 %}
       <input type="{{ type }}" vlaue="{{ value }}" size="{{ size }}">
   {% endmacro %}
   ```

   ```html
   {%  import "macro_input.html" as m_input %}
   <h1> input 4</h1>
   {{ m_input.input4() }}
   ```

   

