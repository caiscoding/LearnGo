# 2. 变量

### 变量的命名

<mark style="color:blue;">**变量名**</mark> 必须以一个 <mark style="color:blue;">**字母或下划线开头**</mark> ，后面可以跟任意数量的字母、数字或下划线，在 Go 语言中，变量名区分大小写字母。当然，上述的命名规则在命名 <mark style="color:blue;">**函数名**</mark> 、 <mark style="color:blue;">**常量名**</mark> 、 <mark style="color:blue;">**类型名**</mark> 、 <mark style="color:blue;">**语句标号**</mark> 和 <mark style="color:blue;">**包名**</mark> 等都适用。

特别注意的是，在 Go 中有 `25` 个关键字不能用于定义名字，它们分别是：

| 关键字         | 作用                                                |
| ----------- | ------------------------------------------------- |
| break       | 用于跳出循环                                            |
| default     | 用于选择结构的默认选项(switch、select)                        |
| func        | 定义函数                                              |
| interface   | 定义接口                                              |
| select      | Go语言特有的channel选择结构                                |
| case        | 选择结构标签                                            |
| defer       | 延迟执行内容(在函数结尾的时候执行)                                |
| go          | 并发执行                                              |
| map         | 定义map类型                                           |
| struct      | 定义结构体                                             |
| chan        | 定义channel                                         |
| else        | 选择结构                                              |
| goto        | 跳转语句                                              |
| package     | 包                                                 |
| switch      | 选择结构                                              |
| const       | 定义常量                                              |
| fallthrough | 如果case带有fallthrough，程序会继续执行下一条case，不会再判断下一条case的值 |
| if          | 选择结构                                              |
| range       | 从slice、map等结构中取元素                                 |
| type        | 定义类型                                              |
| continue    | 跳过本次循环                                            |
| for         | 循环结构                                              |
| import      | 导入包                                               |
| return      | 返回                                                |
| var         | 定义变量                                              |

当然，除了上面所说的 `25` 个关键字之外，还有大约 `30` 多个预定义的名字，具体如下。

```
内建常量:
    true false iota nil

内建类型: 
    int int8 int16 int32 int64
    uint uint8 uint16 uint32 uint64 uintptr
    float32 float64 complex128 complex64
    bool byte rune string error

内建函数:
    make len cap new append copy close delete
    complex real imag
    panic recover
```

这些内部预先定义的名字并不是关键字，你可以在定义中重新使用它们，但在普通情况下并 <mark style="color:red;">**不推荐**</mark> 这么做，免得引起语义混乱。在习惯上，Go语言程序员推荐使用 <mark style="color:blue;">**驼峰式**</mark> 命名。

### 变量的声明

Go 语言主要有四种类型的声明语句： <mark style="color:blue;">**var(声明变量)**</mark> 、 <mark style="color:blue;">**const(声明常量)**</mark> 、 <mark style="color:blue;">**type(声明类型)**</mark> 和 <mark style="color:blue;"></mark><mark style="color:blue;">**func(声明函数)**</mark> <mark style="color:blue;"></mark>。

接下来我们就详细讲一讲几种声明变量的方法。

#### <mark style="color:orange;">第一种声明方法 ：一行一个变量</mark>

```go
var <name> <type>
```

其中 `var` 是关键字， `name` 是变量名， `type` 是类型。

当然，你也可以在声明时给定该变量的初始值：

```go
var <name> <type> = <expression>
```

如果你在声明变量的时候只指定了其类型， Go 会自动给你的变量初始化为默认值。例如 `string` 类型会初始化为空字符串 `""` ， `int` 类型会初始化为 `0` ， `float` 会初始化为 `0.0` ， `bool` 类型会初始化为 `false` ， `接口` 和 `引用类型` 和 `指针类型` 就初始化为 `nil` ， `数组` 或 `结构体` 等聚合类型对应的默认值是每个元素或字段都是对应该类型的默认值等。

你在已经给定变量初始值的情况下，可以将类型 `type` 部分省略， Go 将根据初始化表达式 `expression` 来推导变量的类型：

```go
var <name> = <expression>
```

下面演示了一行声明一个变量的例子：

```go
package main

import (
	"fmt"
)

func main() {
	// var <name> <type>
	var name string
	fmt.Println("name = ", name)

	// var <name> <type> = <expression>
	var pi float64 = 3.14
	fmt.Println("pi = ", pi)

	// var <name> = <expression>
	var phone = "123456"
	fmt.Println("phone = ", phone)
}
```

运行该程序输出为：

```
name =  
pi =  3.14
phone =  123456
```

#### <mark style="color:orange;">第二种声明方法 ：一组变量一起声明</mark>

```go
var (
    <name> <type>
    <name> <type>
    ...
)
```

上面的例子每个变量都要写一行声明，为了简洁，我们可以修改成一组变量一起声明的形式：

```go
package main

import (
	"fmt"
)

func main() {

	// var (
	//	 <name> <type>
	//	 <name> <type>
	//	 ...
	// )
	var (
		name string
		pi float64 = 3.14
		phone = "123456"
	)

	fmt.Println("name = ", name)
	fmt.Println("pi = ", pi)
	fmt.Println("phone = ", phone)
}
```

当然，运行该程序会产生同样的输出。

#### <mark style="color:orange;">第三种声明方法 ：短声明，只能在函数内</mark>

在函数内部，有一种称为简短变量声明语句的形式可用于声明和初始化局部变量，变量的类型根据表达式来自动推导。

```go
<name> := <expression>
```

例如，下面的三条等价的语句：

```go
phone := "123456"
var phone string = "123456"
var phone = "123456"
```

但要特别注意，短声明 **只能** 用在函数内部，在包级别的声明不能使用短声明，要使用关键字 `var` 进行声明。

#### <mark style="color:orange;">第四种声明方法 ：一行声明和初始化多个变量</mark>

```go
var <name1>, <name2> <type> = <expression1>, <expression2>
```

下面是一行声明和初始化多个变量的例子：

```go
var i, j, k int                     // int, int, int
var ok, number = true, 1.2          // bool, float64
phone, city := "123456", "Beijing"  // string, string
```

这种方法经常用于变量之间的交换：

```go
var a int = 1
var b int = 2
b, a = a, b
```

#### <mark style="color:orange;">第五种声明方法 ：通过 new 创建指针变量</mark>

一般变量分为两种，上面说过的那些存放数据本身的 <mark style="color:blue;">**普通变量**</mark> <mark style="color:blue;"></mark>和存放数据地址的 <mark style="color:blue;">**指针变量**</mark> 。

例如，下面的例子，用 `var x int` 声明语句声明的是一个 x 普通变量，那么 `&x` 表达式所代表的就是一个指向 x 普通变量的指针变量，即 `&x` 为存放 x 数据的地址，其对应的数据类型为 `*int` 。而 `*p` 表达式所代表的是对应 p 指针指向的变量的值，即 x 的值。

```go
package main

import (
	"fmt"
)

func main() {
	x := 1
	p := &x					// p, of type *int, points to x
	fmt.Println("p = ", p)	// "0xc0000aa058"
	fmt.Println("*p = ", *p)// "1"
	*p = 2					// equivalent to x = 2
	fmt.Println("x = ", x)	// "2"
}
```

运行该程序会输出下面的类似结果，其中第一行输出的是存放普通变量 `x` 的地址，该值不固定：

```
p =  0xc0000aa058
*p =  1
x =  2
```

而这里讲的 `new` 函数是 Go 里的一个内建函数。

使用表达式 `new(Type)` 将创建一个 `Type` 类型的匿名变量，初始化为 `Type` 类型的零值，然后返回变量地址，返回的指针类型为 `*Type` 。

```go
package main

import (
	"fmt"
)

func main() {
	p := new(int)			// p, *int 类型, 指向匿名的 int 变量
	fmt.Println("*p = ", *p)// 匿名的 int 变量零值为 "0"
	*p = 2					// 设置 int 匿名变量的值为 2
	fmt.Println("*p = ", *p)// "2"
}
```

该程序输出如下：

```
*p =  0
*p =  2
```

用 `new` 创建变量除了不需要声明一个临时变量的名字外，和普通变量声明语句方式创建变量没有什么区别。

#### <mark style="color:orange;">第六种 ：make 函数创建 slice、map 或 chan 类型变量</mark>

在 Go 语言中可以使用 `make` 函数创建 slice、map 或 chan 类型变量：

```go
var mySlice = make([]int, 8)
var myMap = make(map[string]int)
var myChan = make(chan int)
```

slice、map 和 chan 是 Go 中的引用类型，它们的创建和初始化，一般使用 make。特别的， <mark style="color:red;">**chan 只能用 make**</mark> 。slice 和 map 还可以简单的方式：

```go
mySlice := []int{0, 0}
myMap := map[string]int{}
```

#### <mark style="color:orange;">特别注意</mark>

变量或者常量都只能声明一次，特别注意短声明，例如下面的示例， `a` 和 `b` 都已经使用短声明了，再使用一次短声明就是错误的。但是，如果短声明中仅有一些变量在相同的词法域声明过了，那么短变量声明语句对这些已经声明过的变量就只有赋值行为。

```go
a, b := 1, 2
...
a, b := 1, 3	// error
a, b = 1, 3		// ok
a, c := 1, 3	// ok
```

当然， <mark style="color:blue;">**匿名变量**</mark> （也称作占位符，或者空白标识符，用下划线表示）可以声明多次。匿名变量有三个优点：

1. 不分配内存，不占用内存空间
2. 不需要你为命名无用的变量名而纠结
3. 多次声明不会有任何问题

通常我们用匿名接收 <mark style="color:blue;">**必须接收，但是又不会用到的值**</mark> ，例如：

```go
// array of 3 integers
var a [3]int

// Print the elements only.
for _, v := range a {
    fmt.Printf("%d\n", v)
}
```

### 变量的生命周期

变量的生命周期指的是在程序运行期间变量有效存在的时间段。对于在包一级声明的变量来说，它们的生命周期和整个程序的运行周期是一致的。而相比之下，局部变量的生命周期则是从创建的声明语句开始，直到该变量不再被引用为止，然后变量的存储空间可能被回收。函数的参数变量和返回值变量都是局部变量。它们在函数每次被调用的时候创建。

一个循环迭代内部的局部变量的生命周期可能超出其局部作用域。同时，局部变量可能在函数返回之后依然存在。

编译器会自动选择在栈上还是在堆上分配局部变量的存储空间，但可能令人惊讶的是，这个选择并不是由用 `var` 还是 `new` 声明变量的方式决定的。
