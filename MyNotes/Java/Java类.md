# Java类



注意事项:

1. 成员变量是直接定义在类当中，在方法外边
2. 成员方法不要写`static`关键字



创建对象的方法：

1. 导包

   指出需要使用的类在什么位置

   ```java
   import 包名称 类名称;
   import cn.itcast.day06.demo01.Student;
   ```

   对于和当前类属于同一个包的情况，可以省略导包语句不写。

2. 创建

   ```java
   类名称 对象名 = new 类名称();
   Student stu = new Student();
   ```

   

3. 使用

   - 使用成员变量：**对象名.成员变量名**
   - 使用成员方法：**对象名.成员方法名(参数)** 



### 构造方法

构造方法是专门用来创建对象的方法，当我们通过关键字`new`创建对象时，其实就是在调用构造方法。

```java
public 类名称(参数类型 参数名称){
  方法体;
}
```



注意事项：

1. 构造方法的名称必须和所在类的名称完全一样，大小写也相同
2. 构造方法不要写返回值类型，连`void`都不写
3. 构造方法不能`return`一个具体的返回值
4. 如果没有编写任何构造方法，那么编译器将会**默认构建**一个构造方法，没有参数、方法体什么是都不用做
5. 一旦写了构造方法，编译器将不再默认构造
6. 构造方法可以进行重载(方法名称相同，参数不同)



### 标准类

一个标准类通常要有下面四个组成部分：

1. 所有的成员变量都要使用`private`关键字修饰
2. 为每个成员变量编写一对`Setter/Getter`方法
3. 编写一个无参构造
4. 编写一个有参构造

这样的标准类也叫做`Java Bean`

---

### 匿名对象

匿名对象就是只有右边的对象，没有左边的名字和赋值运算。

格式：`new 类名称();`通过`.成员方法()`来调用成员函数。例如`new Person().showName();`

**匿名对象只能使用一次，再次使用只能重新创建。**

#### 匿名对象可以作为参数和返回值：

```java
public class Demo {
    public static void main(String[] args){

        //使用匿名对象作为参数
        methodParam(new Scanner(System.in));
        System.out.println("==========");

        //使用匿名对象作为返回值
        Scanner sc = methodReturn();
        int num = sc.nextInt();
        System.out.println("输入的数字是：" + num);

    }

    public static void methodParam(Scanner sc){
        int num = sc.nextInt();
        System.out.println("输入的数字是：" + num);
    }

    public static Scanner methodReturn(){
        return new Scanner(System.in);
    }

}
```

