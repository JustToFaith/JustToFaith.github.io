# Java常用API

1. #### Scanner

   键盘输入数据，包路径`import java.util.Scanner`

   ```java
   import java.util.Scanner;
   public class Demo{
     public static void main(String[] args){
       Scanner sc = new Scanner(System.in);  //System.in是从键盘输入
       
       // 获取键盘数字
       int num = sc.nextInt();
       System.out.println(num);
   
       // 获取键盘字符串
       String str = sc.next();
       System.out.println(str);
     }
   }
   ```

   **创建**

   `类名称 对象名 = new 类名称();`

   **使用**

   `对象名.成员方法名();`

   获取键盘输入的`int`数字：`int num = sc.nextInt();`

   获取键盘输入的字符串：`String str= sc.next();`

   ---

   

2. #### Random

   用来生成随机数字，包路径`import java.util.Random`

   **创建**

   `Random r = new Random();`

   **使用**
   `int num = r.nextInt(10);`范围0~9	

   ---

   

3. #### ArrayList

   `array`的数组的长度不可以改变，但是`arraylist`可以改变。

   **创建**
   `ArrayList<String> list = new ArrayList<>(); `，==<>==填写什么类型(**填写的泛型只能是引用数据或者包装类)**就只能添加什么类型.

   ###### 包装类：

   | 基本类型 | 包装类(引用类型，包装类都位于java.lang包下) |
   | -------- | ------------------------------------------- |
   | byte     | Byte                                        |
   | short    | Short                                       |
   | int      | Integer                                     |
   | long     | Long                                        |
   | float    | Float                                       |
   | double   | Double                                      |
   | char     | Character                                   |
   | boolean  | Boolean                                     |

   

   **使用**

   `list.add("data")`，添加数据到数组中。

   `list.get(index)`，读取元素，`index`为索引

   `list.remove(index/name)`删除对应索引位置的元素，或者元素名称

   `list.size()`得到数组的长度

   ---

4. #### String

   **字符串的特点：**

   1. 字符串的内容永不可变
   2. 由于字符串不可变，所以字符串是可以共享使用
   3. 字符串效果上相当于是`char[]`字符数组，但是底层原理是`byte[]`字节数组

   **创建字符串的常见3+1中方式：**

   - 三种构造方法
     - `public String()`：创建一个空白字符串，不含有任何内容。
     - `public String(char[] array)`：根据**字符**数组的内容，来创建对应的字符串
     - `public String(byte[] array)`：根据**字节**数组的内容，来创建对应的字符串
   - 一种直接创建
     - `String str = "Hello"`

   **注意**

   字符串常量池：程序当中直接写上的双引号字符串，就在字符串常量池中。

   对于基本数据类型来说，`==`是进行数值的比较。

   对于引用数据类型来说，`==`是进行**地址值**的比较。

   **`String`当中与获取相关的常用方法**

   1. `public int length()`：获取字符串当前含有的字符个数，拿到字符串长度

      ```java
      int length = "abc".length();
      ```

   2. `public String concat(String str)`：将当前字符串和参数字符串进行皮拼接操作，返回新的字符串

      ```java
      String str3 = str1.concat(str2)
      ```

   3. `public char charAt(int index)`：获取指定索引位置的单个字符

      ```java
      char ch = "abcde".charAt(1)
      ```

   4. `public int indexOf(String str)`：查找参数字符串在本字符串中首次出现的位置，如果没有范围-1值。

      ```java
      int index = "abcd".indexOf("c")
      ```

   **字符串的截取**

   ```java
   String str = "HelloWord".substring(2, 5)
   ```

   **字符串的转换**

   1. `public char[] toCharArray()`：将当前字符串拆分成为新的字符数组作为返回值

      ```java
      char[] chars = "Hello".toCharArray();
      ```

      

   2. `public byte[] getBytes()`：获得当前字符串底层的字符数组

      ```java
      byte[] bytes = "abc".getBytes();
      ```

      

   3. `public String replace(CharSequence oldString, CharSequence newString)`：将所有出现的老字符串替换成新的字符串，返回替换后的结果新字符串。

      ```java
      String str1 = "How do you do?";
      String str2 = str1.replace("o", "*")
      ```

   **字符成的分割**

   `public String[] split(String regex)`：按照参数的规则，将字符串切分成为若干部分

   ```java
   String str = "abc,abc,abc";
   String[] str2 = str.split(",");
   ```

   *`split`是用正则表达式进行切合，当使用英文`.`或者其他在正则表达式中有特殊含义的字符时无法完成切割，需要进行转义。*

5. #### static

   1. 如果一个**成员变量**使用了`static`，那个这个成员变量不属于对象，属于类。

      ```java
      public class Student{
        private int id;
        private String name;
        private int age;
        static String room;
        private static int idCounter; //学号计数器，每当new了一个新对象的时候，计数器++
          public Student(){
            inCounter++;
          }
        public Student(String name, int age)
        {
          this.name = name;
          this.age = age;
          this.id = idCounter++;
        }
        
        public String getName(){
          return name;
        }
        
      }
      ```

   2. 一旦使用`static`修饰成员方法，那么这个方法就成为了**静态方法**，静态方法不属于对象，而是属于类。

      如果没有使用`static`关键字，那么必须先创建对象，然后调用方法。而使用了`static`关键字的静态方法，推荐使用**类名称调用静态方法**

      ```java
      静态变量：类名称.静态变量;
      静态方法：类名称.静态方法();
      ```

      注意：

      1. ==静态不能直接访问非静态==

         原因：因为在内存中是**先有静态内容，后有非静态内容**

         先人不知道后人，但是后人知道先人。

      2. 静态方法中不能使用`this`

         `this`表示当前对象，通过谁调用的方法，谁就是当前对象。

   3. 静态代码块

      当首次使用到本类时，静态代码块执行一次（唯一执行一次）。

      静态内容总是优先于非静态，所以静态代码块比构造方法先执行

      **静态代码块的用途**

      用来一次性地对静态成员变量进行赋值。

6. #### Arrays

   1. `public static String toString(数组)`：将参数数组变成字符串（按照默认格式[element1, element2,...]）

      ```java
      String intStr = Arrays.toString({10, 20, 30});
      // [10, 20, 30]
      ```

      

   2. `pubic static void sort(数组)`：按照默认升序（从小到大）对数组的元素进行排序。*没有返回值，直接将原数组进行排序*

      ```java
      int[] array1 = {2, 1, 3, 5, 6};
      Arrays.sort(array1);
      ```

      备注：

      1. 如果是数值，`sort`默认按照升序从小到大
      2. 如果是字符串，`sort`默认按照字母升序
      3. 如果是自定义的类型，那么这个自定义的类型需要有`Comparable`或者`Comparator`接口的支持。

7. #### Math类

   1. `public static double abs(double num)`：绝对值
   2. `public static double ceil(double unm)`：向上取整
   3. `public static double floor(double num)`：向下取整
   4. `public static long round(double num)`：四舍五入