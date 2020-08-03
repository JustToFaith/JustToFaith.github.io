## 数据库的设置

1. 数据库的安装

   - 安装服务器

   ```
   sudo apt-get install mysql-server
   ```

   - 安装客户端

   ```
   sudo apt-get install mysql-client
   ```

2. 在Flask中使用mysql数据库，需要安装一个flask-sqlalchemy的扩展

   ```
   pip install flask-sqlalchemy
   ```

   需要连接mysql数据库，仍需要安装flask-mysqldb

   ```
   pip install flask-mysqldb
   ```

3. Flask的数据配置

   - 设置连接数据库

     ```
     app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/test3'
     ```

     root是账号，mysql是密码， 127.0.0.1是ip，3306是端口，test3是数据库名字

   - 设置每次请求结束后会自动提交数据库的改动

     ```
     app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True
     ```

     我们一般情况下不设置次参数，因为自动提交会导致我们看不到提交后的提示信息，无法判断是否有异常。

   -   跟踪修改

     ```
     app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
     ```

      当我们对模型进行修改时，为了保证数据库中的数据和模型相同，我们通常引用这个参数，它会自动同步模型和数据库中的信息。这个参数我们通常是开启的。

   - 显示原始的SQL语句

     ```
     app.config['SQLALCHEMY_ECHO'] =True
     db = SQLALchemy(app)
     ```



### 常用的SQLALchemy字段类型

| 类型名       | python中的类型    | 说明                                                |
| ------------ | ----------------- | --------------------------------------------------- |
| Integer      | int               | 普通整数，一般是32位                                |
| SmallInteger | int               | 数值范围小的整数，一般是16位                        |
| BigInteger   | int或long         | 不限制精度的整数                                    |
| Float        | float             | 浮点数                                              |
| Numeric      | decimal.Decimal   | 普通整数，一般是32位                                |
| String       | str               | 变长字符串                                          |
| Text         | str               | 变长字符串，对较长或不限长度的字符串做了优化        |
| Unicode      | unicode           | 变长Unicode字符串                                   |
| UnicodeText  | unicode           | 变长Unicode字符串，对较长或不限长度的字符串做了优化 |
| Boolean      | bool              | 布尔值                                              |
| Date         | datetime.date     | 时间                                                |
| Time         | datetime.datetime | 日期和时间                                          |
| LargeBinary  | str               | 二进制文件                                          |



### 常用的SQLALchemy列选项

| 列项名      | 说明                                              |
| ----------- | ------------------------------------------------- |
| primary_key | 如果为True，代表表的主键                          |
| unique      | 如果为True，代表这列不允许出现重复的值            |
| index       | 如果为True，为这列创建索引，提高查询效率          |
| nullable    | 如果为True，允许有空值，如果为False，不允许有空值 |
| default     | 为这列定义默认值                                  |



### 常用SQLALchemy关系选项

| 选项名       | 说明                               |
| ------------ | ---------------------------------- |
| backref      | 在关系的另一模型中添加反向引用     |
| primary join | 明确指定两个模型之间使用的联结条件 |
|              |                                    |

 

### 数据库操作

1. 查询

   - Role.query.all()		在Role这张表中查询所有的数据 

   - Role.query.first()		查询Role这张表中的第一个值

   - Role.query.get(2)		这个只能查询主键对应的值

   - db.session.query(Role).all()		查询Role表中的值

   - User.query.filter_by(name="wang").all()		查询User表中所有name为'wang'的信息,严格匹配

   - from sqlalchemy import or_         首先从sqlalchemy中导入ro_

     User.query.filter(or_(User.name=="wang", User.email.endswith("163.com"))).all()        使用or\_查询名字为wang或者邮箱以163.com结尾的数据
     
   - User.query.offset(2).all()        偏移两个，跳过两条，从第三条开始取
   
   - User.query.offset(2).limit(2).all()        跳过两条，限制取两条
   
   - User.query.order_by(User.id.desc())       通过User的id进行降序查询
   
   - User.query.order_by(User.id.asc())          通过User的id进行升序查询，默认升序，可以不用写
   
   - db.session.query(User.role_id).group_by(User.role_id)        通过查询role_id对role_id进行分组



##### 常用的SQLAlchemy查询过滤器

| 过滤器      | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| filter()    | 把过滤器添加到原查询上，返回一个新的查询                     |
| filter_by() | 把等值过滤器添加到原查询上，返回一个新查询                   |
| limit()     | 使用指定的值限制原查询返回的结果数量，返回一个新查询（限制取几条） |
| offset()    | 偏移原查询返回结果，返回一个新查询（偏移跳过几条）           |
| order_by()  | 根据指定条件对原查询结果进行排序，返回一个新查询（User.id.desc()/.asc 倒序/升序。默认升序） |
| group_by()  | 根据指定条件对原查询结果进行分组，返回一个新查询             |



#### 最常用的SQLAlchemy查询执行方法

| 方法           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| all()          | 以列表形式返回查询的所有结果                                 |
| first()        | 返回查询的第一个结果，如果没有结果，则返回None               |
| first_or_404() | 返回查询的第一个结果，如果没有结果，则终止请求，返回404错误响应 |
| get()          | 返回指定主键对应的行，如果没有对应的行，则返回None           |
| get_or_404()   | 返回指定主键对应的行，如果没有找到指定的主键，则终止请求，返回404错误响应 |
| count()        | 返回查询结果的数量                                           |
| paginate()     | 返回一个Paginate()对象，包含指定范围内的结果                 |



1. 修改

   - admint_role.name = 'Administrator'		

     db.session.add(admit_role)

     db.session.commit()		将admit的name改为Administrator, admit_role为admit在创建时赋值的用于提交的那个。

   - User.query.filter_by(name="zhou").update({"name":"python","email":"123@qq.com"})        通过过滤找到我们需要修改的那一项，然后通过updata将需要修改的通过字典的形式将其修改

     

     
     
     

2. 删除行

   db.session.delete(mod_role)

   db.session.commit()		删除Moderator,mod_role为Moderator在创建时用于提交的那个变量



## 数据库的迁移

1. 首先导入两个库

   ```python
   from flask_migrate import Migrate, MigrateCommand
   from flask_script import Shell, Managet
   ```

   

2. 在app初始化和配置完成后创建flask脚本管理工具对象和数据库迁移工具对象

   ```python
   # 创建flask脚本管理工具对象
   manager = Manager(app)
   # 创建数据库迁移工具对象
   migrate = Migrate(app, db)
   # 向manager对象中添加数据库的操作命令
   manager.add_command("db", MigrateCommand)
   ```

3. 最后在启动时不用app.run() ，而是使用manager.run()

   ```python
   # 通过manager对象启动程序
   manager.run()
   ```



#### 启动命令

- python app.py db init		# 添加数据库迁移支持，并创建migrations目录，所有迁移脚本都存放在那里。
- python app.py db migrate -m "migrate detail "        # 自动创建迁移脚本
- python app.py db upgrade        # 把迁移应用到数据库中
- python app.py db history        # 查看历史迁移记录
- python app.py db downgrade status_code        # 降级操作，downgrade后面接状态码，在历史记录中可以查看 