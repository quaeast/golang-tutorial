16 - 结构体  
========================

上一节：[第十篇 if else 语句](/docs/golang_tutorial_10.md)   
下一节：[第十二篇 包](/docs/golang_tutorial_12.md)  

这是本Golang系列教程的第16篇。   

## 什么是结构体？

结构体是一个由用户定义的类型，它代表一组字段。他适用于将一组数据打包到一一个单元而不是分开管理的情况。

比如一个员工有姓、名和年龄。把这三个值集合成一组放到一个结构体 `employee` 中就很合理。

## 声明结构体

```golang
type Employee struct {  
    firstName string
    lastName  string
    age       int
}
```

上面的代码中声明了一个结构体类型 `Employee`, 它有字段 firstName, lastName 和 年龄。这个结构体也能更加紧凑，通过把同样类型的字段声明在同一行里然后类型放到他们的后面。在上面的结构体中，`firstName` 和 `lastName` 属于同一个类型 `string` 因此这个结构体可以被重写为：

```golang
type Employee struct{
  firstName, lastName string
  age, salary int
}
```

上面的 `Employee` 结构体被称为 `命名结构体` 因为它创造了个新的类型叫做 `Employee` ，它可以被用来创建 `Employee` 类的结构体。

声明一个结构体而不声明一个新的类型是可以的，这叫做 `匿名结构体`。

```golang
var employee struct{
  firstName, lastName string
  age int
}
```

上面的片段创建了一个 **匿名结构体** `employee`

## 创建命名结构体

让我们定义一个 **命名结构体 Employee** 用一段简短的代码

```golang
package main

import (
  "fmt"
)

type Employee struct {
  firstName, lastName string
  age, salary int
}

func main() {

  //creating structure using field names
  emp1 := Employee{
    firstName: "Sam",
    age: 25,
    salary: 500,
    lastName: "Anderson",
  }

  //creating structure without using field names
  emp2 := Employee{"Thomas", "Paul", 29, 800}

  fmt.Println("Employee 1", emp1)
  fmt.Println("Employee 2", emp2)
}
```

在上面程序的第 7 行，我们创建了一个明明结构体 `Employee`。在上面程序的第 15 行，
