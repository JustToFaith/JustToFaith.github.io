# Mybatis（二）

1、回顾mybatis自定义和环境搭建+完善自定义Mybatis的注解开发
2、Mybatis基于代理Dao的CRUD操作					重点内容
3、CRUD中可能遇到的问题：参数的传递以及返回值的封装
4、介绍Mybatis基于传统dao方式的使用（自己编写dao的实现类）	了解的内容
5、mybatis主配置文件中的常用配置
	properties标签
	typeAliases标签				---解释Integer的写法
	mappers标签的子标签：package

-----------------------------------------
OGNL表达式：
	Object Graphic Navigation Language
	对象	图	导航	   语言
	

它是通过对象的取值方法来获取数据。在写法上把get给省略了。
比如：我们获取用户的名称
	类中的写法：user.getUsername();
	OGNL表达式写法：user.username
mybatis中为什么能直接写username,而不用user.呢：
	因为在parameterType中已经提供了属性所属的类，所以此时不需要写对象名



#### 分析编写DAO实现类Mybatis的执行过程

![非常重要的一张图-分析编写dao实现类Mybatis的执行过程](https://tva1.sinaimg.cn/large/007S8ZIlly1gdzfexokwej32sc0u04hb.jpg)



#### 分析代理DAO的执行过程

![非常重要的一张图-分析代理dao的执行过程](https://tva1.sinaimg.cn/large/007S8ZIlly1gdzffursctj30u02lmwlj.jpg)

#### 的过程

![非常重要的一张图](https://tva1.sinaimg.cn/large/007S8ZIlly1gdzfgwteuoj31wy0u017o.jpg)

