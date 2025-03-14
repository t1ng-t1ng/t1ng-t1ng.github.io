---
title: Go
subtitle:
date: 2025-03-14T12:08:30+08:00
slug: 130c926
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
  - Go
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
  enable: true
  url:

# See details front matter: https://fixit.lruihao.cn/documentation/content-management/introduction/#front-matter
---

<!--more-->

<!-- Place resource files in the current article directory and reference them using relative paths, like this: `![alt](images/screenshot.jpg)`. -->

## 简介

- Go 是一种跨平台的开源编程语言
- Go 可用于创建高性能应用程序
- Go 是一种快速、静态类型的编译语言，以其简单性和效率而闻名
- Go 由 Google 的 Robert Griesemer、Rob Pike 和 Ken Thompson 于 2007 年开发
- Go 的语法与 C++ 类似

### Go 的用途是什么？

- Web 开发（服务器端）
- 开发基于网络的程序
- 开发跨平台企业应用程序
- 云原生开发



## Hello World

``` Go
package main
import("fmt")

func main(){
        fmt.Println("Hello World!")
}
```


## Go语法

一个Go文件由以下部分组成：

- 包声明
- 导入包
- 功能
- 语句和表达式

使用 `package`关键字来定义它。在此示例中，程序属于包 `main`
 `import ("fmt")`导入`fmt`包中包含的文件
 `func main() {}`是一个函数。其花括号内的任何代码都`{}`将被执行
 在 Go 中，语句通过行尾（按 Enter 键）或分号“ `;`”来分隔
 左花括号`{`不能位于行首
 单行注释以两个正斜杠 ( `//`) 开头
 多行注释以 开头`/*`，以 结尾`*/`
`/*`和之间的任何文本`*/`将被编译器忽略


## Go变量

### 声明变量

#### Go 变量类型

- `int`- 存储整数（整数），例如 123 或 -123
- `float32`- 存储带有小数的浮点数，例如 19.99 或 -19.99
- `string`- 存储文本，例如“Hello World”。字符串值用双引号括起来
- `bool`- 存储两种状态的值：true 或 false


#### 创建变量

##### 1.使用 var 关键词，后跟变量名称和类型

``` Go
var variablename type = value  //声明variablename变量 ， 必须指定其中一个`type` 或`value`
```

##### 2. 附有 := 标志，后跟变量值

``` Go
variablename := value
```

变量的类型是根据值**推断出来的**（意味着编译器根据值来决定变量的类型）


#### 带初始值的变量声明

``` Go
package main
import ("fmt")

func main() {
        var student1 string = "John" //type is string
        var student2 string = "Jane" //type is inferred
        x := 2 //type is inferred

        fmt.Println(student1)
        fmt.Println(student2)
        fmt.Println(x)
    //student2 和 x 的变量类型根据赋予的值所判断的
}
```

#### 无初值的变量声明

``` Go
package main
import ("fmt")

func main(){
        var a string
        var b int
        var c bool

        fmt.Println(a)
        fmt.Println(b)
        fmt.Println(c)
}
```

运行代码可得到各自默认值

``` Go
a是""，b是0，c是false
```


#### 声明变量后赋值

``` Go
package mian
improt("fmt")

fun main() {
  var student1 string
  student1 = "John"
  fmt.Println(student1)
}
```

> [!NOTE]
> 不能使用":="声明变量而不为其分配值

``` Go
0
false
John
```

#### var 和 := 之间的区别

var 可以在函数内部或外部都可使用
:= 只能在函数内部进行使用

``` Go
package main
import ("fmt")

var a int
var b int = 2
var c = 3

func main() {
  a = 1
  fmt.Println(a)
  fmt.Println(b)
  fmt.Println(c)
}
```

``` Go
0
2
3
```

如果在func  main函数外使用，运行程序会导致错误

``` GO
./prog.go:5:1: syntax error: non-declaration statement outside function body
```


### 多个变量声明

可以在同一行声明多个变量

``` Go
package main
improt ("fmt")

func main () {
  var g, h, j, k int =1, 3, 5, 7

  fmt.Println(g)
  fmt.Println(h)
  fmt.Println(j)
  fmt.Println(k)
} 
```

> [!NOTE]
> 如果使用`类型` 关键字，则每行只能声明**一种类型**的变量

``` Go
package main
import ("fmt")

func main(){
  var l, m = 6, "Hello!"
  n ,o := 7, "World!"
  fmt.Println(l)
  fmt.Println(m)
  fmt.Println(n)
  fmt.Println(o)
}
```

#### 在块中声明变量

``` Go
package main  
import ("fmt")  
  
func main() {  
   var (  
     a int  
     b int = 1  
     c string = "hello"  
   )  
  
  fmt.Println(a)  
  fmt.Println(b)  
  fmt.Println(c)  
}
```


### 命名规则

Go变量命名规则：

- 变量名必须以字母或下划线 (_) 开头
- 变量名不能以数字开头
- 变量名只能包含字母数字字符和下划线（`a-z, A-Z`、`0-9`和`_`）
- 变量名区分大小写（age、Age 和 AGE 是三个不同的变量）
- 变量名的长度没有限制
- 变量名不能包含空格
- 变量名不能是任何 Go 关键字

#### 多词变量名

myVariableName = "John"

MyVariableName = "John"

my_variable_name = "John"


## Go常量

语法：const CONSTANAME type = value
常量通常用大写字母表示
常量可以在函数内部和外部声明

> [!NOTE]
> 声明变量时必须指定值

### 声明常量

``` Go
package main
import ("fmt")

const PI = 3.14

func main() {
  fmt.Println(PI)
}
```

### 常量类型

1. 类型常量
2. 无类型常量

#### 类型常量

``` Go
package main  
import ("fmt")  
  
const A int = 1  
  
func main() {  
  fmt.Println(A)  
}
```

#### 无类型变量

``` Go
package main  
import ("fmt")  
  
const A = 1  
  
func main() {  
  fmt.Println(A)  
}
```

无类型变量的值是编译器根据值来决定类型

常量只能读取，不能改变

### 多个常量声明

``` Go
package main  
import ("fmt")  
  
const (  
  A int = 1  
  B = 3.14  
  C = "Hi!"  
)  
  
func main() {  
  fmt.Println(A)  
  fmt.Println(B)  
  fmt.Println(C)  
}
```

## 输出

Go有三个函数用于输出文本

1. Print()
2. Println()
3. Printf()

### Print()函数

以默认格式打印其参数

``` Go
package main  
import ("fmt")  
  
func main() {  
  var i,j string = "Hello","World"  
  
  fmt.Print(i)  
  fmt.Print(j)  
}
```

``` 
HelloWorld
```

在新行中打印参数

``` Go
package main  
import ("fmt")  
  
func main() {  
  var i,j string = "Hello","World"  
  
  fmt.Print(i, "\n")  
  fmt.Print(j, "\n")  
}
```

```
Hello  
World
```

只使用一个`Print()`来打印多个变量

``` Go
package main  
import ("fmt")  
  
func main() {  
  var i,j string = "Hello","World"  
  
  fmt.Print(i, "\n",j)  
}
```

```
Hello  
World
```

在字符串参数之间添加空格，需要使用“”

``` Go
package main  
import ("fmt")  
  
func main() {  
  var i,j string = "Hello","World"  
  
  fmt.Print(i, " ", j)  
}
```

```
Hello World
```

`Print()`**如果参数都不**是字符串，则在参数之间插入空格

``` Go
package main  
import ("fmt")  
  
func main() {  
  var i,j = 10,20  
  
  fmt.Print(i,j)  
}
```

```
10 20
```

### Println() 函数

`Print()`不同之处在于参数之间添加了空格，并且在末尾添加了换行符

``` Go
package main  
import ("fmt")  
  
func main() {  
  var i,j string = "Hello","World"  
  
  fmt.Println(i,j)  
}
```

```
Hello World
```

### Printf() 函数

函数首先根据给定的格式化动词格式化其参数，然后打印

- `%v`用于打印 参数的**值**
- `%T`用于打印 参数的**类型**

``` Go
package main  
import ("fmt")  
  
func main() {  
  var i string = "Hello"  
  var j int = 15  
  
  fmt.Printf("i has value: %v and type: %T\n", i, i)  
  fmt.Printf("j has value: %v and type: %T", j, j)
```

```
i has value: Hello and type: string  
j has value: 15 and type: int
```

