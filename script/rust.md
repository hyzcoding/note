# [RUST](https://www.rust-lang.org/learn)

# 环境安装

# 基本语法

**注意：**`Rust` 不支持 `++` 和`--`

## 打印

```rust
println!("会保存
    回车等格式");
println!(r#"忽略"等字符，不需要转义,适合创建json"#);

fn r#return(){}
let r#let = 6; 
r#return(); // 用于自定义与关键字一样的函数及变量
println!("{:?}", b"This will look like numbers"); // 作为byte数组打印
println!("{:X}", '행' as u32); // 打印16进制
println!("Binary: {:b}, hexadecimal: {:x}, octal: {:o}", number, number, number); // 2 16 10
println!("This is {1} {2}, son of {0} {2}.", father_name, son_name, family_name);
println!("{},{{}}",a);  //{{}}转义字符,转义后为{}
println!(
        "{city1} is in {country} and {city2} is also in {country},
but {city3} is not in {country}.",
        city1 = "Seoul",
        city2 = "Busan",
        city3 = "Tokyo",
        country = "Korea"
    );
println!("{:ㅎ^11}", letter); // ㅎㅎㅎㅎㅎaㅎㅎㅎㅎㅎ 前五个和后五个为ㅎ
//------------------------//
fn main() {
    let title = "TODAY'S NEWS";
    println!("{:-^30}", title); // no variable name, pad with -, put in centre, 30 characters long
    let bar = "|";
    println!("{: <15}{: >15}", bar, bar); // no variable name, pad with space, 15 characters each, one to the left, one to the right
    let a = "SEOUL";
    let b = "TOKYO";
    println!("{city1:-<15}{city2:->15}", city1 = a, city2 = b); // variable names city1 and city2, pad with -, one to the left, one to the right
}
/**
 * ---------TODAY'S NEWS---------
 * |                            |
 * SEOUL--------------------TOKYO
 *
 **/
//------------------------//
```



## 变量

> 强类型语言

```rust
let a=123;				// 默认变量值不可修改,默认为32位整形
a=4; 					// 编译错误
let a=56;				// 可以运行，是重复绑定，可以发生值、类型、可变属性的变化
let mut a = 123; 		// 可变变量,仅值可以修改
const b:u64=123; 		// 定义为常量，无法重新绑定
let b=56				// 编译错误
```

## 数据类型

 ### String

  ```rust
  // &str和String
  
  ```

  

 ### 整形

| 位长度  | 有符号 | 无符号 |
| :------ | :----- | :----- |
| 8-bit   | i8     | u8     |
| 16-bit  | i16    | u16    |
| 32-bit  | i32    | u32    |
| 64-bit  | i64    | u64    |
| 128-bit | i128   | u128   |
| arch    | isize  | usize  |

 ### 浮点型

  ```rust
  let x = 2.0; // f64,更常用,效率相当
  let y: f32 = 3.0; // f32
  ```


 ### 布尔

  ```rust
  let x:bool=false; // 只能是true,false
  ```

 ### 字符

  ```rust
  let x:char='a'; // 必须使用utf8编码，4字节
  ```

 ### 复合类型


   #### 元组

```rust
/* 元组 一组数据 可以包含不同类型 */
let tup:(u16,f32,char) = (500,6.5,'a');
let (x,y,z) = tup; // z='a'
```

   #### 数组

```rust
/* 数组 同类型 */
let a = [1,2,3];
let b:[i16,3]=[1,2,3]; // 长度为3的i16类型的整数数组
let c = [3; 5];// 等同于 [3, 3, 3, 3, 3]
let mut d = [1, 2, 3];
d[0] = 4; 
let array_of_ten = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let three_to_five = &array_of_ten[2..5];
let start_at_two = &array_of_ten[1..];
let end_at_five = &array_of_ten[..5];
let everything = &array_of_ten[..];
```




## 函数

 ### 基本结构

```rust
fn functionName(params) -> result{body;}
fn main() {
    println!("Hello, world!");
    another_function(2);
}
fn another_function(x: i32) { //全小写，单词间下划线分隔
    println!("Hello, world!{}",x);
}
fn add(a: i32, b: i32) -> i32 {
    return a + b;
}
```

- ### 函数式编程

  ```rust
  let x = {let y = 5;y+1};
  ```
## 控制语句

- ### 条件语句

  > 条件表达式必须是 bool 类型

  ```rust
  if number <5{
      ...
  }else if number <10 && number >5{
      ...
  }else{
      
  }
  // 函数表达式
  let number = if a>0{1}else{0};
  // 注意：两个函数体表达式的类型必须一样！且必须有一个 else 及其后的表达式块。
  ```

  

- ### 循环语句

  ```rust
  /* while */
  let mut number = 1;
  while number != 4 {
      println!("{}", number);
      number += 1;
  }
  /* for */
  let a = [10, 20, 30, 40, 50];
  for i in a.iter() {
      println!("值为 : {}", i);
  }
  for j in 0..5 {
      println!("a[{}] = {}", j, a[j]);
  }
  /* loop 相当于 while(true) */
  let s = ['R', 'U', 'N', 'O', 'O', 'B'];
  let mut i = 0;
  loop {
      let ch = s[i];
      if ch == 'O' {
          break;
      }
      println!("\'{}\'", ch);
      i += 1;
  }
  /- -/
  let s = ['R', 'U', 'N', 'O', 'O', 'B'];
  let mut i = 0;
  let location = loop {
      let ch = s[i];
      if ch == 'O' {
          break i;
      }
      i += 1;
  };
  println!(" \'O\' 的索引为 {}", location);
  ```

## 所有权

> 除了基本类型变量，其他变量移动则之前的变量失效
>
> 如果将变量当作参数传入函数，那么它和移动的效果是一样的
>
> 被当作函数返回值的变量所有权将会被移动出函数并返回到调用函数的地方，而不会直接被无效释放

```rust
let s1 = String::from("hello");
let s2 = s1; // 赋值给s2时把所有权交给s2，此时s1未赋值状态
println!("{}, world!", s1); // 错误！s1 已经失效 

let s1 = String::from("hello");
let s2 = s1.clone();
println!("s1 = {}, s2 = {}", s1, s2);
```

### 引用与租借

> 相当于指针
>
> **&** 运算符可以取变量的"引用"。

```rust
let s1 = String::from("hello");
let s2 = &s1;
let s3 = &s1;
println!("s1 is {}, s2 is {}， s2 is {}", s1, s2, s3);

let s1 = String::from("hello");
let s2 = &s1;
let s3 = s1;
println!("{}", s2); // 编译错误，s2无效，s2租借过期

let s1 = String::from("hello");
let mut s2 = &s1;
let s3 = s2;
s2 = &s3; // 重新从 s3 租借所有权
println!("{}", s2);
s2.push_str("oob"); // 编译错误，禁止修改租借的值

let mut s1 = String::from("run"); // s1 是可变的
let s2 = &mut s1; // s2 是可变的引用
s2.push_str("oob");
println!("{}", s2);

let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s; // 多重引用，编译错误
println!("{}, {}", r1, r2);
```

### 垂悬引用

> "垂悬引用"在 Rust 语言里不允许出现，如果有，编译器会发现它。

```rust
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");
    &s // 引用所指向的值已经不能确定的存在，故不允许其出现。
}
```

## 切片

> 尽量不要在字符串中使用非英文字符，因为编码的问题。

```text
..y 等价于 0..y
x.. 等价于位置 x 到数据结束
.. 等价于位置 0 到结束
```

```rust
let mut s = String::from("runoob");
let slice = &s[0..3];
 s.push_str("yes!"); // 错误,s 被部分引用，禁止更改其值。
```

```rust
let s1 = String::from("hello");
let s2 = &s1[..];
// string str格式转换
// 凡是用双引号包括的字符串常量整体的类型性质都是 &str：
```

## 结构体

- ### 对象

```rust
struct Site {
    domain: String,
    name: String,
    nation: String,
    found: u32
}
```

```rust
let runoob = Site {
    domain: String::from("domain"),
    name: String::from("name"),
    nation: String::from("nation"),
    found: 2020
};
```

- ### 元组

```rust
struct Color(u8, u8, u8);
let black = Color(0, 0, 0);
println!("black = ({}, {}, {})", black.0, black.1, black.2);
```

- ### 结构体所有权

> 结构体失效的时候会释放所有字段。

```rust
#[derive(Debug)] // 必须调用才可以输出结构体

struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };

    println!("rect1 is {:?}", rect1); // {:?} 一行输出 。 {:#?} 格式化输出
}
```

- ### 结构体方法

> 结构体方法的第一个参数必须是 &self，不需声明类型，因为 self 不是一种风格而是关键字。

```rust
struct Rectangle {
    width: u32,
    height: u32,
}
   
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
    fn wider(&self, rect: &Rectangle) -> bool {
        self.width > rect.width
    }
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };
    let rect2 = Rectangle { width: 40, height: 20 };
    println!("rect1's area is {}", rect1.area());
    println!("{}", rect1.wider(&rect2));
}
```

- ### 结构体关联函数

> 相当于静态方法	
>
>  **String::from** 函数就是一个"关联函数"。

```rust
impl Rectangle {
    fn create(width: u32, height: u32) -> Rectangle {
        Rectangle { width, height }
    }
}
let rect = Rectangle::create(30, 50);
println!("{:?}", rect);
```

- ### 单元结构体

```rust
struct UnitStruct;
```

## 枚举

```rust
enum Book {
    Papery, Electronic
}
let book = Book::Papery;

enum Book {
    Papery(u32),
    Electronic(String),
}
let book = Book::Papery(1001);
let ebook = Book::Electronic(String::from("url://..."));

enum Book {
    Papery { index: u32 },
    Electronic { url: String },
}
let book = Book::Papery{index: 1001};
```

- ### match 语法

> 相当于 switch()方法

```rust
match book {
    Book::Papery { index } => {
        println!("Papery book {}", index);
    },
    Book::Electronic { url } => {
        println!("E-book {}", url);
    }
}
```

```rust
match 枚举类实例 {
    分类1 => 返回值表达式,
    分类2 => 返回值表达式,
    ...
}
```

>  除了能够对枚举类进行分支选择以外，还可以对整数、浮点数、字符和字符串切片引用（&str）类型的数据进行分支选择
>
> 在例外情况下没有任何要做的事 **.** 例外情况用下划线 **_** 表示

```rust
let t = "abc";
match t {
    "abc" => println!("Yes"),
    _ => {},
}
```



## 组织

- ### 箱

  > "箱"是二进制程序文件或者库文件，存在于"包"中。

- ### 包

  > 程的实质就是一个包，包必须由一个 Cargo.toml 文件来管理，该文件描述了包的基本信息以及依赖项。

- ### 模块

  > Java 组织功能模块的主要单位是类，而 JavaScript 组织模块的主要方式是 function,Rust 中的组织单位是模块（Module）。

  ```rust
  mod nation {
      mod government {
          fn govern() {}
      }
      mod congress {
          fn legislate() {}
      }
      mod court {
          fn judicial() {}
      }
  }
  ```

  ```bash
  nation
   ├── government
   │ └── govern
   ├── congress
   │ └── legislate
   └── court
     └── judicial
  ```

  ```rust
  //  路径
  // 绝对路径 从 crate 关键字开始描述
  crate::nation::government::govern();
  // 相对路径 从 self 或 super 关键字或一个标识符开始描述
  nation::government::govern();
  ```

- ### 访问权限

  > Rust 中有两种简单的访问权：公共（public）和私有（private）。
  >
  > 默认情况下，如果不加修饰符，模块中的成员访问权将是私有的,只有在与其平级的位置或下级的位置才能访问。

  ```rust
  mod back_of_house {
      pub struct Breakfast {
          pub toast: String,
          seasonal_fruit: String,
      }
  
      impl Breakfast {
          pub fn summer(toast: &str) -> Breakfast {
              Breakfast {
                  toast: String::from(toast),
                  seasonal_fruit: String::from("peaches"),
              }
          }
      }
  }
  pub fn eat_at_restaurant() {
      let mut meal = back_of_house::Breakfast::summer("Rye");
      meal.toast = String::from("Wheat");
      println!("I'd like {} toast please", meal.toast);
  }
  fn main() {
      eat_at_restaurant()
  }
  ```

- ### 模块引用

  [**官方标准库**](https://doc.rust-lang.org/stable/std/all.html)

  ```rust
  mod second_module;
  ...
  println!("{}", second_module::message());
  
  // second_module.rs
  pub fn message() -> String {
      String::from("This is the 2nd module.")
  }
  
  // use 关键字能够将模块标识符引入当前作用域：
  mod nation {
      pub mod government {
          pub fn govern() {}
      }
      pub use government::governa;
      pub fn govern() {}
  }
  
  use crate::nation::government::govern;
  use crate::nation::govern as nation_govern;
  fn main() {
      nation::governa();
      nation_govern();
      govern();
  }
  ```

## 异常处理

```rust
// 常用
if let Ok(result) = function(params){
    println!("success result {}",result);
}else{
    // panic! 宏被调用时停止运行
    panic!("error message");
}
```

## 泛型

```rust
fn max<T>(array:&[T]) ->T {
	...
}
```

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
```

## 生命周期

```rust
&i32        // 常规引用
&'a i32     // 含有生命周期注释的引用
&'a mut i32 // 可变型含有生命周期注释的引用
```

```rust
fn longer<'a>(s1: &'a str, s2: &'a str) -> &'a str {
    if s2.len() > s1.len() {
        s2
    } else {
        s1
    }
}
```

## 文件与IO
- ### 命令行交互

```rust
use std::io;

let mut guess = String::new();
io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

```

- ### 参数

```rust
fn main() {
    let args = std::env::args();
    for arg in args {
        println!("{}", arg);
    }
}
```

- ### 文件读取

```rust
use std::fs;

fn main() {
    let text = fs::read_to_string("D:\\text.txt").unwrap();
    println!("{}", text);
}
```

[通过例子学 Rust]: https://www.bookstack.cn/read/rust-by-example-cn/

