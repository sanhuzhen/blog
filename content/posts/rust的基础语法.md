---
title: rust的基本语法
date: 2024-11-01
taxonomies:
  categories: ["Rust"]
  tags: ["Rust"]
---
打印 ` Hello,World!`

```rust
fn main(){
    println!("Hello,World!");
}
```

## 变量

### 可变性

rust中的变量与kotlin同样具有可变性和不可变性

```rust
let m = 6;
m = 7;
```

上述就会报错，因为直接使用 ` let`声明的变量具备不可变性，即赋值一次就不可变了，与kotlin中的 ` val`一致

但我们可以用 ` mut`来将变量 ` m`赋予可变性，即

```rust
let mut m = 6;
m = 7;
```

### 常量

同理，rust中一样会有常量，这时我们可以与其他语言一样，用 ` const`来声明常量

```rust
const MATH.PI: f64 = 3.1415926;
```

常量默认不可变，并且必须表明其类型，比如上述的 ` f64`就表明 ` MATH.PI`是一个浮点数

### 隐藏

rust中如果我们在后面新定义了一个与前面变量名一样的变量，我们就说前一个变量就被这一个变量给隐藏了。

```rust
fn main(){
    let mut x = 3;
    x = x * 2;
    {
        let x = x * 2;
        println!("{x}");
    }
    println!("{x}");
}
输出：
12
6

```

` {}`就是一个作用域，我们在新的作用域中使用 ` let x = `又新创造了一个新的变量，将之前的隐藏了，当 ` {}`结束后，x又返回之前的值，故输出6。

` mut`和隐藏的区别：

- ` mut`并不会受作用域的影响，如上这个程序，如果不用 ` let x =`去新创造一个变量，仅单单 ` x = x * 2`那么，酒后程序输出的都是12

- ` mut`还不会改变变量的类型，而隐藏可以改变这个新的变量的类型

  ```rust
  fn main(){
      let str = "   ";
      let str = str.len();
  }
  ```

  如上，我们创造了一个 ` str`字符串，后又用 ` str`来表示 ` str`的长度，二者的类型是不同的，但我们可以用隐藏来实现，如果用 ` mut`那肯定就不行。

## 数据类型

rust是一门静态类型语言，这表明编译器在编译时，就需要知道所有变量的类型。

rust的数据类型主要分为：

- 标类：整型，浮点型，布尔型和字符类型（感觉其他语言的基本类型也是这些？）
- 复合

### 标类

#### 整型

| 有符号   | 无符号   |
| -------- | -------- |
| ` i8`    | ` u8`    |
| ` i16`   | ` u16`   |
| ` i32`   | ` u32`   |
| ` i64`   | ` u64`   |
| ` i128`  | ` u128`  |
| ` isize` | ` usize` |

有无符号代表该数是否可以为负数

` isize`和 ` usize`类型依赖于运行程序的计算机架构，64位为64位，32位为32位

#### 浮点数

` f32`和 ` f64`分别占32位和64位，rust默认位 ` f64`.

```rust
fn main(){
    let x = 3.2;//f64
    let x: f32 = 3.2;//f32
}
```

#### 布尔型

与其他语言一样，rust也有 ` bool`型变量

```rust
fn main(){
    let target = true;
    let target: bool = false;
}
```

#### 字符类型

` char`主要用来表示字符类型，字符一般用单引号来声明 ` char`字面量

### 复合

rust中将多个值组合成一个类型，rust有两个原生的复合类型：

- 元组（tuple）
- 数组（array）

#### 元组

元组是一个将多个其他类型的值组合进一个复合类型的主要方式。

元组长度固定：一旦声明，其长度不会增大或缩小。

```rust
fn main(){
    let tup: (i32, f64, u32) = (500, 3.14, 2);
}
```

##### 解构

为了从元组中获取单个值，可以使用模式匹配（pattern matching）来解构（destructure）元组值

```rust
fn main(){
    let tup: (i32, f64, u32) = (500, 3.14, 2);
    let (x, y, z) = tup;
    println!("{y}");
}
输出：
3.14

```

我们也可以使用点号（`.`）后跟值的索引来直接访问它们（索引从0开始）

```rust
fn main(){
    let tup: (i32, f64, u32) = (500, 3.14, 2);
    let y = tup.1;
    println!("{y}");
}
```

不带任何值的元组有个特殊的名称，叫做 **单元（unit）** 元组。这种值以及对应的类型都写作 `()`，表示空值或空的返回类型。如果表达式不返回任何其他值，则会隐式返回单元值。

#### 数组

数组中的每个元素的类型必须相同

我们将数组的值写成在方括号内，用逗号分隔：

```rust
fn main(){
    let nums = [0, 1, 2, 3, 4];
}
```

可以像这样编写数组的类型：在方括号中包含每个元素的类型，后跟分号，再后跟数组元素的数量。

```rust
fn main(){
    let nums: [i32: 5] = [0, 1, 2, 3, 4];
}
```

通过在方括号中指定初始值加分号再加元素个数的方式来创建一个每个元素都为相同值的数组：

```rust
fn main(){
    let nums = [3: 5];
    //等价于 let nums = [3, 3, 3, 3, 3];
}
```

## 函数

对于任意语言，都有函数这个概念，rust也一样

rust主要用 ` fn`来表示一个函数：

```rust
fn man(){
    println!("Hello,World!");
}
```

要带参数直接 ` name: type`写在函数的括号里：

```rust
fn man(num: i32){
    println!("Hello,{num}");
}
```

### 语句和表达式

语句：执行一些操作，但不返回值

表达式：执行操作，并且返回值。

我们在平时写的 ` let x = 6`就是一个语句，

函数定义也是语句.

所以，不能把 `let` 语句赋值给另一个变量，如下面这个栗子：

```rust
fn main(){
    let x = {let y = 3};
}
```

函数调用是一个表达式。宏调用是一个表达式。用大括号创建的一个新的块作用域也是一个表达式，例如：

```rust
fn main(){
    let x = {
        let y = 2;
        y + 2//注意这里没有分号，如果有分号就是语句了，不会返回只给x
    };
    println!("{x}");
}
```

### 带返回值的函数

```rust
fn man() -> u32{
    5
}
```

## 控制流

### ` if`语句

```rust
fn main(){
    let num = 5;
    if num > 4 {
        println!("num is big!");
    } else {
        println!("num is small!");
    }
}
```

注意，条件一定时 ` bool`类型。

当然，` if`语句可以直接给变量赋值！

```rust
fn main(){
    let condition = true;
    let num = if condition { 6 } else { 5 };
    println!("{num}");
}
```

### 循环

#### ` loop`

`loop` 关键字告诉 Rust 一遍又一遍地执行一段代码直到你明确要求停止。

```rust
fn main(){
    loop {
        println!("Hello,World!");
    }
}
```

当然，循环当中也有 ` break`和 ` continue`关键字。

从循环中返回值

```rust
fn main(){
    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter % 2 == 0{
            break counter * 2;
        }
    };
    println!("{result}");
}
```

循环标签：消除多个循环之间的歧义。

```rust
fn main(){
    'counter1: loop{
        loop{
            //主要是为了消除多层循环之间的歧义，并不像其他语言那样，直接结束最靠近的那一层循环，也可以结束最外面的循环
            break 'counter1;
        }
    }
}
```

#### ` while`

```rust
fn main(){
    let mut counter = 10;
    while counter >= 5 {
        println!("{counter}");
        counter -= 1;
    }
    println!("{counter}");
}
```

#### ` for`

```rust
fn main(){
    for i in 1..4 {
        println!("{i}");
    }
}
```

但对于数组而言，

```rust
fn main(){
    let nums = [0, 1, 2, 3, 4];
    for i in nums {
        println!("{i}");
    }
}
```

