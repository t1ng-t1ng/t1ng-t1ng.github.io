---
title: Java
subtitle:
date: 2025-03-14T12:15:30+08:00
slug: d4b1714
draft: false
author:
  name: T1ng
  link:
  email:
  avatar: /images/avater.jpg
description:
keywords:
license:
comment: false
weight: 0
tags:
  - Java
categories:
  - Code
hiddenFromHomePage: false
hiddenFromSearch: false
hiddenFromRelated: false
hiddenFromFeed: false
summary:
resources:
  - name: featured-image
    src: featured-image.jpg
  - name: featured-image-preview
    src: featured-image-preview.jpg
toc: true
math: false
lightgallery: false
password:
message:
repost:
  enable: false
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->

<!-- Place resource files in the current article directory and reference them using relative paths, like this: `![alt](images/screenshot.jpg)`. -->

## Java教程

### Java语法

Java 区分大小写：“MyClass”和“myclass”具有不同的含义
java 文件的名称必须与类名匹配。保存文件时，使用类名保存，并在文件名末尾添加“.java”

```java
public static void main(String[] args)
```

方法内的任何代码都`main()`将被执行

System.out.pritlnm()
`System`是一个内置 Java 类，包含有用的成员，例如`out`，它是“输出”的缩写。`println()`方法是“打印行”的缩写，用于将值打印到屏幕（或文件）。

### Java输出

可以添加`println()`任意数量的方法

```java
System.out.println("Hello World!");
System.out.println("I am learning Java.");
System.out.println("It is awesome!");
```

处理文本时，必须将其括在双引号内`""`。如果忘记双引号，则会出现错误：
Print()方法
它不会在输出末尾插入新行

```java
System.out.print("Hello World! ");
System.out.print("I will print on the same line.");
```

Hello World! I will print on the same line.

使用该`println()`方法来打印数字，与文本不同，不将数字放在双引号内

```java
System.out.println(3);
System.out.println(358);
System.out.println(50000);
```

可以在`println()`方法内部执行数学计算

```java
System.out.println(3 + 3);
```

### Java注释

单行注释以两个正斜杠 ( `//`) 开头。`//`Java 会忽略该行和该行末尾之间的任何文本（将不会被执行）。

```java
// This is a comment
System.out.println("Hello World");
```

多行注释以 开头`/*`，以 结尾`*/`。
`/*`和之间的任何文本`*/`都将被 Java 忽略。

```java
/* The code below will print the words Hello World
to the screen, and it is amazing */
System.out.println("Hello World");
```

### Java变量

#### 变量

- `String`- 存储文本，例如“Hello”。字符串值用双引号括起来
- `int`- 存储整数（整数），不带小数，例如 123 或 -123
- `float`- 存储带有小数的浮点数，例如 19.99 或 -19.99
- `char`- 存储单个字符，例如 'a' 或 'B'。字符值用单引号括起来
- `boolean`- 存储两种状态的值：true 或 false

声明创建变量，必须指定类型并为其赋值

```java
type variableName = value;
```

type是Java的类型之一（例如int或 String），variableName是变量的名称，等号用于为变量赋值。
例：
若创建一个存储文本变量：创建一个名为name类型的变量String并为其赋值“Jhon”

```java
String name = "John";
System.out.println(name);
```

若创建一个存储数字的变量：创建一个名为myNum类型的变量int并为其赋值15

```java
int myNum = 15;
System.out.println(myNum);
```

声明变量而不分配值，稍后再分配值

```java
int myNum;
myNum = 15;
System.out.println(myNum);
```

如果为现有变量分配新值，将覆盖原来的值

```java
int myNum = 15;
myNum = 20;    // myNum is now 20
System.out.println(myNum);
```

Final变量
不想让其他人（或您自己）覆盖现有值，使用final（会将变量声明为“final”或“c​​onstant”，这意味着不可更改和只读）

```java
final int myNum = 15;
myNum = 20;  // will generate an error: cannot assign a value to a final variable
```

*java: 无法为最终变量myNum分配值*

```java
int myNum = 5;
float myFloatNum = 5.99f;
char myLetter = 'D';
boolean myBool = true;
String myText = "Hello";
```

#### 打印变量

显示变量
println()方法常用来显示变量
要组合文本和变量，用+字符

```java
String name = "John";
System.out.println("Hello " + name);
```

还可以用+字符将一个变量添加到另一个变量

```java
String firstName = "John ";
String lastName = "Doe";
String fullName = firstName + lastName;
System.out.println(fullName);
```

对于数值，+字符充当数学运算符（使用`int`（整数）变量）

```java
int x = 5;
int y = 6;
System.out.println(x+y);   //Print the value of x + y
```

#### 多个变量

声明多个同一类型的变量，可用逗号分割

```java
int x = 5,  y = 6, z = 50;
System.out.println(x + y + z);
```

一个值对应多个变量

```java
int x, y, z;
x = y = z = 50;
System.out.println(x + y + z);
```

#### 标识符

所有 Java**变量**都必须 具有唯一的名称来标识
建议使用描述性名称以创建可理解和可维护的代码

```java
// Good
int minutesPerHour = 60;

// OK, but not so easy to understand what m actually is
int m = 60;
```

变量命名的一般规则

- 名称可以包含字母、数字、下划线和美元符号
- 名称必须以字母开头
- 名称应以小写字母开头，且不能包含空格
- 名称也可以以 $ 和 _ 开头（但在本教程中我们不会使用它）
- 名称区分大小写（“myVar”和“myvar”是不同的变量）
- 保留字（如 Java 关键字，例如`int`或 `boolean`）不能用作名称

创建程序存储有关大学生的不同数据类型

```java
//  Student data
String studentName = "John Doe";
int studentID = 15;
int studentAge = 25;
float studentFee = 75.25f;
char studentGrade = 'B';

//    Print variables
System.out.println("Student Name："+ studentName);  
System.out.println("Student ID：" + studentID);  
System.out.println("Student Age：" + studentAge);  
System.out.println("Student Fee：" + studentFee);  
System.out.println("Student Grade：" + studentGrade);
```

计算矩形面积

```java
//    Creat integer variable
int length = 4;
int width = 6;
int area;

//    Calculate the area of a rectangle
area = length * width;

//    Print variables
System.out.println("Length is ：" + length);
System.out.println("Width is ：" + width);
System.out.println("Area of the rectangle is ：" + area);
```

### Java数据类型

#### 数据类型

- 原始数据类型-包括`byte``short`​`int``long``float``double``boolean``char`
- 非原始数据类型 - 例如[String]，数组和 类

原始数据类型
原始数据类型指定变量值的大小和类型，并且没有附加方法

| Data Type | Size | Description                                                  |
| --------- | ---- | ------------------------------------------------------------ |
| byte      | 1    | Stores whole numbers from -128 to 127                        |
| short     | 2    | Stores whole numbers from -32,768 to 32,767                  |
| int       | 4    | Stores whole numbers from -2,147,483,648 to 2,147,483,647    |
| long      | 8    | Stores whole numbers from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| float     | 4    | Stores fractional numbers. Sufficient for storing 6 to 7 decimal digits |
| double    | 8    | Stores fractional numbers. Sufficient for storing 15 decimal digits |
| boolean   | 1    | Stores true or false values                                  |
| char      | 2    | Stores a single character/letter or ASCII values             |

#### 数字

原始数字分为两类：
整数类型    存储整数，正数，负数（例如 123或-456）有效类型为 int long byte short
浮点类型    表示带有小数部分的数字，包含一个或多个小数，两种类型float 和douclew

##### 整数类型

byte
可以存储从-128到127的整数。当值在以上区间时，可以使用byte代替其他整数类型，节省空间

```java
byte myNum = 100;
System.out.println(myNum);
```

short
可以存储从-32768到32767的整数

```java
short myNUm = 5000;
System.out.println(myNum);
```

int
可以存储-2147483648到21247483647的整数

```java
int myNum = 10000；
System.out.println(myNum);
```

long
可以存储从 -9223372036854775808 到 9223372036854775807 的整数。应以"L"结尾该值

```java
long myNum = 15000000000L;
System.out.println(myNum);
```

##### 浮点类型

float和double都可以存储小数。
浮点数的值应该以“f”结尾，而双精度的值应以“d”结尾

```java
float myNum = 5.75f;
System.out.println(myNum);
```

```java
double myNum = 19.99d;
System.out.println(myNum);
```

使用float和double？
浮点值的精度表示该值在小数点后可以有多少位数字，精度只有六到七位小数，而double的精度为15位。因此大多数，使用浮点数更为安全。

##### 科学数字

浮点数也可以是带有“e”的科学数，，表示10的幂

```java
float f1 =35e3f;
double d1 = 12E4d;
System.out.println(f1);
System.out.println(d1);
```

#### 布尔值

Java具有一种boolean数据结构，只能采用以下值ture和false

```java
boolean isJavaFun = ture;
boolean isFishTasty = false;
System.out.println(isJavaFun);   //Outputs ture
System.out.println(isFishTasty);    //Outputs false
```

#### Characters

char数据类型用于存储单个字符。字符必须用单引号括起来，例'A'

```java
char myGrade = 'A';
System.out.println(myGrade);
```

如果熟悉ASCII值，也可以用这些值显示字符

```java
char myVar1 = 65, myVar2 = 66, myVar3 = 67;
System.out.println(myVar1);     //Outputs    A
System.out.println(myVar2);    //Outputs    B
System.out.println(myVar3);    //Outputs    C
```

#### Strings

Strings数据类型用于存储字符序列（文本）。字符串类型必须用双引号括起来

```java
Strings greeting = "Hello World";
System.out.println(greeing);
```

Java 中的字符串实际上是一种**非原始**数据类型，因为它指的是对象。String 对象具有用于对字符串执行某些操作的方法。

#### 实例

```java
// Create variables of different data types
int items = 50;
float costPerItem = 9.99f;
float totalCost = items * costPerItem;
char currency = '$';

// Print variables
System.out.println("Number of items: " + items);
System.out.println("Cost per item: " + costPerItem + currency);
System.out.println("Total cost = " + totalCost + currency);
```


#### Java非原始数据类型

非原始数据类型被称为**引用类型，因为它们引用对象**
**原始**数据类型和**非原始**数据类型之间的主要区别是：

- 原始类型在 Java 中是预定义的（已经定义）。非原始类型由程序员创建，并且不是由 Java 定义的（除了`String`）。
- 非原始类型可以用来调用方法来执行某些操作，而原始类型则不能。
- 原始类型总是有一个值，而非原始类型则可以`null`。
- 原始类型以小写字母开头，而非原始类型以大写字母开头。
  非原始类型的示例有[字符串](https://www.w3schools.com/java/java_strings.asp)、[数组](https://www.w3schools.com/java/java_arrays.asp)、[类、](https://www.w3schools.com/java/java_classes.asp)[接口](https://www.w3schools.com/java/java_interface.asp)等

### Java类型转换

类型转换是将一种原始数据类型的值分配给另一类型
在Java有两种类型转换：
    扩展类型转换（自动）    - 将较小的类型转换为较大的类型
byte->short->char->int->long->float->double
     缩小类型转换（手动）    - 将较大的类型转换为较小的类型
double->float->long->int->char->short->byte

#### 加宽铸造

将较小的尺寸类型传递给较大的尺寸类型时，会自动进行扩展转换

```java
public class Main {
  public static void main(String[] args) {
    int myInt = 9;
    double myDouble = myInt; // Automatic casting: int to double

    System.out.println(myInt);      // Outputs 9
    System.out.println(myDouble);   // Outputs 9.0
  }
}
```

#### 缩小铸造范围

必须通过将类型`()` 放在值前面的括号中来手动完成缩小转换

```java
public class Main {
  public static void main(String[] args) {
    double myDouble = 9.78d;
    int myInt = (int) myDouble; // Manual casting: double to int

    System.out.println(myDouble);   // Outputs 9.78
    System.out.println(myInt);      // Outputs 9
  }
}
```

#### 实例

创建一个程序来计算用户得分占游戏最高得分的百分比，使用类型转换来确保结果是浮点值，而不是整数

```java
// Set the maximum possible score in the game to 500
int maxScore = 500;

// The actual score of the user
int userScore = 423;

/* Calculate the percantage of the user's score in relation to the maximum available score.
Convert userScore to float to make sure that the division is accurate */
float percentage = (float) userScore / maxScore * 100.0f;

System.out.println("User's percentage is " + percentage);
```


### Java运算符

运算符用于对变量和值执行运算

```java
int x = 100 + 50;
```

```java
int sum1 = 100 + 50;        // 150 (100 + 50)
int sum2 = sum1 + 250;      // 400 (150 + 250)
int sum3 = sum2 + sum2;     // 800 (400 + 400)
```

Java 将运算符分为以下几组：

- 算术运算符
- 赋值运算符
- 比较运算符
- 逻辑运算符
- 按位运算符

###### 算术运算符

| 运算符 | 名称 | 描述                   | 示例  |
| ------ | ---- | ---------------------- | ----- |
| +      | 加法 | 将两个值相加           | x + y |
| -      | 减法 | 从一个值中减去另一个值 | x - y |
| *      | 乘法 | 将两个值相乘           | x * y |
| /      | 除法 | 将一个值除以另一个值   | x / y |
| %      | 取余 | 返回除法的余数         | x % y |
| ++     | 自增 | 将变量的值增加 1       | ++x   |
| --     | 自减 | 将变量的值减少 1       | --x   |

###### Java赋值运算符

赋值运算符用于给变量赋值。

```java
int x = 10;
```

加法**赋值运算**符 ( `+=`) 为变量添加一个值：

```java
int x = 10;
x += 5;
```

| 运算符 | 示例     | 相同       |
| ------ | -------- | ---------- |
| =      | x = 5    | x = 5      |
| +=     | x += 3   | x = 3 + 3  |
| -=     | x -= 3   | x = x -3   |
| *=     | x *= 3   | x = x * 3  |
| /=     | x /= 3   | x = x/3    |
| %=     | x %= 3   | x = x % 3  |
| &=     | x &= 3   | x = x & 3  |
| \| =   | x \| = 3 | x = x \| 3 |
| ^=     | x ^= 3   | x = x ^ 3  |
| >>=    | x >>= 3  | x = x >> 3 |
| <<=    | x <<= 3  | x = x<< 3  |

###### Java比较运算符

比较运算符用于比较两个值（或变量），比较的返回值为 或`true`，`false`这些值称为 _布尔值_

```java
int x = 5;
int y = 3;
System.out.println(x > y); // returns true, because 5 is higher than 3
```

| 运算符 | 名称       | 示例   |
| ------ | ---------- | ------ |
| ==     | 等于       | x == y |
| !=     | 不等于     | x != y |
| >      | 大于       | x >y   |
| <      | 小于       | x < y  |
| >=     | 大于或等于 | x >= y |
| <=     | 小于或等于 | x <= y |

###### Java逻辑运算符

| 运算符 | 名称   | 描述                           | 示例               |
| ------ | ------ | ------------------------------ | ------------------ |
| &&     | 逻辑与 | 如果两个语句都为真，则返回真\| | x < 5 &&  x < 10   |
| \|\|   | 逻辑或 | 如果其中一个语句为真，则返回真 | x < 5 \|\| x < 4   |
| !      | 逻辑非 | 反转结果，如果结果为真则返回假 | !(x < 5 && x < 10) |

### Java字符串

字符串用于存储文本

#### 字符串

##### Java字符串

变量`String`包含用双引号括起来的字符集合

```java
String greeting = "Hello";
```

##### 字符串长度

length()可以表示字符串的长度

```java
String txt = "ABCDEFGHIJKL";
System.out.println("The length of the txt string is: " + txt.length());
```

##### 更多字符串方法

```java
toUpperCase()，toLowerCase()
String txt = "Hello World";
System.out.println(txt.toUpperCase());    //Outputs "HELLO WORLD"
System.out.println(txt.toLowerCase());    //Outputs "hello world"
```

##### 在字符串中查找字符

indexOf() 方法返回字符串中第一次出现的指定文本的索引（位置）（包括空格）：

```java
String txt = "Please locate where 'locate' occurs!";
System.out.println(txt.indexOf("locate")); // Outputs 7
```

#### String Concatenation

运算符号+可以在字符串中使用

```java
String firstName = "Jhon";
String lastName = "Doe";
System.out.println(firstName + " " + lastName);
```

同样可以使用concat()方法连接字符串

```java
String firstName = "Jhon";
String lastName = "Doe";
System.out.println(firstName.concat(lastName));
```

#### Java数字和字符串

如果将两个数字相加，结果将是一个数字

```java
int x = 10;
int y = 20;
int z = x + y;  // z will be 30 (an integer/number)
```

如果添加两个字符串，结果将是字符串连接

```java
String x = "10";
String y = "20";
String z = x + y;  // z will be 1020 (a String)
```

如果添加一个数字和一个字符串，结果将是字符串连接

```java
String x = "10";
int y = 20;
String z = x + y;  // z will be 1020 (a String)
```

##### Java特殊字符

因为字符串必须用引号引起来，所以 Java 会误解这个字符串，并产生错误

```java
String txt = "We are the so-called "Vikings" from the north.";
```

避免此问题的方法是使用**反斜杠转义字符**
