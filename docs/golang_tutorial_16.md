16 - 结构体  
========================

上一节：[第十篇 if else 语句](/docs/golang_tutorial_10.md)   
下一节：[第十二篇 包](/docs/golang_tutorial_12.md)  

这是本Golang系列教程的第16篇。   

## 什么是结构体？

结构体是一个由用户定义的类型，它代表一组字段。他适用于将一组数据打包到一个单元而不是分开管理的情况。

比如一个员工有姓、名和年龄。把这三个值集合成一组放到一个结构体 `employee` 中就很合理。

## 声明结构体

```golang
type Employee struct {  
    firstName string
    lastName  string
    age       int
}
```

上面的代码中声明了一个结构体类型 `Employee`, 它有字段 firstName, lastName 和 age 。这个结构体也能更加紧凑，通过把同样类型的字段声明在同一行里然后把类型放到他们的后面。在上面的结构体中，`firstName` 和 `lastName` 属于同一个类型 `string` ，因此这个结构体可以被重写为：

```golang
type Employee struct{
  firstName, lastName string
  age, salary int
}
```

上面的 `Employee` 结构体被称为 `命名结构体` 因为它创造了个新的类型叫做 `Employee` ，它可以被用来创建 `Employee` 类的结构体变量。

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

在上面程序的第 7 行，我们创建了一个命名结构体 `Employee`。在上面程序的第 15 行， `emp1` 结构体通过具体说明各值名字的方式定义。这样各个值就不需要按照特定的顺序声明。在这里如果我们改变 `lastName` 的位置并放到最后一排是可行的，没有任何问题。

在上面程序的第 23 行，`emp2` 被省略字段名声明。在这种情况下按顺序声明各个字段就是必要的。

程序的输出如下

```
Employee 1 {Sam Anderson 25 500}  
Employee 2 {Thomas Paul 29 800}  
```

## 创建匿名结构体

```golang
package main

import (
  'fmt'
)

func main() {
  emp3 := struct {
    firstName, lastName string
    age, salary int
  }{
    firstName: "Andreach",
    lastName: "Nikola",
    age: 31,
    salary: 5000,
  }

  fmt.Println("Employee 3", emp3)
}
```

在上面代码的第 8 行，一个 **匿名结构体变量** `emp3` 被定义。像我们提过的，这个结构体是匿名的，因为他只创建一个新的结构体变量 `emp3` ，并且不定义任何新结构体类型。

这个程序输出：

```
Employee 3 {Andreah Nikola 31 5000}  
```

## 结构体的零值

当一个结构体被定义并且没有对值进行初始化，这个结构体的字段被赋值为他们的默认零值。

```golang
package main

import (
  "fmt"
  )

type Employee struct {
  firstName, lastName string
  age, salary int
}

func main()  {
  var emp4 Employee //zero valued structure
  fmt.Println("Employee 4", emp4)
}
```

上面的程序中定义了 `emp4` 但没有初始化值。因此 `firstName` 和 `lastName` 被幅值为 `string` 的零值 `""`，而 `age`, `salary` 被幅值为 `int` 的零值 `0`。这段程序的输出为。

```
Employee 4 {  0 0}  
```

也可以特殊指定某几个字段而忽略其他字段。在这种情况下，忽略字段被赋为零值。

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
  emp5 := Employee{
    firstName: "John",
    lastName: "Paul",
  }
  fmt.Println("Employee 5", emp5)
}
```

在上面程序的第 14 和 15 行， `firstName` 和 `lastName` 被初始化了，然而 `age` 和 `salary` 却没有。因此 `age` 和 `salary` 被赋了他们的零值。这个程序的输出为：

```
Employee 5 {John Paul 0 0}   
```

## 访问结构体的个别字段

点 `.` 运算符被用来访问结构体的个别字段。

```golang
package main

import (
  "fmt"
)

type Employee struct {
  firstName, lastName string
  age, salary int
}

func main()  {
  emp6 := Employee{"Sam", "Anderson", 55, 6000}
  fmt.Println("First Name:", emp6.firstName)
  fmt.Println("Last Name:", emp6.lastName)
  fmt.Println("Age:", emp6.age)
  fmt.Println("Salary:", emp6.salary)
}
```

**emp6.firstName** 在上面的程序里访问 `emp6` 结构体的 `firstName` 字段。这个程序的输出为：

```
First Name: Sam  
Last Name: Anderson  
Age: 55  
Salary: $6000  
```

先创建一个零值结构体再去给各个字段赋值也是可以的。

```golang
package main

import (
  "fmt"
)

type Employee struct {
  firstNamem, lastName string
  age, salary int
}

func main() {
  var emp7
}
```

在上面的程序中 `emp7` 先被定义，`firstName` 和 `lastName` 后被幅值。这个程序的输出如下：

```
Employee 7: {Jack Adams 0 0}  
```

## 结构体指针

创建一个结构体指针也是可行的

```golang
package main

import (
  "fmt"
)

type Employee struct {
  firstName, lastName string
  age, salary int
}

func main()  {
  emp8 := &Employee{"Sam", "Anderson", 55, 6000}
  fmt.Println("First Name:", (*emp8).firstName)
  fmt.Println("Age:", (*emp8).age)
}
```

emp8 在上面的程序中是 `Employee` 结构体的一个指针。 `(*emp8).firstName` 是访问 `emp8` 结构体的 `firstName` 字段的语法。这个程序的输出为：

```
First Name: Sam  
Age: 55  
```

**这个语言给我们一个选择：用`emp8.firstName`而不用特地通过取值 `(*emp8).firstName` 去访问 `firstName` 字段**

```golang
package main

import (
  "fmt"
)

type Employee struct {
  firstName, lastName string
  age, salary int
}

func main()  {
  emp8 := &Employee{"Sam", "Anderson", 55, 6000}
  fmt.Println("First Name", emp8.firstName)
  fmt.Println("Age", emp8.age)
}
```

我们用过 `emp8.firstName` 去访问 `firstName` 字段在上面的程序中。程序的输出为：

```
First Name: Sam  
Age: 55  
```

## 匿名字段

创建一个只包含字段类型而不包含字段名的结构体是可以的。这类字段被称为匿名字段。

这段代码创建了一个 `Person` 结构体，它有两个匿名字段 `string` 和 `int`。

```golang
type Person struct {
  string
  int
}
```

让我们用匿名字段写一个程序

```golang
package main

import (
  "fmt"
)

type Person struct {
  string
  int
}

func main() {
  p := Person{"Naveen", 50}
  fmt.Println(p)
}
```

在上面的程序里，`Person` 是一个有两个匿名字段的结构体。`p := Person{"Naveen", 50}` 定义了一个 `Person` 类型的变量。这个程序输出 `{Naveen 50}`。

**尽管匿名字段没有名字，但默认情况下匿名字段的名字是类型名。** 比如在上面 `Person` 结构体的例子中，尽管字段是匿名的，但默认情况下他们选取类型名作为字段名。所以 `Person` 结构体有 2 个字段的名字是 `string` 和 `int`。

```golang
package main

import (
  "fmt"
)

type Person struct {
  string
  int
}

func main() {
  var p1 Person
  p1.string = "naveen"
  p1.int = 50
  fmt.Println(p1)
}
```

在上面程序的第 14 行和 15 行，我们访问 `Person` 结构体通过他们各自的类型名 `string` 和 `int`。

```
{naveen 50}
```

## 嵌套的结构体（Nested structs）

结构体里面包含一个结构体字段是可以的。这类结构体被称为嵌套的结构体。

```golang
package main

import (
  "fmt"
)

type Address struct {
  city, state string
}
type Person struct {
  name string
  age int
  address Address
}

func main() {
  var p Person
  p.name = "Naveen"
  p.age = 50
  p.address = Address {
    city: "Chicago"
    state: "Illinois"
  }
  fmt.Println("Name:", p.name)
  fmt.Println("Age", p.age)
  fmt.Println("City:", p.address.city)
  fmt.Println("State:", p.address.state)
}
```

上面的 `Person` 结构体有字段 `address` 它也是一个结构体。这个程序的输出如下：

```
Name: Naveen  
Age: 50  
City: Chicago  
State: Illinois  
```

## 提升字段 （Promoted fields）

属于匿名结构体字段的字段被称为提升字段，因为他们能被访问就好像他们属于包括匿名字结构体段的结构体。这个定义太复杂了，所以还是看看代码吧 :)。

```golang
type Address struct {
  city, state string
}
type Person struct {
  name string
  age int
  Address
}
```

在上面的代码段中，`Person` 结构体有一个匿名字段 `Address` 是结构体。现在这个 `Address` 的字段也就是 `city` 和 `state` 被称为提升字段因为他们能够被访问就好像直接在 `Person` 中声明的一样。

```golang
package main

import (
  "fmt"
)

type Address struct {
  city, state string
}
type Person struct {
  name string
  age int
  Address
}

func main() {
  var p Person
  p.name = "Naveen"
  p.age = 50
  p.Address = Address{
    city: "Chicago",
    state: "Illinois",
  }
  fmt.Println("Name:", p.name)
  fmt.Println("Age:", p.age)
  fmt.Println("City:", p.city)
  fmt.Println("Statee:", p.state)
}
```

在上面程序的第 26 和 27 行，提升字段 `city` 和 `state` 被访问就好像他们直接在结构体 `p` 中被声明，可以直接用 `p.city` 和 `p.state` 访问。这个程序输出为：

```
Name: Naveen  
Age: 50  
City: Chicago  
State: Illinois  
```

## 导出结构体和字段

如果结构体类型以大写字母开头，这就是一个导出类型，然后他就能在包外被访问。类似地，如果这个结构体的字段以大写字母开头，他们就能在包外被访问。

我们写一个程序，它有一个特指的包来更好的理解这个。

创建一个文件夹叫 `structs` 在你工作空间中的 `src` 文件夹中。创建另一个文件夹 `computer` 在 `structs` 里。

在 `computer` 文件夹里，用 `spec.go` 的名字保存这个程序。

```golang
package computer

type Spec structs {
  Maker string
  model string
  Price int
}
```

上面的代码段里创造了一个包 `computer` 他包含导出结构体类型， `Spec` 包含两个导出字段 `Maker` 和 `Price` 和一个非导出字段 `model`。让我们从主包中导入这个包并使用 `Spec` 结构体。

在 `structs` 文件夹中创建一个文件命名为 `main.go` 的文件并把下面的程序写入 `main.go`

```golang
package main

import "strcuts/computer"
import "fmt"

func main()  {
  var spec computer.spec
  spec.Maker = "apple"
  spec.Price = 50000
  fmt.Println("Spec:", spec)
}
```

这个包结构应该像下面那样：

```
src  
   structs
          computer
                  spec.go
          main.go
```

在上面程序的第三行，我们导入 `computer` 包。在第 8 和 9 行，我们访问 `Spec` 的两个导出字段 `Marker` 和 `Price`。这个程序可以被运行依靠命令 `go install structs` 在 `workspacepath/bin/strcuts`下。

这个程序将会输出 `Spec: {apple 50000}`。

如果我们试图访问非导出字段 `model`，编译器会报错。用下面的代码替换 `main.go` 中的内容。

```golang
package main

import "struct/computer"
import "fmt"

func main()  {
  var spec computer.Spec
  spec.Marker = "apple"
  spec.Price = 50000
  spec.model = "Mac Mini"
  fmt.Println("Spec:", spec)
}
```

在上面程序的第 10 行，我们试图访问非导出字段 `model`。运行这段程序将会导致编译错误 **spec.model undefined (cannot refer to unexported field or method model)**。

## 结构体相等性

如果每个字段都是可比的，那结构体就是值类型并且可比较。如果两个结构体变量对应的字段是相等的，那这他们俩就被认为是相等的。

```golang
package main

import (
  "fmt"
)

type name struct {
  firstName string
  lastName string
}


func main() {
  name1 := name{"Steven", "Jobs"}
  name2 := name{"Steven", "Jobs"}
  if name1 == name2 {
    fmt.Println("name1 and name2 are equal")
  } else {
    fmt.Println("name1 and name2 are not equal")
  }

  name3 := name{firstName:"Steven", lastName:"Jobs"}
  name4 := name{}
  name4.firstName = "Steven"
  if name3 == name4 {
    fmt.Println("name3 and name4 are equal")
  } else {
    fmt.Println("name3 and name4 are not equal")
  }
}
```

在上面程序中，`name` 结构体类型包含两个字段。因为字符串是可比较类型，比较两个 `name` 类型就是可行的。

在上面程序中， `name1` 和 `name2` 是相等的，然而 `name3` 和 `name4` 就不是。 这个程序会输出。

```
name1 and name2 are equal  
name3 and name4 are not equal  
```

**结构体的字段中如果有不可比较字段，那结构体就是不可比较的。** （感谢 reddit 上的 [alasijia](https://www.reddit.com/r/golang/comments/6cht1j/a_complete_guide_to_structs_in_go/dhvf7hd/)指出了这个问题）。

```golang
package main

import (
  "fmt"
)

type image struct {
  data map[int]int
}

func mian()  {
  image1 := image{data: map[int]int{
    0: 155,
  }}
  image2 := image{data: map[int]int{
    0: 155,
  }}
  if image1 == image2 {
    fmt.Println("image1 and image2 are equal")
  }
}
```

上面的程序中 `image` 结构体的字段中的 `data` 字段是 `map` 类型。`map` 类型是不可比较的，因此 `image1` 和 `image2` 是不可比较的。如果你运行这段程序，编译器将会报错 **main.go:18: invalid operation: image1 == image2 (struct containing map[int]int cannot be compared).**

这期教程的和源代码在 [github](https://github.com/golangbot/structs) 上。

Go 的结构体教程就到这里，祝好。
