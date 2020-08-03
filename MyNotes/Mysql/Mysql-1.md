1. sudo service mysql start  开始运行数据库
2. sudo service mysql stop  停止服务
3. sudo service mysql restart  重启服务
4. mysql -uroot -p  连接数据库
5. quit、exit、ctrl + D  退出数据库 
6. show databases;  查看所有的数据库
7. select now();  显示时间
8. select version():  显示数据库的版本
9. create database 数据库名 charset =utf8；  创建数据库，如果不写charset则默认latin1编码 
10. show create 数据库名;  查看刚才创建的数据库的信息
11. drop database 数据库名；  删除数据库
12. 如果数据库名字中有特殊符号，则加\`数据库\`表示一个整体 “\`”是tab上面那个键
13. use 数据库名；  使用数据库
14. select database()  查看当前使用的数据库  
15. show tables;  查看数据库中所有的表
16.  select * from table_name;  查询某一张表