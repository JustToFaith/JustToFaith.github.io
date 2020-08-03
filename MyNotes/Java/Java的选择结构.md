# Java的选择结构

### 单if语句

```java
if (关系表达式) {
  语句体;
}
```

---

### 多if-else if-else语句

#### if-else

```java
if (关系表达式) {
  语句体;
} else {
  语句体;
}
```

#### if - else if ...else if -else

```java
if (判断语句1) {
  语句体;
} else if (判断语句2) {
  语句体;
}
...
else if (判断语句n) {
  语句体;
} else {
  语句体;
}
```

---

### switch语句

```java
switch(表达式) {
  case 常量值1:
    语句体1;
    break;
  case 常量值2:
    语句体2;
    break;
  ...
  case 常量值n:
    语句体n;
    break;
  default:
    语句体n+1;
    break;
}
```

#### 执行流程

- 首先计算出表达式的值；
- 其次，和`case`依次比较，一旦有对应的值，就会执行相应的语句体。在执行`break`后退出该选择结构；
- 最好，如果比较所有`case`都没有与之对应，那么执行`default`默认语句，然后退出选择结构；



#### 注意事项

1. 多个`case`后面的数值不可以重复
2. `switch`后面的小括号只能是以下几种数值
   - 基本数据类型：`byte`、`short`、`char`、`int`
   - 引用数据类型：`String字符串`、`enum枚举`
3. `switch`语句前后顺序可以颠倒，并且`break`语句可以省略。当省略`break`语句后，匹配到哪个`case`就执行哪条语句，并且一直执行之后的语句，直到遇到`break`或者整体结束为止