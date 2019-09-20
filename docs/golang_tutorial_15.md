
15 - 指针  
========================

上一节：[第十四篇 字符串](/docs/golang_tutorial_10.md)   
下一节：[第十六篇 结构体](/docs/golang_tutorial_12.md)  

这是本Golang系列教程的第15篇。  

## 什么是指针？

指针是存储一个变量的内存地址的变量。  

![pointer-explained](../images/pointer-explained.png)  

在上图中，变量 `b` 的值是 `156`，存储在地址为 `0x1040a124` 的内存中。变量 `a` 存储了变量 `b` 的地址。现在可以说 `a` 指向 `b`。  

## 指针的声明  

指向类型 `T` 的指针用 `*T` 表示。  

让我们写一些代码。  
```golang
package main

import (  
    "fmt"
)

func main() {  
    b := 255
    var a *int = &b
    fmt.Printf("Type of a is %T\n", a)
    fmt.Println("address of b is", a)
}
```

`&` 操作符用来获取一个变量的地址。在上面的程序中，第 9 行我们将 `b` 的地址赋给 `a`（`a` 的类型为 `*int`）。现在我们说 `a` 指向了 `b`。当我们打印 `a` 的值时，`b` 的地址将会被打印出来。程序的输出为：  
```
Type of a is *int  
address of b is 0x1040a124  
```

你可能得到的是一个不同的 `b` 的地址，因为 `b` 可以在内存中的任何地方。  

## 指针的空值  

指针的空值为 `nil` 。  

```golang
personSalary := make(map[string]int)
```

上面的代码创建了一个名为 `personSalary` 的 `map`。其中键的类型为 `string`，值的类型为 `int`。  

`**map` 的 `0` 值为 `nil`。试图给一个 `nil map` 添加元素给会导致运行时错误。因此 `map` 必须通过 `make` 来初始化（译者注：**也可以使用速记声明来创建 map**，见下文）。    

```golang
package main

import (  
    "fmt"
)

func main() {  
    var personSalary map[string]int
    if personSalary == nil {
        fmt.Println("map is nil. Going to make one.")
        personSalary = make(map[string]int)
    }
}
```

上面的程序中，`personSalary` 为 `nil`，因此使用 `make` 初始化它。程序的输出为：`map is nil. Going to make one`.   

## 用 new 函数创建指针

Go 也提供了一个易用的 `new` 函数来创建指针。这个 `new` 函数接受一个类型作为参数并返回一个该参数类型的分配过的零值的指针。

下面的例子能把事情说得更明白。

```golang
package main

import (  
    "fmt"
)

func main() {  
    size := new(int)
    fmt.Printf("Size value is %d, type is %T, address is %v\n", *size, size, size)
    *size = 85
    fmt.Println("New size value is", *size)
}
```

在前面程序的第八行，我们用 `new` 函数创造了一个 `int` 类型的指针。这个函数会返回一个新分配的 `int` 类型的零值。这个 `int` 类型的零值是 `0`。

上面的程序输出为：

```
Size value is 0, type is *int, address is 0x414020  
New size value is 85
```

## 指针取值

指针取值意思是获取指针所指的这个变量的值。 `*a` 是 `a` 取值的语法。

我们来看看这段程序怎么运作：

```golang
package main  
import (  
    "fmt"
)

func main() {  
    b := 255
    a := &b
    fmt.Println("address of b is", a)
    fmt.Println("value of b is", *a)
}
```

在上面程序的第十行，我们对指针 `a` 取值并打印他的值。和预想的一样，他打印出了 `b` 的值。这段程序的输出为：

```
address of b is 0x1040a124  
value of b is 255
```

让我们再写一个程序，在这里面我们使用指针来更改 `b` 的值。

```golang
package main

import (  
    "fmt"
)

func main() {  
    b := 255
    a := &b
    fmt.Println("address of b is", a)
    fmt.Println("value of b is", *a)
    *a++
    fmt.Println("new value of b is", b)
}
```

在上面程序的十二行，我们把 `a` 所指的变量增加了 1，也就是 `b` 的值，因为 `a` 指向 `b`。因此 `b` 的值变成了 256 。程序的输出是：

```golong
address of b is 0x1040a124  
value of b is 255  
new value of b is 256  
```

## 给函数传指针

```golang
package main

import (  
    "fmt"
)

func change(val *int) {  
    *val = 55
}
func main() {  
    a := 58
    fmt.Println("value of a before function call is",a)
    b := &a
    change(b)
    fmt.Println("value of a after function call is", a)
}
```

在上面程序的第 14 行我们把携带 `a` 的地址的指针变量 `b` 传进函数 `change`。在 `change` 函数里面，`a` 的值在第八行通过取值来改变。这个程序的输出，

```
value of a before function call is 58  
value of a after function call is 55  
```

## 函数返回指针

对于函数来说，返回局部变量的指针是完全合法的。Go 编译器足够聪明所以他会在堆中为这个变量分配空间。

```
package main

import (  
    "fmt"
)

func hello() *int {  
    i := 5
    return &i
}
func main() {  
    d := hello()
    fmt.Println("Value of d", *d)
}
```

在上面程序的第 9 行，我们从 `hello` 函数中返回了局部变量 `i`。**像 `i` 这种局部变量在所在函数返回时被用于的行为在 C 和 C++ 中是未定义行为。但是在 Go 中，编译器会进行 `逃逸分析` 并把将要逃逸的局部变量 `i` 分配到堆区。** 因此这个程序会正常执行并打印出：

```
Value of d 5
```

## 不要把数组的指针传给函数。用切片替代。

我们设想我们要对函数内的数组做一些修改并且这些修改对于他的调用者是可见。达到这种目的的一种方式就是把数组的指针作为参数传给函数。

```golang
package main

import (  
    "fmt"
)

func modify(arr *[3]int) {  
    (*arr)[0] = 90
}

func main() {  
    a := [3]int{89, 90, 91}
    modify(&a)
    fmt.Println(a)
}
```

在上面程序的第 13 行，我们把数组 `a` 的地址传给 `modify` 函数。在第 8 行， `modify` 函数中，我们对 `arr` 取值并把 `90` 赋值给数组的第一个元素。这个程序的输出为 `[90 90 91]`

**a[x] 是 (*a)[x] 的缩写。所以 (*arr)[0] 在上面程序中能被 arr[0]替换。** 让我们用缩写重写上面的程序。

```golang
package main

import (  
    "fmt"
)

func modify(arr *[3]int) {  
    arr[0] = 90
}

func main() {  
    a := [3]int{89, 90, 91}
    modify(&a)
    fmt.Println(a)
}
```

这个程序也会输出 `[90 90 91]`

**尽管这种给函数传数组指针的方式确实可行，但却不是 Go 语言中最地道的方式。为了这个我们有切片**

我们用切片来重写这个程序

```golang
package main

import (  
    "fmt"
)

func modify(sls []int) {  
    sls[0] = 90
}

func main() {  
    a := [3]int{89, 90, 91}
    modify(a[:])
    fmt.Println(a)
}
```

在上面程序的第 13 行，我们传一个切片给 `modify` 函数。这个切片的第一个元素在 `modify` 函数中被改成了 `90`。这个程序也输出了 `[90 90 91]`。 **所以忘记传数组的指针，用切片替代吧 :)。** 这段代码更简洁也是更地道的 Go :)。

## Go 不支持指针运算

Go 不支持类似 C/C++ 中的指针运算。

```golang
package main

func main() {  
    b := [...]int{109, 110, 111}
    p := &b
    p++
}
```

上面的程序将会抛出编译错误 **main.go:6: invalid operation: p++ (non-numeric type *[3]int)**

我（原作者）在 [GitHub](https://github.com/golangbot/pointers) 上创建了一个小程序包含我们讨论过的所有内容。

Go 语言的指针就到这里，祝好。
