## FLASK 发送邮件

#### 安装扩展 Flask-Mail



1. 导入flask-mail库

   ```python
   from flask_mail import Mail, Message
   ```

2. 配置邮件

   ```python
   # 配置邮件：服务器、端口、传输层安全协议、邮箱名、密码
   app.config.update(
   	DEBUG = True,
       MAIL_SERVER = "smtp.qq.com",
       MAIL_PORT = 465,
       MAIL_USE_TLS = True,
       MAIL_USERNAME = "123@qq.com",
       MAIL_PASSWORD = "ALKDJLAJLGJ"
   	)
   
   mail = Mail(app)
   # 发送邮件，sender 发送方，recipients 接受方列表
   msg = Message("This is a 标题", sender="123@qq.com", recipients=['41342@qq.com', '2342@qq.com'])
   
   # 邮件内容
   msg.body = "Flask test mail"
   
   # 发送邮件
   mail.send(msg)
   ```

   

