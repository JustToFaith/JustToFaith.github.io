![](/home/liz/typora照片/1.png)

![2](/home/liz/typora照片/2.png)

![3](/home/liz/typora照片/3.png)

![4](/home/liz/typora照片/4.png)



![](/home/liz/typora照片/21.png)

![5](/home/liz/typora照片/5.png)

![6](/home/liz/typora照片/6.png)

![](/home/liz/typora照片/7.png)

###### 从字符串的第一个开始匹配，若匹配的不是第一个，则无法匹配成功。





![10](/home/liz/typora照片/10.png)

![11](/home/liz/typora照片/11.png)

![12](/home/liz/typora照片/12.png)



![8](/home/liz/typora照片/8.png)

![](/home/liz/typora照片/9.png)





### 

### RE的另一种用法

![](/home/liz/typora照片/13.png)

![](/home/liz/typora照片/14.png)

![](/home/liz/typora照片/15.png)







### RE库的match对象

![](/home/liz/typora照片/16.png)

![](/home/liz/typora照片/17.png)





### 匹配

![](/home/liz/typora照片/18.png)

###### 注意：Re库中默认采用贪婪匹配，即输出匹配最长的子串

![](/home/liz/typora照片/19.png)

![](/home/liz/typora照片/20.png)

###### 一般采用加"?"来获得最小匹配







​    hrefpat='\\s+'

​    items=re.sub(hrefpat,'-',item)    //替换所有空格