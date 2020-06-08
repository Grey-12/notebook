[toc]



# Go Programming Language Study Notes



## Chapter 1 数据类型



### 01. 变量

#### 01.01 变量的声明

``````go
var 变量名称 类型 = 表达式	
``````

- 指定变量类型，**声明后若不赋值，使用默认值**

  ``````go
  var i int // 声明不赋值，默认为零值 int类型的零值为0
  ``````

- 根据值自行判定变量类型（**类型推导**）

  ``````go
  var num = 10.11 // 此时num为float64
  ``````

- 省略`var`，使用 **:=** 进行赋值

  ``````go
  name := "jack" // 此时name为string类型
  ``````

- 多变量声明

  ``````go
  n1, name, age := 100, "jack", 18
  var (
  	n2 = 200
  	n3 = 300
  	name2 = "Tom"
  )
  ``````



### 02. 整数类型

| 类型     | 有无符号 | 占用空间                     | 表数范围                                                     | 备注                |
| -------- | -------- | ---------------------------- | ------------------------------------------------------------ | ------------------- |
| `int`    | 有       | 32位系统4字节  64位系统8字节 | -2<sup>31</sup> ~ 2<sup>31</sup>-1   -2<sup>63</sup> ~ 2<sup>63</sup>-1 |                     |
| `int8`   | 有       | 1字节                        | -128 ~ 127                                                   |                     |
| `int16`  | 有       | 2字节                        | -2<sup>15</sup> ~ 2<sup>15</sup> -1                          |                     |
| `int32`  | 有       | 4字节                        | -2<sup>31</sup> ~ 2<sup>31</sup>-1                           |                     |
| `int64`  | 有       | 8字节                        | -2<sup>63</sup> ~ 2<sup>63</sup>-1                           |                     |
| `uint8`  | 无       | 1字节                        | 0 ~ 255                                                      |                     |
| `uint16` | 无       | 2字节                        | 0 ~ 2<sup>16</sup> -1                                        |                     |
| `uint32` | 无       | 4字节                        | 0 ~ 2<sup>32</sup> -1                                        |                     |
| `uint64` | 无       | 8字节                        | 0 ~ 2<sup>64</sup> -1                                        |                     |
| `rune`   | 有       | 4字节                        | -2<sup>31</sup> ~ 2<sup>31</sup>-1                           | 表示一个`unicode`码 |
| `byte`   | 无       | 与`uint8`等价                | 0 ~ 255                                                      | 1byte = 8bit        |

- `Golang` 的整型默认声明为`int`

- 查看变量字节大小及数据类型

  ``````go
  var n int64
  fmt.Printf("n type %T n size %d", n, unsafe.Sizeof(n)) 	
  ``````



### 03. 小数类型/浮点型

| 类型            | 占用存储空间 | 表数范围               |
| --------------- | ------------ | ---------------------- |
| 单精度`float32` | 4字节        | -3.403E38 ~ 3.403E38   |
| 双精度`float64` | 8字节        | -1.798E308 ~ 1.798E308 |

- 浮点数=符号位 + 指数位 + 尾数位（浮点数都是有符号的）

- 尾数部分可能丢失，造成精度损失

- 浮点类型有固定范围和字段长度，不受OS影响

- 浮点数默认声明为`float64`, 默认值为0

- 浮点数表达方式

  ``````go
  // 十进制数形式
  num := 5.12
  num2 := .123 // 0.123
  
  // 科学计数法形式
  num3 := 5.1234e2 // 5.1234 * 10的2次方
  num4 := 5.1234E-2 // 5.1234 / 10的2次方 0.051234
  ``````



#### 03.01 字符类型

- Go 中没有专门的字符类型，如果存储单个字符，一般使用`byte`保存
- 字符串就是一串固定长度的字符连接起来的字符序列。Go中的字符串是由单个字节连接起来的。
- 字符常量使用单引号扩起来的单个字符。 Go中使用UTF-8编码
- 字符的本质是一个整数，直接输出时对应的为UTF-8的码值
- 可以直接给某个变量赋值一个数字，然后格式化%c输出对应的`unicode`字符
- **字符可以进行运算的**

###  04. 布尔类型

- `bool`类型数据只允许有true和false
- `bool` 占 1字节
- `bool `类型适于**逻辑运算**， 一般用于程序流程控制
- `bool` 类型零值为 false

### 05. string类型

- 字符串就是一串固定长度的字符连接起来的字符序列。Go中的字符串是由单个字节连接起来的。

- 字符串赋值过后，字符串不能修改（不可变）
- 字符串表现形式: 01.双引号 02. 反引号
- 字符串拼接使用 `+`
- 字符串默认值为空字符串`""`

### 06. 基本类型转换

**Go中不同类型的变量之间赋值时需进行显示转换**

- 基本语法

  `````go
  /* 
  表达式 T(v) 将值v 转换为类型T
  T: 数据类型
  v: 需要转换的数据
  */
  
  package main
  
  import "fmt"
  
  func main() {
      var i int32 = 100
      var n1 float32 = float32(i)
      var n2 int8 = int8(i)
      var n3 int64 = int64(i)  // 全部为显示转换
  }
  `````

- 如果将`int64` 转成`int8` 编译不会报错，会按照溢出处理



### 07. 基本数据类型和string转换

#### 07.01 基本数据类型转string类型

- `fmt.Sprintf("%参数", 表达式)`

  ``````go
  func Sprintf(format string, a ...interface{}) string
  // Sprintf根据format参数生成格式化的字符串并返回该字符串
  // format参数需要和表达式的数据类型相匹配
  var num1 int = 99
  str = fmt.Sprintf("%d", num1) 
  // %d  十进制整数
  // %x, %o, %b  十六进制， 八进制， 二进制整数
  // %f, %g, %e  浮点数：不同的取值范围
  // %t    布尔类型
  // %c    字符rune 
  // %s    字符串
  // %q    带双引号的字符串或者带单引号的字符
  // %v    变量的自然形式
  // %T    变量的类型
  // %%    字面上百分号标志
  ``````

- 使用`strconv`包的函数进行转换

  ``````go
  func FormatBool(b bool) string
  func FormatFloat(f float64, fmt byte, prec, bitSize int) string
  // num float64 = 1.0
  // str = strconv.FormatFloat(num, "f", 10, 64)
  // f 格式 10 保留小数位10位 64 表示float64
  func FormatInt(i int64, base int) string
  func FormatUint(i uint64, base int) string
  func Itoa(i int) string // Itoa是FormatInt(i, 10) 的简写
  ``````



#### 07.02 string类型转基本数据类型

- 使用`strconv`包中函数

  ``````go
  func ParseBool(str string) (value bool, err error)
  func ParseFloat(s string, bitSize int) (f float64, err error)
  func ParseInt(s string, base int, bitSize int) (i int64, err error)
  func ParseUint(s string, b int, bitSize int) (n uint64, err error)
  ``````

- 将string类型转换为基本数据类型时，要确保string类型能够转成有效的数据



### 08. 指针

- 基本数据类型，变量存的数据为值， 叫值类型

- 获取变量的地址用`&`符号

  ``````go
  var num int
  ptrNum := &num // 此时ptrNum为*int类型指针
  // 指针存储的为指向值的内存地址， 指针数据本身也有自己的内存地址
  ``````

- 获取指针类型所指向的值，使用`*`

  ``````go
  var ptr *int
  fmt.Println(*ptr) // 0
  ``````

- 值类型都有对应的指针类型，形式为`*Type`

- 值类型包括：`int`, `float`,`bool`, `string`, 数组，结构体`struct`

- 引用类型： 指针， `slice`切片， `map` , 管道`channel`, 接口`interface`

- 值类型变量直接存储值， 通常存在内存栈中

- 引用类型变量存储一个地址，地址对应的空间才真正存储数据，内存通常在堆中分配，当没有任何变量引用这个地址时，该地址对应的数据空间就成为一个垃圾由GC回收



### 09. 标识符

- 变量、方法、函数等命名时使用的字符
- 英文字母大小写加数字加下划线
- 数字不可以开头
- 标识符连接不可以用空格
- 严格区分大小写
- 不能使用关键字作为标识符
- 驼峰法命名
- Go中如果变量名、函数名、首字母大写表示其他包可以访问（可导出），如果为小写则只能本包中使用



## Character 2 运算符



### 01.  算术运算符

| 运算符 | 运算       | 范例          | 结果    |
| ------ | ---------- | ------------- | ------- |
| +      | 正号       | +3            | 3       |
| -      | 负号       | -4            | -4      |
| +      | 加         | 1+1           | 2       |
| -      | 减         | 2-1           | 1       |
| *      | 乘         | 2*2           | 4       |
| /      | 除         | 5/5           | 1       |
| %      | 取模/取余  | 7%5           | 2       |
| ++     | 自增       | `i = 1` `i++` | `i=2`   |
| --     | 自减       | `i=1` `i--`   | `i=0`   |
| +      | 字符串拼接 | “he” + "llo"  | "hello" |

- % 使用特点

  ``````go
  // a % b = a -a/b*b
  // -10 % 3 = -10 -(-10)/3 * 3 = -1
  // 10 % -3 = 10 - 10/(-3) * (-3) = 1
  // -10 % -3 = -10 - (-10)/(-3) * (-3) = -1
  ``````



###  02. 关系运算符

| 运算符 | 运算     | 范例 | 结果  |
| ------ | -------- | ---- | ----- |
| ==     | 相等于   | 1==1 | true  |
| !=     | 不等于   | 1!=1 | false |
| <      | 小于     | 1<1  | false |
| >      | 大于     | 1>2  | false |
| <=     | 小于等于 | 1<=1 | true  |
| >=     | 大于等于 | 1>=1 | true  |

- 比较运算符的结果都为`bool`型



### 03. 逻辑运算符

假设A=True	B=False

| 运算符 | 描述                      | 实例           |
| ------ | ------------------------- | -------------- |
| &&     | 逻辑与 都为True为True     | A&&B = False   |
| \|\|   | 逻辑或 有一个True即为True | A\|\|B = True  |
| !      | 逻辑非 取反               | !(A&&B) = True |

- && 为**短路与**：**如果第一个条件为false**，第二个条件不会判断，结果为false
- || 为**短路或**：**如果第一个条件为true**,第二个条件不会判断，结果为true



### 04. 赋值运算符

| 运算符 | 描述                                   | 实例                        |
| ------ | -------------------------------------- | --------------------------- |
| =      | 简单的赋值运算符，将表达式的值赋给左边 | c = a +b 将a+b的结果赋值给c |
| +=     | 相加后再赋值                           | c += a ==> c = c + a        |
| -=     | 相减后再赋值                           | c -= a ==> c = c - a        |
| *=     | 相乘后再赋值                           | c *= a ==> c = c * a        |
| /=     | 相除后再赋值                           | c /= a ==> c = c / a        |
| %=     | 求余后再赋值                           | c % a ==> c = c % a         |
|        |                                        |                             |
| <<=    | 左移后再赋值                           | c <<= 2 ==> c = c << 2      |
| \>\>=  | 右移后再赋值                           | c >>= 2 ==> c = c >> 2      |
| &=     | 按位与后再赋值                         | c &= 2 ==> c = c & 2        |
| ^=     | 按位异或后再赋值                       | c ^= 2 ==> c = c ^ 2        |
| \|=    | 按位或后再赋值                         | c \|= 2 ==> c = c \| 2      |

- 经典题： 数值交换

  ``````go
  var a int = 10
  var b int = 20
  // 第一种方法
  a = a + b
  b = a - b
  a = a - b
  // 第二种方法
  tmp := a
  a = b
  b = tmp
  // 第三种方法
  a, b = b, a
  ``````

  

### 05. 位运算符

| 运算符 | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| &      | 按位与运算符`&`是双目运算符 其功能是参与运算的两数各对应的二进位相与。   运算规则是同时为1则为1，否则为0 |
| \|     | 按位或运算符`|`是双目运算符 其功能是参与运算的两数各对应的二进位相或。   运算规则是当二进制位不同时，结果为1否则为0 |
| ^      | 按位异或运算符`^`是双目运算符 其功能是参与运算的两数各对应的二进位相异或。计算规则是当二进位不同时结果为1否则为0 |
| <<     | 左移运算符`<<`是双目运算符 其功能把`<<`左边的运算数的个二进位全部左移若干位，高位丢弃，低位补0， 左移n位就是乘以2的n次方 |
| \>\>   | 右移运算符`>>`是双目运算符 其功能把`>>`左边的运算数的个二进位全部右移若干位， 左移n位就是除以2的n次方 |

### 06. 其他运算符

| 运算符 | 描述             |
| ------ | ---------------- |
| &      | 返回变量存储地址 |
| *      | 指针变量         |

### 07. 运算符优先级

| 分类       | 描述                                         | 关联性     |
| ---------- | -------------------------------------------- | ---------- |
| 后缀       | () [] -> . ++ --                             | 左到右     |
| 单目       | + - ~ ! (type) * & `sizeof`                  | **右到左** |
| 乘法       | * / %                                        | 左到右     |
| 加法       | + -                                          | 左到右     |
| 移位       | <<  >>                                       | 左到右     |
| 关系       | <、 <=、 >、 >=                              | 左到右     |
| 相等       | == !=                                        | 左到右     |
| 按位AND    | &                                            | 左到右     |
| 按位XOR    | ^                                            | 左到右     |
| 按位OR     | \|                                           | 左到右     |
| 逻辑AND    | &&                                           | 左到右     |
| 逻辑OR     | \|\|                                         | 左到右     |
| 赋值运算符 | =、+=、-=、*=、/=、%=、>>=、<<=、&=、^=、\|= | **右到左** |
| 逗号       | ,                                            | 左到右     |

- 以上优先级从高到低

### 08. 键盘输入语句

- 使用`fmt`包中的`fmt.Scanln()` 或`fmt.Scanf()`

  ``````go
  func Scanln(a ...interface{}) (n int, err error)
  // Scanln 类似Scan，但会在换行时才停止扫描
  func Scanf(format string, a ...interface{}) (n int, err error)
  // Scanf 从标准输入扫描文本，根据format参数指定的格式将成功读取的空白分隔的值传递给本函数的参数
  ``````

- 使用

  ``````go
  var name string
  fmt.Scanln(&name)
  
  var age int
  fmt.Scanf("%d", &age)
  ``````

### 09. 进制

- 二进制： go中不能直接使用二进制表示一个整数
- 十进制
- 八进制
- 十六进制： 0-9 A-F

#### 09.01 其他进制转换十进制

- 二进制转十进制

  ``````reStructuredText
  1011 = 1*1+1*2+0*2*2+1*2*2*2 = 1 + 2 + 0 + 8 = 11
  // 从低位开始，每位乘以2的（位数-1）次方
  ``````

- 八进制转十进制

  ``````
  0123 = 3 * 1 + 2 * 8 + 1 * 8 * 8 = 3 + 16 + 64 = 83
  // 从低位开始，每位乘以8的（位数-1）次方
  ``````

- 十六进制转十进制

  ```````
  0x34A = 10 * 1 + 4 *16 + 3 * 16 * 16 = 10 + 64 + 768 = 842
  // 从低位开始，每位乘以16的（位数-1）次方
  ```````

#### 09.02 十进制转其他进制

- 十进制转二进制

  ``````
  将该数不断除以2，直到商为0，然后将每部得到的余数倒过来
  ``````

- 十进制转八进制

  ``````
  将该数不断除以8，直到商为0，然后将每部得到的余数倒过来
  ``````

- 十进制转十六进制

  ``````
  将该数不断除以16，直到商为0，然后将每部得到的余数倒过来
  ``````

#### 09.03 二进制转八进制、十六进制

- 二进制转八进制

  ``````
  将二进制数每三个一组（从低位开始组合）， 转成对应的八进制数即可
  11010101 = 0325
  ``````

- 二进制转十六进制

  ``````
  将二进制数每四个一组（从低位开始组合）， 转成对应的十六进制数即可
  11010101 = 0xD5
  ``````

#### 09.04 八进制、十六进制转二进制

- 八进制转二进制

  ``````
  将八进制数每一位转成对应的三位的二进制数即可
  0237 = 10011111
  ``````

- 十六进制转二进制

  ``````
  将十六进制的每一位数转成对应的四位二进制数即可
  0x237 = 1000110111
  ``````

#### 09.05 位运算

##### 09.05.01 原码、补码、反码

- 对于有符号，二进制的最高位表符号位：0表正数，1表负数
- 正数的原码、反码、补码都一样
- 负数的反码为： 原码的符号位不变，其他位取反
- 负数的补码为： 反码每位+1
- 0的原码、反码、补码都为0
- 计算机中运算都是以补码的方式进行运算

##### 09.05.02 位运算符和移位运算符

- 按位与 `&` :  全为1结果为1否则为0

- 按位或 `|` : 有一个为1结果为1否则为0

- 按位异或 `^` : 一个为0一个为1结果为1否则为0

- 右移位运算符 `>>` : 低位溢出，符号不变，用符号位补溢出的高位

- 左移位运算符 `<<` : 符号位不变，低位补0

  `````` go
  a := 1 >> 2 // 0000 0001 => 0000 0000 = 0
  c := 1 << 2 // 0000 0001 => 0000 0100 = 4
  ``````



## Character 3 程序流程



### 01. 顺序流程

``````go
func main() {
	var num1 int = 10
    var num2 int = num1 + 10
    fmt.Println(num2)
}
``````



### 02. 分支流程控制

- 单分支

  ``````go
  /* 
  if 条件表达式 {
  	执行代码块
  }
  */
  func main() {
      var age int = 20
      if age > 18 {
          fmt.Println("已成年")
      }
      // 方式二
      /*
      if age := 20; age > 18 {
      	fmt.Println("已成年")
      }
      */
  }
  ``````

- 双分支控制

  ``````go
  /* 
  if 条件表达式 {
  	代码块
  } else {
  	代码块
  }
  */
  
  func main() {
      var age int = 20
      if age > 18 {
          fmt.Println("已成年")
      } else {
          fmt.Println("未成年")
      }
  }
  ``````

  

- 多分支控制

  ``````go
  /*
  if 条件表达式 {
  	代码块
  } else if 条件表达式 {
  	代码块
  } else {
  	代码块
  }
  */
  func main() {
      var score int
      score = 90
      if score == 100 {
          fmt.Println("Nice")
      } else if score > 80 && score < 90 {
          fmt.Println("Better")
      } else if score >= 60 && score <= 80 {
          fmt.Println("Good")
      } else {
          fmt.Println("Bad")
      }
  }
  ``````




### 03. Switch 分支控制

- switch 语句用于基于不同条件执行不同动作，每个case分支都是唯一的，从上到下逐一测试，直到匹配为止

- 匹配项后面不需要再加**break**

  ``````go
  /*
  switch 表达式 {
  
  case 表达式1， 表达式2 :
  	代码块
  case 表达式3， 表达式4 :
  	代码块
  ......
  default:
  	代码块
  }
  */
  var key byte = 'a'
  
  switch key {
      case 'a':
      fmt.Println("this is a")
      case 'b':
      fmt.Println("this is b")
      default:
      fmt.Println("Nothing")
  }
  ``````

- case/switch 后面是一个表达式（常量、变量、有返回值的函数）

- case后的各个表达式的值数据类型必须和switch的表达式数据类型保持一致

- case后面可以有多个表达式，使用逗号间隔

- switch穿透 `fallthrough`，如果在case语句块后添加`fallthrough`，则会继续执行下一个case

- Type Switch: switch语句可以被用于type-switch 来判断某个interface变量中实际指向的变量类型

  ``````go
  var x interface{}
  var y = 10.0
  x = y
  switch i:=x.(type) {
      case nil:
      fmt.Printf("x的类型是:%T\n", i)
      case int:
      fmt.Printf("x是int\n")
      case float64:
      fmt.Printf("x是float64\n")
      case func(int) float64:
      fmt.Printf("x是func(int)\n")
      case bool, string:
      fmt.Printf("x是bool或string\n")
      default:
      fmt.Printf("Don't know\n")
                 
  }
  ``````

- switch/if比较

  - 如果判断的**具体数值不多**， 而且符合整数、浮点型、字符、字符串等几种类型，建议使用switch
  - 对区间判断和结果为`bool`类型的判断，使用if，if的使用范围更广



### 04. for循环控制

``````go
/*
for 循环变量初始化; 循环条件; 循环变量迭代 {
	循环操作代码块
}
*/
for i:=1; i < 10; i++ {
    fmt.Println("hello")
}
/*
for 循环判断条件 {
	循环语句
}
*/
j := 1
for j < 10 {
    fmt.Println("world")
    j++
}
/*
for {
	循环语句
}
==> while True
==> for ; ; {}
*/
k := 1
for {
    if k < 10 {
        fmt.Println("TT")
    } else {
        break
    }
    k++
}
``````



- `for-range` 遍历数组字符串

  ``````go
  var str string = "hello, world"
  for i:0; i < len(str); i++ {
      fmt.Printf("%c\n", str[i])
  }
  for index, value := range str {
      fmt.Printf("%c\n", value)
  }
  ``````

- 使用第一种方式遍历`unicode`字符会出现乱码，解决方案是将`string`转换成[]rune切片

  ``````go
  var str string = "出现乱码"
  str2 := []rune(str) // str转换成[]rune切片
  for i:=0; i <len(str2); i++ {
      fmt.Printf("%c \n", str2[i])
  }
  
  ``````

- Go中实现while 和do...while

  ``````go
  // go中没有while和do...while语法，可以通过for循环模拟实现
  // while的实现
  /*
  for {
  	if 循环条件表达式 {
  		break
  	}
  	循环操作
  	循环变量迭代
  }
  */
  var i int = 0
  for {
      if i > 10 {
          break
      }
      fmt.Println("hello")
      i++
  }
  // ------------------------------------------------------------
  // do...while 的实现
  /*
  for {
  	循环操作
  	循环变量迭代
  	if 循环条件表达式 {
  		break
  	}
  }
  代码至少执行一次
  */
  var j int = 1
  for {
      fmt.Println("OK")
      j++
      if j > 10 {
          break
      }
  }
  ``````

  ``````go
  var num int = 9
  for i:=1; i <= num; i++ {
      for j:=1; j <= i; j++ {
          fmt.Printf("%v * %v = %v\t", j, i, j*i)
      }
      fmt.Println()
  }
  ``````

- break语句 跳出循环

  - 用于终止某个语句块的执行

  - break只能跳出当前循环

  - 可以通过`lable`指定跳出循环

    ``````go
    lable2:
    for i:=0;i < 4; i++ {
        for j:=0; j < 10; j++ {
            if j == 2{
                break lable2
            }
            fmt,Println("j=", j)
        }
    }
    ``````

- continue 跳转控制语句

  - continue语句用于结束本次循环，继续执行下一次循环

  - continue可以通过`lable`结束指定循环

    ``````go
  // lable2:
    for i:=0;i < 4; i++ {
      for j:=0; j < 10; j++ {
            if j == 2{
                continue
            }
            fmt,Println("j=", j)
        }
    }
    ``````
  
- `goto` 跳转控制语句
  
  - Go语言中的`goto`可以无条件跳转程序指定位置
  - `goto`语句通常与条件语句配合使用
  - 一般不主张使用`goto`，避免造成程序混乱
  ``````go
  var n int = 30
  fmt.Println("OK1")
  if n > 20 {
      goto lable1
  }
  fmt.Println("OK2")
  fmt.Println("OK3")
  lable1
  fmt.Println("OK4")
  fmt.Println("OK5")
  fmt.Println("OK6")
  ``````
  
- return 跳转控制语句

  - return 使用在方法或者函数中，表示跳出所在的方法或者函数
  
    ``````go
    func test() {
        var j int = 1
        for {
            fmt.Println("OK")
            j++
            if j > 10 {
                return
            }
        }
    }
    ``````
  
  - return 如果在普通函数，表示跳出该函数，后面的代码不再执行
  
  - return在main函数，表示终止程序
  
## Character 04 函数、包和错误处理



### 01. 函数

- 函数的声明

  ``````go
  /*
  func name(parameter-list) (result-list) {
      code body
      return result-list
  }
  */
  func cal(n1 float64, n2 float64, operator byte) float64 {
      var res float64
      switch operator {
          case '+':
          res = n1 + n2
          case '-':
          res = n1 - n2
          case '*':
          res = n1 * n2
          case '/':
          res = n1 / n2
          default:
          fmt.Println("Operator error")
      }
      return res
  }
  ``````



### 02. 包

- 实际开发中不同文件放在不同包中，方便管理
- 包的实质就是创建不同的文件夹
- go中每一个文件都属于一个包，go是以包的形式管理文件和项目结构
- 包可以区分相同函数、变量等标识符
- 当程序文件很多时，可以很好的管理项目
- 控制函数、变量等访问范围，即作用域
- 打包语法： package 包名
- 引入包： import "包路径"

 ### 03. 函数的递归

- 函数在函数体内可以调用自身的行为

  ``````go
  func test(n int) {
      if n > 2{
          n--
          test(n)
      }
      fmt.Println("n=", n)
  }
  ``````

- 执行一个函数时，就创建一个新的独立空间（新的函数栈）

- 函数的局部变量是独立的，不会相互影响

- 递归必须想退出递归的条件逼近，否则就是无限递归

- 当一个函数执行完毕或者return就会返回，遵守谁调用返回给谁

  ``````go
// 斐波那契数
func fbn(n int) int {
    if ( n==1 || n == 2) {
        return 1
    } else {
        return fbn(n - 1) + fbn(n - 2)
    }
}
  ``````

- Go函数不支持重载

- Go中，函数也是一种**数据类型**，可以赋值给一个变量

- 函数可以作为形参，并且调用使用

- 可以使用type简化函数类型的使用

  ``````go
  /*
  type 自定义类型名 数据类型
  */
  type myIne int
  type mySum func(int, int) int
  ``````

- Go支持可变参数

  ``````go
  /*
  func sum(args... int){}
  func sum(n1 int, args... int){}
  */
  ``````

  - `args`是slice切片，通过`args[index]`可以访问各个值
  - 如果函数的形参列表有可变参数，则可变参数需要放在形参列表最后

### 03. `init`函数

- 每一个源文件都可以包含一个`init`函数，该函数会在main函数执行前被调用
- 如果一个文件同时包含全局变量定义，`init`函数和main函数，执行流程为全局变量定义->`init`函数->main函数
- `init`函数最主要的作用就是完成一些初始化的工作

  

### 04. 匿名函数

- Go中支持匿名函数，如果函数只希望使用一次则可以使用匿名函数

  ``````go
  func main() {
      res1 := func (n1 int, n2 int) int {
          return n1 + n2
      }(10, 20)
  }
  ``````

- 将匿名函数给一个变量，通过变量来调用函数

  ``````go
  func main() {
      a := func (n1, n2 int) int {
          return n1 - n2
      }
      res2 := a(10, 20)
  }
  ``````

- 全局匿名函数：如果将匿名函数赋值给一个全局变量，这个函数就是全局匿名函数



### 05. 闭包

- 闭包就是一个函数和与其相关的引用环境组合的一个整体

  ``````go
  func AddUpper() func(int) int {
      var n int = 10
      return func (x int) int {
          n = n + x
          return n
      }
  }
  
  func main() {
      f := AddUpper()
      f(1) // 11
      f(2) // 13
      f(3) // 16
  }
  ``````




### 06. defer函数

- 为了在**函数执行完毕后，及时的释放资源**，Go中提供了defer函数（延时机制）

- 当程序执行到defer时，不会立即执行defer后的语句，而是将defer后的语句压入到一个栈中，然后继续执行函数下一个语句

- 当函数执行完毕后，从栈顶依次去除语句执行（先入后出）

  ``````go
  func test() {
      file = openfile(filename)
      defer file.close()
      ...
  }
  ``````



### 07. 函数参数

- 值传递： 值类型默认是值传递，变量直接存储值
- 引用传递：引用类型默认是引用传递，变量存储一个地址，地址对应的空间才是真正的值，当没有任何变量引用这个地址时，该地址对应的数据空间就成为一个垃圾，由GC来回收
- 如果让值类型变为引用传递可使用指针



### 08. 字符串的常用函数

- 统计字符串长度`len(str)`
- 字符串遍历`r:=[]rune(str)`
- 字符串转`[]byte`, `var bytes = []byte(str)`
- 10进制转2， 8， 16禁止`str = strconv.FormatInt(123, 2) // 2为进制可为8，16`
- 查找字符串 `strings.Contains("seafood", "foo") // true`
- 统计字符串制定字符出现次数 `strings.Count("chesese", "e") // 3`
- 不区分大小写比较字符串 `strings.EqualFold("abc", "ABC") // true`
- 返回字符串中特定字符第一次出现的index，如果没有返回-1 `strings.Index("net_abc", "abc") // 4`
- 返回字符串中特定字符最后一次出现的index，如果没有返回-1 `strings.LastIndex("go golang", "go") // 3`
- 替换字符串指定的字符 `strings.Replace("go go hello", "go~", n) // n指定替换几个，如果n=-1，表示全部替换`
- 按照制定字符切分成一个字符串数组 `strings.Split("hello,world,ok", ",")`
- 字符串大小写转换 `strings.ToLower("Go")// go ` `strings.ToUpper("go") // GO`
- 将字符串左右两边的空格去掉 `strings.TrimSpace("  tn  ") // tn`
- 将字符串左右两边指定的字符去掉 `strings.Trim("!hello!", "!") // hello`
- 将字符串左边指定的字符去掉 `strings.TrimLeft("!hello!", "!") // hello!`
- 将字符串右边指定的字符去掉 `strings.TrimRight("!hello!", "!") //!hello`
- 判断字符串是否以指定的字符串开头 `strings.HasPrefix("ftp://192.168.0.1", "ftp") // true`
- 判断字符串是否以指定的字符串结尾 `strings.HasSuffix("NTL_abc.ini", "ini") // true`



### 09. 时间和日期函数

- 使用的包 `time`

- `time.Time` 类型 用于表示时间

  ``````go
  func main() {
      //获取当前时间
      now := time.Now()
  }
  ``````

- 获取日期其他信息

  ``````go
  func main() {
      now := time.Now()
      now.Year() // 年
      now.Month() // 月
      now.Day() // 日
      now.Hour() // 小时
      now.Minute() // 分钟
      now.Second() // 秒
  }
  ``````

- 格式化时间，使用`fmt.Sprintf()`

  ``````go
  dateStr := fmt.Sprintf("%d-%d-%d %d:%d:%d", now.Year(), now.Month(),
                        now.Day(), now.Hour(), now.Minute(), now.Second(),
                        )
  ``````

- 格式化时间 `time.Format()`

  ``````go
  now.Format("2006-01-02 15:04:05")
  now.Format("2006-01-02")
  now.Format("15:04:05")
  // 2006-01-02 15:04:05 是固定字符串
  ``````

- 时间常量

  ``````go
  const(
  	Nanosecond	Duration = 1 //纳秒
      Microsecond 	= 1000 * Nanosecond
      Millisecond		= 1000 * Microsecond
      Second			= 1000 * Millisecond
      Minute			= 60 * Second
      Hour			= 60 * Minute
  )
  time.Sleep(time.Millisecond * 100) // sleep 0.1秒
  ``````

- `time` 的Unix和UnixNano

  ``````go
  func (t time) Unix() int64
  // Unix表示将t表示为Unix时间，即从时间点January 1， 1970 UTC到时间点t所进过的时间 单位秒
  
  func (t time) UnixNano() int64
  // UnixNano 表示将t表示为Unix时间，即从时间点January 1， 1970 UTC到时间点t所进过的时间 单位纳秒 如果单位时间超过了int64能表示的范围，结果是未定义
  ``````



### 10. 内置函数

- `len` : 用来求长度 比如string，array， slice，map， channel

- `new`: 用来分配内存，主要用来分配值类型的指针

  ``````go
  func main() {
      num1 := 100
      num2 := new(num1) // *int
  }
  ``````

- `make`: 用来分配内存，主要用来分配引用类型，比如map，channel，slice



### 11. 错误处理

- panic错误

  ``````go
  func test() {
      num1 := 0
      num2 := 100
      rest := num2 / num1
      fmt.Println("res=", res)
  }
  
  func main() {
      test()
  }
  ``````

- 上述代码会发生panic，程序会崩溃

- Go中不支持`try...catch...finally`

- Go中引入的处理方式为： defer，panic，recover

- Go中抛出一个panic异常，在defer中通过recover捕获异常

  ``````go
  func test() {
      defer func() {
          err := recover()
          if err != nil {
              fmt.Println("err=", err)
          }
      }()
      num1 := 0
      num2 := 100
      rest := num2 / num1
      fmt.Println("res=", res)
  }
  func main() {
      test()
  }
  ``````

- 错误处理程序不会轻易挂掉，使程序更加健壮

#### 11.01 自定义错误处理

- Go支持自定义错误，使用`errors.New` 和 `panic`内置函数来实现

- `errors.New("错误信息")` 返回一个error类型的值， 表示一个错误

- `panic` 内置函数，接收一个interface{} 类型的值（任何值），作为参数，接收error类型的值，输出错误信息，并退出程序

  ``````go
  func readConf(name string) (err error) {
      if name == "config.ini" {
          load...
          return nil
      } else {
          return errors.New("read file error")
      }
  }
  
  func test() {
      err := readConf("config.txt")
      if err != nil {
          panic(err)
      }
      fmt.Println("run continue")
  }
  ``````



## Character 05 复杂数据类型



### 01. 数组

- 数组可以存放多个同一类型数据。数组本身也是一种数据类型
- 数组是由一个固定长度的特定类型元素组成的序列，一个数据可以由零个或者多个元素组成
- 数组的长度是固定的

#### 01.01 数组的定义

- `var 数组名 [数组大小] 数据类型`
  - `var a[5] int`
  - 初始复制 `a[0] = 1 a[1] = 20`
- 数组属于值类型，获取数组的地址可以通过数组名来获取 `&intArr`
- 数组的第一个元素的地址，就是数组的首地址
- 数组的各个元素的地址间隔是一句数组的类型决定的 `int64->8`  `int32->4`

#### 01.02 数组的使用

- 访问数据的元素通过数据的下标来访问 下标从0开始

#### 01.03 初始化数组

- `var numArr1 [3]int = [3]int{1, 2, 3}`
- `var numArr2 = [3]int{5, 6, 7}`
- `var numArr3 = [...]int{8, 9, 10}`
- `var numArr4 = [...]int{1: 90, 2: 100, 3: 500}`
- `strArr := [...]string{"tom", "jack"}`

#### 01.04 数组的遍历

- 传统for循环

  ``````go
  for i:=0; i < len(Arr); i++ {
      fmt.Println(Arr[i])
  }
  ``````

- for-rang 

  ``````go
  /*
  for index, value := range array {
  
  }
  */
  
  func main() {
      intArr := [...]int{1, 2, 3, 4}
      for index, value := range intArr {
          fmt.Println(value)
      }
  }
  ``````

#### 01.05 数组注意事项

- 数组一旦声明，其长度是固定的，不能动态变化
- 数组中的元素可以是任何数据类型，包括值类型和引用类型，但只能是单一类型
- 数组创建后如果没有赋值，默认是数组数据类型对应的默认值
- 数组使用先声明数组开辟内存空间 然后给数组各个元素赋值
- 数组的下标从0开始
- 数组下标必须在指定范围内使用，否则报panic，index out range
- 数组属值类型，默认是值传递
- 如果想修改数组可以使用引用传递 数组指针
- 长度是数组类型的一部分，在传递函数参数时，需要考虑数组的长度



### 02. 切片

- 切片是数组的一个引用，因此切片是引用类型，在进行传递时，遵循引用传递的机制

- 切片的使用类似数组，遍历、范文切片的元素和求切片长度等方式都一致

- 切片的长度是可以变化的，因此切片是一个可以动态变化的数组

- 切片定义的基本语法

  ``````go
  // var 切片名 [] 类型
  var a []int
  func main() {
      var intArr [5]int = [...]int{1, 22, 3, 55, 66}
      slice01 := intArr[1:3]
      // 使用cap函数查看切片的容量
      fmt.Println("容量=", cap(slice01)) // 4
  }
  ``````

- 切片从底层来说，就是一个结构体

  ``````go
  type slice struct {
      ptr *[2]int
      len  int
      cap  int
  }
  ``````

#### 02.01 切片的使用

- 定义切片可以直接去引用一个创建好的数组

- 通过make来创建切片

  ``````go
  // var sliceNmae []type = make([]type, len, cap)
  // cap是切片的容量，可选如果设置cap则cap>=len
  var sliceFloat []float64 = make([]float64, 5, 10)
  ``````

- 定义一个切片，直接指定具体的数组

  ``````go
  var strSlice []string = []string{"tom", "jack"}
  ``````

- 通过make创建的切片会在底层维护一个数组，由切片来引用

- 切片定义完后，因为本身是一个空的，需要让其引用一个数组，或者make分配内存空间

- 切片可以继续切片

- 用append内置函数对切片进行动态追加

  ``````go
  var slice3 []int = []int{100, 200, 300}
  slice3 = append(slice3, 400, 500, 600) // 100,200,300,400,500,600
  slice3 = append(slice3, slice3...)
  // 100,200,300,400,500,600,100,200,300,400,500,600
  ``````

- 切片append操作本质上就是对数组扩容

- go底层会创建一个新的数组，将slice原来的元素拷贝到新的数组，然后slice重新引用到新的数组，数组对程序员不可见

- 切片的拷贝操作

  ``````go
  // 切片使用copy内置函数来完成拷贝
  var slice1 []int = []int{1, 2, 3, 4, 5}
  var slice2 = make([]int, 6)
  copy(slice2, slice1)
  fmt.Println(slice2) // 1, 2, 3, 4, 5, 0
  ``````

- copy(para1, para2) 参数的数据类型是切片

- 参数间的数据类型是独立的互不影响

- 如果`len(para2) > len(para1) 则只会拷贝para1长度的数据给para1`

#### 02.02 string和slice

- string底层是一个byte数组，因此string也可以进行切片处理

  ``````go
  func main() {
      str := "hello"
      strSlice := str[:]
  }
  
  ``````

- string是不可变的，所以不能通过`str[0]='z'`的方式来修改string

- 如果需要修改字符串可以通过`string ->[]byte->change string->join to string`

  ``````go
  str := "hello"
  arr1 := []byte
  arr1[0] = 'z'
  str = string(arr1) // zello
  // 如果保护unicode编码
  arr2 = []rune(str)
  arr2[0] = "呵"
  str = string(arr2) // 呵ello
  ``````

  ``````go
  // 
  func fbn(n int) []uint64 {
      fbnSlice := make([]uint64, n)
      fbnSlice[0] = 1
      fbnSlice[1] = 1
      for i:=2; i<n; i++ {
          fbnSlice[i] = fbnSlice[i-1] + fbnSlice[i-2]
      }
      return fbnSlice
  }
  ``````



### 03.排序和查找

- 排序是将一组数据，依指定顺序进行排列的过程
- 内部排序：将需要处理的数据加载到内存中进行排序（交换式排序、选择式排序和插入式排序）
- 外部排序：需要处理的数据过大，无法加载到内存（合并排序法和直接合并排序法）

#### 03.01 冒泡排序

``````go
package main

import "fmt"
func BubbleSort(arr *[5]int) {
    fmt.Println("开始排序", arr)
    temp := 0 //临时变量，用于交换
    for i:=0; i < (*arr); i++ {
        for j:=0; j< len(*arr)-i; j++ {
            if (*arr)[j] > (*arr)[j+1] {
                temp = (*arr)[j]
                (*arr)[j] = (*arr)[j+1]
                (*arr)[j+1] = temp
            }
        }
    }
    fmt.Println("排序完毕", (*arr))
}

func main() {
    arr := [5]int{24, 69,80,57,13}
    BubbleSort(&arr)
    fmt.Println("arr=", arr) // [13 24 57 69 80]
}
``````



#### 03.02 二分查找

``````go
package main

import "fmt"

func BinaryFind(arr *[6]int, leftIndex int, rightIndex int, findVal int) {
	if leftIndex > rightIndex {
		fmt.Println("can't find", findVal)
	}
	middle := (leftIndex + rightIndex) / 2
	if (*arr)[middle] > findVal {
		BinaryFind(arr, leftIndex, middle -1, findVal)
	} else if (*arr)[middle] < findVal {
		BinaryFind(arr, middle+1, rightIndex, findVal)
	} else {
		fmt.Println("find this number, the index is",middle)
	}
}

func main() {
	num := [6]int{1, 8, 10, 89, 1000, 1234}
	// 二分查找
	BinaryFind(&num, 0, len(num)-1, 1)
}
``````



### 04. 二维数组

- 二维数组图输出

  ``````go
  package main
  
  import "fmt"
  
  func main() {
      /*
      0 0 0 0 0 0
      0 0 1 0 0 0
      0 2 0 3 0 0
      0 0 0 0 0 0
      */
      // 声明
      var arr [4][6]int
      arr[1][2] = 1
      arr[2][1] = 2
      arr[2][3] = 3
      for i:=0; i < len(arr); i++ {
          for j:=0; j < len(arr[0]); j++ {
              fmt.Println(arr[i][j], " ")
          }
          fmt.Println()
      }
  }
  ``````

- 初始化

  ``````go
  // var name [size][size]type = [size][size]type{}
  
  var arr [2][3]int = [2][3]int{{1,2,3}, {4, 5, 6}}
  // var name [size][size]type = [size][size]type{}
  // var name [size][size]type = [...][size]type{}
  // var name = [size][size]type{}
  // var name = [...][size]type{}
  ``````



### 05. map

- map是key-value数据结构
- 基本语法：`var mapNmae map[keytype]valuetype`
- `key`的类型 `bool`，数字，string，指针，channel 通常为`int`、`string`
- slice，map，function不可以作为key
- `value`的数据类型基本和key一致

#### 05.01 map的声明

``````go
var a map[string]string
var b map[string]int
var c map[string]map[string]string
``````

- map的声明不会分配内存，初始化需要make，分配内存过后才能使用

  ``````go
  package main
  
  import "fmt"
  
  func main() {
      var a map[string]string
      a = make(map[string]string, 10)
      a["no1"] = "1"
      a["no2"] = "2"
  }
  ``````

- map的`key`不可重复，如果重复了，则以最后这个key-value为准

- map的value是可以相同的

- map的key-value是无序的

- make内置函数数目，可以不指定size

#### 05.02 map的CRUD

- map增加和更新

  ``````go
  map[key] = value
  // 如果key还没有，就是增加，如果key存在就是修改
  func main() {
      cities := make(map[string]string)
      cities["1"] = "NewYork"
      cities["2"] = "Tokoy"
  }
  ``````

- map删除

  ``````go
  // delete(map, key)
  // delete是一个内置函数，如果key存在，就删除key-value,如果key不存在，不操作，也不会报错
  delete(cities, "1")
  ``````

  如果删除map所有的key，没有一个专门的方法一次删除，可以遍历一下key，逐个删除或者

  map = make(...), make一个新的让原来的成为垃圾被gc回收

- map查找

  ``````go
  val, ok := cities["2"]
  if ok {
      // key 存在
  } else {
      // key 不存在
  }
  // 如果key存在ok为true 如果key不存在 ok为false
  ``````

- map遍历

  ``````go
  // for-range 遍历map
  for k, v := range cities {
      fmt.Printf("k=%v, v=%v\n", k, v)
  }
  ``````

- map的长度

  ``````go
  func len(v Type) int
  ``````

#### 05.03 map切片

- 切片的数据类型如果是map，则为map切片

  ``````go
  package main
  import "fmt"
  
  func main() {
      var monsters []map[string]string
      monsters = make([]map[string]string, 2)
      if monsters[0] == nil {
          monsters[0] = make(map[string]string, 2)
          monsters[0]["name"] = "Gost"
          monsters[0]["age"] = "100"
      }
      
      newMonster := map[string]string {
          "name": "newGost",
          "age": "200",
      }
      monsters = append(monsters, newMonster)
  }
  ``````

#### 05.04 map排序

- Go中没有专门针对map的key进行排序

- Go中map默认无序

- Go中map的排序是先将key进行排序，然后根据key值遍历输出

  ``````go
  package main
  
  import (
  	"fmt"
      "sort"
  )
  
  func main() {
      map1 := make(map[int]int, 10)
      map[10] = 100
      map[1] = 13
      map[4] = 56
      map[8] = 90
      
      fmt.Println(map1)
      
      var keys []int
      for k, _ := range map1 {
          keys = append(keys, k)
      }
      // 对key进行排序
      sort.Ints(keys)
      fmt.Println(keys)
      
      for _, k := range keys {
          fmt.Printf("map[%v]=%v \n", k, map1[k])
      }
  }
  ``````

#### 05.05 map使用

- map是引用类型，遵守引用类型传递的机制，在一个函数接受map， 修改后，会直接修改原来的map
- map的容量达到后，再增加map元素，会自动扩容，并不会发生panic
- map的value 也经常是`struct`类型，更适合管理复杂的数据



## Character 06 Go面对对象

### 01. 结构体

- 结构体是一种聚合的数据类型，是由零个或者多个任意类型的值聚合成的实体

- 每个值称为结构体的成员/字段/属性

  ``````go
  type Cat struct {
      Name string
      Age int
  }
  ``````

- 字段的类型可以为基本类型、数组、引用类型

- 创建结构体变量后，如果没有赋值，都是对应数据类型的零值

- 结构体本身是值类型

  ``````go
  type Person struc {
      Name string
      Age int
  }
  
  var person Person
  var person2 Person = Person{}
  var person3 *Person = new(Person) // 结构体指针
  var person4 *Person = &Person{} // 结构体指针
  ``````

- 使用结构体指针类型访问字段的标准方式：(\*person3).Name  (\*person).Age = 19

- 在Go中做了简化，支持 结构体指针.字段名 person4.Name ==> (\*person).Name

- 结构体的所有字段在内存中是连续的

  ``````go
  package main
  
  import "fmt"
  
  type Point struct {
      x int
      y int
  }
  
  type Rect struct {
      leftUp, rightDown Point
  }
  
  type Rect2 struct {
      leftUp, rightDown *Point
  }
  
  func main() {
      r1 := Rect{
          Point{1, 2},
          Point{3, 4},
      }
      // r1有四个int，在内存中是连续分布
      r2 := Rect2{
          &Point{10, 20},
          &Point{30, 40},
      }
      // r2有两个*Point类型，*Point本身地址是连续的，但他们指向的地址不一定是连续的
  }
  ``````

- 结构体是用户单独定义的类型，和其他类型进行转换时需要有完全相同的字段(字段，个数，类型)

- 如果对结构体进行type重新定义，Go中认为是新的数据类型，但是可以相互转换

- `struct`的每个字段上都可以写上一个tag，该tag可以通过反射机制获取，常见的使用场景就是序列化和反序列化

  ``````go
  type Monster struct {
      Name string `json:"name"`
      Age int `json:"age"`
      Skill string `json:"skill"`
  }
  
  monster := Monster{"Gohst", 500, "gun"}
  jsonStr, err := json.Marshal(monster)
  if err != nil {
      fmt.Println("json 处理错误", err)
  }
  fmt.Println("jsonStr", string(jsonStr))
  ``````



### 02. 方法

- Go中的方法是作用在指定数据类型上的（和指定数据类型绑定），因此自定义数据类型都可以有方法，不仅仅是`struct`

- 方法的声明和调用

  ``````go
  type A struct {
      Num int
  }
  func (a A) test() {
      fmt.Println(a.Num)
  }
  // (a A) 体现test方法是和A类型绑定的
  
  type Person struct {
      Name string
  }
  
  func (p Person) test() {
      fmt.Println("test() name=", p.Name)
  }
  
  func main() {
      var p Person
      p.Name = "Tom"
      p.test()
  }
  ``````

- 方法的调用和传参机制和函数基本一样，同时方法调用时，会将调用方法的变量当作实参传递给方法，可以在方法内使用

- 结构体类型是值类型，在方法调用时，遵守值类型的传递机制，是值拷贝方式

- 如果希望在方法中修改结构体变量的值，可以通过结构体指针的方式处理

- Go中的方法作用在指定的数据类型上

- 如果一个类型实现了String()这个方法，那么`fmt.Println()`默认会调用这个变量的String()的输出

- 不管调用形式如何，真正决定是值拷贝还是地址拷贝，主要是方法和哪个类型绑定

- 如果是和值类型绑定则是值拷贝，如果和指针类型绑定则是地址拷贝



### 03. 工厂模式

- Go的结构体没有构造函数，通常使用工厂函数解决这个问题

  ``````go
  // 如果某个结构体是不可导出，可以使用工厂模式对外提供接口
  package model
  
  type student struct {
      Name string
      Score float64
  }
  
  func NewStudent(n string, s float64) *student {
      return &student{
          Name: n,
          Score: s,
      }
  }
  ``````



### 04. 封装

- 封装就是把抽象出来的字段和对字段的操作封装在一起，数据被保护在内部，程序的其他包只能通过被授权的操作才能对字段进行操作
- 封装隐藏实现细节
- 可以对数据进行验证，保证安全合理



### 05. 继承

- 解决代码复用的问题，代码冗余且不利于维护，也不利于功能的扩展问题

  ``````go
  type Goods struct {
      Name string
      Price int
  }
  
  type Book struct {
      Goods
      Writer string
  }
  // 匿名结构体嵌套到其他结构体内 实现继承特性
  ``````

- 结构体可以使用嵌套匿名结构体所有的字段和方法 即首字母大写或者小写的字段、方法 都可以使用

  ``````go
  package main
  
  import "fmt"
  
  type A struct {
      Name string
      age int
  }
  
  func (a *A) SayOk() {
      fmt.Println("A SayOk", a.Name)
  }
  
  func (a *A) hello() {
      fmt.Println("A hello", a.Name)
  }
  
  type B struct {
      A
  }
  
  func main() {
      var b B
      // b.A.Name = "tom"
      // b.A.SayOk()
      // b.A.hello()
      // 简化写法
      b.Name = "tom"
      b.SayOk()
      b.hello()
  }
  ``````

- 当执行`b.Name` 编译器先看b对应的类型中有没有Name，如果有直接调用B类型的Name字段

- 如果没有就去A中查看有没有Name字段，如果A中也没有就报panic

- 当结构体和匿名结构体有相同的字段或者方法时，编译器采用就近访问原则，如果希望访问匿名结构体的字段和方法，可以通过匿名结构体名来区分

- 结构体嵌入两个或者多个匿名结构体，如**两个匿名结构体有相同的字段和方法（同时结构体本身没有同名的字段和方法）**，在访问时，就必须明确指定匿名结构体名字，否则编译报错

- 如果一个结构体嵌套了一个有名结构体，这种模式就是组合，如果是组合关系，那么在访问组合的结构体的字段或者方法时，必须带上结构体的名字

  ``````go
  type C struct {
      a A // 有名结构体  组合关系
  }
  var c C
  c.a.Name = "john"
  ``````

- 嵌套匿名结构体后，也可以在创建结构体变量时，直接指定各个匿名结构体字段的值



### 06. 多重继承

- 如一个`struct`嵌套了多个匿名结构体，那么该结构体就可以直接访问嵌套的匿名结构体的字段和方法，从而实现了多重继承

  ``````go
  type Goods struct {
      Name string
      Price float64
  }
  
  type Brand struct {
      Name string
      Address string
  }
  
  type TV struct {
      Goods
      Brand
  }
  ``````



### 07. 接口

- 接口类型是对其他类型行为的抽象和概括，因为接口类型不会和特定的实现细节绑定在一起，通过这种抽象的方式可以让函数更加灵活和更具有适应能力

- Go中接口类型的独特之处在于它是满足隐式实现的，也就是说没有必要对于给定的具体类型定义所有满足的接口类型

- 接口类型可以定义一组方法，但是不需要实现，并且接口不能包含任何变量

  ``````go
  tyoe InterfaceName interface {
      method(args) Rargs
      ...
  }
  ``````

- 接口里所有方法都没有方法体，即接口的方法都是没有实现的方法，接口体现了程序设计的多态和高内聚低耦合的思想

- Go中的接口不需要显式的实现。只要一个变量含有接口类型中的所有方法，那么这个变量就实现了这个接口

- 接口本身不能创建实例，但是可以指向一个实现了该接口的自定义类型的变量

  ``````go
  package main
  
  import "fmt"
  
  type Ainterface interface {
      Say()
  }
  
  type Stu struct {
      Name string
  }
  
  func (str Stu) Say() {
      fmt.Println("str Say()")
  }
  
  func main() {
      var stu Stu
      var a Ainterface = stu
      a.Say()
  }
  ``````
  
- 一个自定义类型只有实现了某个接口才能将该自定义类型的实例赋值给接口类型

- 一个自定义类型可以实现多个接口

  ``````go
type AInterface interface {
      Say()
  }
  
  type BInterface interface {
      Hello()
  }
  
  type Monster struct {
      
  }
  
  func (m Monster) Hello() {
      fmt.Println("Monster Hello()~~~")
  }
  
  func (m Monster) Say() {
      fmt.Println("Monster Say()~~~")
  }
  
  var monster Monster
  var a2 AInterface = monster
  var b2 BInterface = monster
  a2.Say()
  b2.Hello()
  ``````
  
- 一个接口可以继承多个其他接口，这是如果实现这个接口也必须实现该接口继承接口的方法
  
- 接口类型默认是一个指针（引用类型），如果没有对interface初始化就使用，那么会输出nil
  
- 空接口没有任何方法，所以所有类型都实现了空接口，可以把变量赋值给空接口
  
- 接口实践对Hero结构体切片排序`sort.Sort(data interface)`
  
  ``````go
  package main
  
  import (
  	"fmt"
      "sort"
      "math/rand"
  )
  
  type Hero struct {
      Name string
      Age int
  }
  
  type HeroSlice []Hero
  
  
  // 实现Sort 接口Len方法
  func (hs HeroSlice) Len() int {
      return len(hs)
  }
  
  // 实现接口 Less方法 决定按照什么规则排序
  func (hs HeroSlice) Less(i, j int) bool {
      // 按照年龄排序
      return hs[i].Age < hs[j].Age
      
      // 按照Name排序
      // return hs[i].Name < hs[j].Name
  }
  
  // 实现接口 Swap方法交换数据
  
  func (hs HeroSlice) Swap(i, j int) {
      hs[i], hs[j] = hs[j], hs[i]
  }
  
  func main() {
      var intSlice = []int{0, -1, 10, 7, 90}
      // 排序
      sort.Ints(intSlice)
      
      var heros HeroSlice
      for i:=0; i < 10; i++ {
          hero := Hero{
              Name: fmt.Sprintf("Hero|%d", rand.Intn(100))
              Age: rand.Intn(100),
          }
          heros = append(heros, hero)
      }
      sort.Sort(heros)
  }
  
  ``````
  
  
  
### 08. 多态

- 变量具有多种形态，在Go中多态特征通过接口实现，可以按照统一的接口来调用不同的实现，这时接口变量就呈现不同的形态

  ``````go
  package main
  
  import "fmt"
  
  type Usb interface {
      Start()
      Stop()
  }
  
  type Phone struct {
      name string
  }
  
  func (p Phone) Start() {
      fmt.Println("Phone start work")
  }
  
  func (p Phone) stop() {
      fmt.Println("Phone stop work")
  }
  
  type Camera struct {
      name string
  }
  
  func (c Camera) Start() {
      fmt.Println("Camera start work")
  }
  
  func (c Camera) Stop() {
      fmt.Println("Camera stop work")
  }
  
  func main() {
      var usbArr [3]Usb
      usbArr[0] = Phone{"xiaomi"}
      usbArr[1] = Phone{"Vivo"}
      usbArr[2] = Camera("Nikon")
      fmt.Println(usbArr)
  }
  ``````



### 09. 类型断言

- 类型断言，由于接口类型是一般类型不知道具体类型，如果要转成具体类型，需要引入类型断言

  ``````go
  var x interface{}
  var b2 float32 = 1.1
  x = b2
  y := x.(float32)
  // 在进行类型断言时，如果类型不匹配，就会报panic
  // 如果类型断言时带上检测机制，如果成功就ok但也不会报panic
  if y, ok := x.(float32); ok {
      fmt.Println("Success")
  }
  ``````

  

  

  

  






























