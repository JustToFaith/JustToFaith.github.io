# java Objects类

> `java.lang.Object`类是java语言中的根类，即所有类的父类。它里面描述的所有方法，子类都可使用。在对象实例化的时候，最终找到的父类就是`Object`。

如果一个类没有特别指定父类，那么这个类默认继承自`Object`类。例如：

```java
public class Myclass /* extends Object */ {
  //...
}
```

### toString方法

`public String toString()`：返回该对象的字符串表示。

`toString`方法返回该对象的字符串表示，其实该字符串内容就是对象类型+@+内存地址值。



由于`toString`方法返回的结果是**内存地址值**，而在开发中，经常需要按照对象的属性得到相应的字符串表现形式，因此也需要重写它。

#### 覆盖重写

如果不希望使用toString方法的默认行为，则可以对它进行覆盖重写。例如自定义的Person类：

```java
public class Person {
  private String name;
  private int age;
  
  @Override
  public String toString(){
    return "Person{" + "name='" + name + '\'' + ", age=" + age + '}';
  }
  // 省略构造器与Getter Setter
}
```



### equals方法

`public boolean equals(Object obj)`：指示其他某个对象是否与此对象相等。

调用成员方法`equals`并指定参数为另一个对象，则可以判断这两个对象是否是相同的。这里的“**相同**”有默认和自定义两种形式。

#### 默认地址比较

如果没有覆盖重写`equals`方法，那个`Object`类中默认进行`==`运算符的对象地址比较，只要不是同一个对象，结果必然为`false`

#### 对象内容比较

如果希望进行对象的内容比较，即所有或指定的部分成员变量相同就判定为两个对象相同，则可以覆盖重写`equals`方法，例如：

```java
import java.util.Objects;

pubic class Person{
  private String name;
  private int age;
  
  @Override
  public boolean equals(Object o) {
    // 如果对象地址一样，则认为相同
    if (this == o)
      return true;
    // 如果参数为空，或者类型信息不一样，则认为不同
    if (o == null || getClass() != o.getClass())
      return false;
    // 转换为当前类型
    Person person = (Person) o;
    // 要求基本类型相等，并且将引用类型交给java.util.Objects类的equals静态方法取用结果
    return age == preosn.age && Objects.equals(name, person.name);
  }
}
```

这段代码充分考虑了对象为空、类型一致等问题，但方法内容并不唯一。大多数IDE都可以自动生成equals方法的代码内容。

