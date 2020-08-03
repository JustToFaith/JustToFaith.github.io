# java四种权限修饰符

> public > protected > (default) > private , default是不写修饰符



|        作用域        | public | protected | (default) | private |
| :------------------: | :----: | :-------: | :-------: | :-----: |
|   同一个类(我自己)   |  yes   |    yes    |    yes    |   yes   |
|   同一个包(我邻居)   |  yes   |    yes    |    yes    |   no    |
|  不同包子类(我儿子)  |  yes   |    yes    |    no     |   no    |
| 不同包非子类(陌生人) |  yes   |    no     |    no     |   no    |



### 类的权限修饰符

1. 外部类：public / (default)
2. 成员内部类：public / protected / (default) / private
3. 局部内部类：什么都不写