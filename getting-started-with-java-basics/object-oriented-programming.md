---
description: >-
  掌握面向对象程序设计的特点和基本概念；掌握类的声明；掌握对象的创建和使用；掌握继承和多态的概念和应用；掌握接口的声明和实现；掌握包的声明、包与类的引入；掌握Java类库常用类的使用
---

# 第一章\~面向对象程序设计

## 1.1 面向对象程序设计的特性和基本概念

### 1.1.1 特性

1. <mark style="color:red;">**封装性**</mark>：封装是面向对象的核心思想。把对象的属性和行为看成一个整体或者隐藏信息
2. <mark style="color:red;">**继承性**</mark>：继承性用于描述类于类之间的关系。通过继承可以在原有类的基础上进行扩展
3. <mark style="color:red;">**多态性**</mark>：实在类的定义的属性和方法被其它类继承后，它们可以具有不同的数据类型或表现出不同的行为

### 1.1.2 基本概念

1. 类：类是对象的抽象，用于描述一组对象的共同特征和行为，即表示某一群体的一些基本特征的抽象
2. 对象：表示一个个具体的事物
3. 成员变量：用于描述对象的特征，也被称为对象的属性
4. 成员方法：用于描述对象的行为，可简称为方法

## 1.2 类的基本组成

### 1.2.1 类的声明

```java
[<访问控制修饰符>] class <类名> {
    成员变量;
    成员方法;
}

// eg: 声明一个学生类
class Student {}
```

### 1.2.2 成员变量的声明

```java
[<访问控制修饰符>] <变量类型> <变量名>;

// eg: 声明一个学生类并包含一个姓名的成员变量
class Student {
    String name;
}
```

### 1.2.3 成员方法的声明与调用，方法的参数传递与返回值

```java
[<访问控制修饰符>] <方法返回值> <方法名>([<参数类型> <参数名> ···]) {
    [return <返回值>]
}

// eg: 声明一个学生类和一个读书的成员方法
class Student {
    String name;
    
    String read(String book) {
        return "我是" + name + ", 我正在看" + book + "书";
    }
}
```

### 1.2.4 方法的重载

{% hint style="info" %}
在同一个作用域内，方法名相同但参数个数或者参数类型不同的方法
{% endhint %}

{% code title="Student 内部" %}
```java
String read() {
    return "我是" + name + ", 我正在看书";
}

String read(String book) {
    return "我是" + name + ", 我正在看" + book + "书";
}
```
{% endcode %}

### 1.2.5 静态变量和静态方法

{% hint style="info" %}
使用 static 关键字修饰的成员变量或成员方法后，则变为了静态变量或者静态方法
{% endhint %}

静态变量可以直接通过 `类名.变量名` 进行访问，是所有对象共享的

## 1.3 对象的创建和使用

### 1.3.1 对象的创建

{% hint style="info" %}
创建一个对象需要使用 new 关键字，

格式为 `new 类名();`

创建并赋值的格式为 `类名 对象名 = new 类名();`
{% endhint %}

```java
// eg: 创建上面的 Student 类并使用 read 方法
Student stu = new Student();
System.out.println(stu.read());
```

### 1.3.2 构造方法与对象的初始化

构造方法又称构造器，定义结构为：

```java
[<访问控制修饰符>] <类名>([<参数类型> <参数名> ···]) {

}
```

{% hint style="warning" %}
默认情况下定义的类会提供一个无参构造方法，但当存在自定义的构造方法后，不在默认提供
{% endhint %}

```java
class Student {
    String name;
    
    public Student(String name) {
        this.name = name;            // 创建学生对象时要求传入学生的姓名进行初始化
    }
}
```

### 1.3.3 成员变量和成员方法的访问

访问成员变量和成员方法的格式为：

`对象名.属性名`

`对象名.方法名`

### 1.3.4 this

{% hint style="info" %}
this 关键字代表当前的代码执行所在的类对象，可以[方便的访问和重名的局部变量](object-oriented-programming.md#1.3.2-gou-zao-fang-fa-yu-dui-xiang-de-chu-shi-hua)
{% endhint %}

## 1.4 类的继承

### 1.4.1 子类的声明

```java
class 类名 extends 父类名 {
    成员变量;
    成员方法;
}

// eg:
class A {}
class B extends A {}
```

### 1.4.2 变量的覆盖和方法的覆盖(重写)

{% hint style="info" %}
子类对继承的属性和方法进行修改，即对父类的重写

<mark style="background-color:red;">对变量的覆盖不会再多态中体现出来</mark>
{% endhint %}

{% hint style="info" %}
方法的重写需要和方法的方法名、返回类型和参数列表相同，同时范围控制修饰符的级别不能低于被重写的方法，且 private 方法不能被重写
{% endhint %}

{% code title="方法的重写" %}
```java
class A {
    String test() { 
        return "A";
    }
}

class B extends A {
    
    @Override
    public String test() {
        return "B";
    }
}
```
{% endcode %}

### 1.4.3 super

{% hint style="info" %}
当子类重写父类的方法后子类无法直接访问被重写的父类方法，需要使用super关键字

使用 super 关键字能访问父类的非私有方法、非私有属性以及构造方法
{% endhint %}

### 1.4.4 final

* 使用 final 修饰的类不能被继承即不能拥有子类
* 使用 final 修饰的方法不能被子类重写
* 使用 final 修改的变量是常量不能被修改，即在第一次赋值后不能再被修改

## 1.5 类及成员的四种访问权限

{% hint style="warning" %}
<mark style="background-color:red;">外部类的修饰符只能是 public 或者 default</mark>
{% endhint %}

<mark style="color:blue;">public</mark>、<mark style="color:purple;">protected</mark>、<mark style="color:green;">default</mark>、<mark style="color:red;">private</mark>

| 访问范围    | public     | protected   | default     | private     |
| ------- | ---------- | ----------- | ----------- | ----------- |
| 同一个类    | :thumbsup: | :thumbsup:  | :thumbsup:  | :thumbsup:  |
| 同一个包中的类 | :thumbsup: | :thumbsup:  | :thumbsup:  | :no\_entry: |
| 不同包的子类  | :thumbsup: | :thumbsup:  | :no\_entry: | :no\_entry: |
| 全局范围    | :thumbsup: | :no\_entry: | :no\_entry: | :no\_entry: |

{% hint style="danger" %}
一个 Java 源文件中可以没有一个 public 类，但是最多只能有一个 public 类，且当有一个 public 类时，源文件的文件名需要和 public 的类名保持一致
{% endhint %}

## 1.6 抽象类与接口

### 1.6.2 抽象类和抽象方法的声明

{% hint style="info" %}
抽象类使用 abstract 关键字进行修饰，抽象类不能创建实例对象，抽象类的子类必实现全部抽象方法，如果不全部实现，则也需要使用 abstract 进行修饰，剩下的抽象方法又交给其子类实现
{% endhint %}

```java
abstract class <抽象类名称> {
    属性;
    普通成员方法;
    
    [<访问控制修饰符>] abstract <返回类型> <方法名>([<参数类型> <参数名> ···]);
}
```

### 1.6.3 接口的声明与实现

> 接口用于描述类和结构的一组行为

{% hint style="info" %}
接口使用 interface 进行修饰，接口可以继承多个父接口，一个类也可以实现多个接口
{% endhint %}

```java
// 接口的定义
[public] interface <接口名> [implements <接口名>[, <接口名> ···]] {
    // 静态常量，同接口名.常量名 进行访问
    [public] [static] [final] <数据类型> <常量名> = <常量>;
    
    // 抽象方法
    [public] [abstract] <返回值数据类型> <方法名>([<参数类型> <参数名>···]);
    
    // 静态方法
    [public] static <返回值数据类型> <方法名>([<参数类型> <参数名>···]) {
        [return <返回值>];
    }
    
    // 默认方法，实现接口的类可以不对其进行实现，接口提供其默认行为
    [public] default <返回值数据类型> <方法名>([<参数类型> <参数名>···]) {
        [return <返回值>];
    }
}
```

{% hint style="danger" %}
接口只支持 public 和 default 两种访问控制，且接口的默认为 default，方法为 public
{% endhint %}

## 1.7 包

{% hint style="info" %}
包的声明使用 package 语句在源文件的第一行代码非注释或空白的代码的位置

使用 import 语句导入类或者静态方法
{% endhint %}

## 1.8 常用类的使用

### 1.8.1 String 类

```java
// String 类的创建
// 方法1
String str1 = "Str1";
// 方法2
String str2 = new String();
String str3 = new String("Str3");
String str4 = new String(new char[] {'S', 't', 'r', '4'});
String str5 = new String(new byte[] {97, 98, 99});

// 判断两个字符串是否相等
str1.equals(str2); // 相等返回 true 不相等返回 false
// 比较连个字符串的大小
str1.compareTo(str2); // 相等返回 0，str1 大返回正整数，str1 小返回负整数

// 获取某个索引位置的字符
char c1 = str1.charAt(1); // c1 = t
int i1 = str1.indexOf('c'); // i1 = 1

// 获取子串
String str = str1.substring(str1.indexOf("Str"), 3); // str = "Str"
```

### 1.8.2 StringBuffer 与 StringBuilder

{% hint style="info" %}
使用 String 定义的字符串是常量，对字符串的修改都会创建一个新的字符串常量，而 StringBuilder 和 StringBuffer 的内容和长度是可以改变的

StringBuilder 不是线程安全的

StringBuffer 是线程安全的
{% endhint %}

```java
// 创建
StringBuilder b = new StringBuilder();
// StringBuilder b = new StringBuilder("init str");
// StringBuilder b = new StringBuilder("init capacity");
b.append(1); // b = "1"
b.append("11"); // b = "111"
```

### 1.8.3 Math 类和 Random 类

{% hint style="info" %}
Math是数学相关的工具类，位于 java.lang 包,可以直接使用

Random 是随机数工具类，也位于 java.util 包
{% endhint %}

### 1.8.4 基本类型的包装类

除了 int 和 char 分别对应 Integer 和 Character，其它都是首字母大写

### 1.8.5 Object 类和 Class 类

1. Object 类：是所有类的直接或间接父类，不使用 extends 默认继承至 Object，提供 hashCode、equals、toString 三级基本方法
2.  Class 类：

    可以通过 Class 类获取类的信息，获取 Class 类的实例可以通过 <mark style="background-color:red;">Class.forName("全限定类名")、对象名.getClass()、类名.class</mark> 这三种方式进行获取
