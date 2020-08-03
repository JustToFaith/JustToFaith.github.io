# Java中的逻辑运算符

1. 与（并且）`&&`，全部都是`true`才是`true`，否者就是`false`。
2. 或（或者）`||`，至少有一个是`true`，才是`true`，否则是`false`。
3. 非（取反）`!`，将原本的`true`换成`false`；原本的`false`换成`true`。

与`&&`，或`||`，具有短路效果：如果根据左边已经可以判断出结论，那么右边的代码将不执行，从而节省一定的性能。

```java
public class Demo {
  public static void main(String[] args) {
    
    int a = 10;
    System.out.println(3 > 4 && ++a < 100);  //false
    System.out.println(a);  // 10,后面的++a没有执行,3>4已经可以判断false
      
    int b = 20;
    System.out.println(3 < 4 || ++b < 100);  //true
    System.out.println(b);  //20, 3 < 4已经可以判断为true，后面的不执行
  }
}
```



#### 注意事项

1. 逻辑运算符只能用于boolean值
2. 与、或需要左右各有一个boolean值，取反只需要一个唯一的boolean值即可
3. 与、或两种运算符可以连接多个条件
   - 两个条件：条件A && 条件B
   - 多个条件：条件A && 条件B && 条件C

tips：1 < x< 3 写成 1< x && x < 3

