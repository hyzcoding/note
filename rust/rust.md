# [RUST](https://www.rust-lang.org/learn)

## 环境安装

## cargo

> *cargo* 是*rust*项目的构建框架和依赖管理工具，使用命令进行快速编译、打包和运行

### 常用命令

- 创建`cargo`项目

  `cargo new project_name` 创建`rust`项目

- 编译与打包`cargo`项目

  `cargo build` 对项目进行编译打包，编译后的文件位于`target/debug` 目录下，不同操作系统对应不同的打包文件，适用于开发环境。

  `cargo build --release` 对项目进行生产包(正式环境)的编译打包，相对`cargo build`命令，该命令适用于生产环境，编译及打包时间更长，代码运行更快，编译后的可执行文件位于`targer/release` 目录下。

### 项目结构

- `src`
- `Cargo.toml`

## 基本语法

**注意：**`Rust` 不支持 `++` 和`--`

### 打印

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



### 变量

> 强类型语言

```rust
let a=123;				// 默认变量值不可修改,默认为32位整形
a=4; 					// 编译错误
let a=56;				// 可以运行，是重复绑定，可以发生值、类型、可变属性的变化
let mut a = 123; 		// 可变变量,仅值可以修改
const b:u64=123; 		// 定义为常量，无法重新绑定
let b=56				// 编译错误
```

### 数据类型

> 除`string`的其他数据类型都是存放在`stack`上，超出作用域则会弹出。
>
> ```rust
> // 基本数据类型直接到栈中
> let x=5;
> let y=x;
> ```
>
> 

 #### String

> 存储在heap上，以byte形式
>
> 实际上是对`Vec<u8>`的包装，杜绝下标取值

- 字符串字面值(字符串切片`&str`)与`string` 区别

  > 区别于string类型，程序里写死的字符串值，不可变。不具有灵活性，直接存储在二进制程序中， 
  >
  > ```rust
  > let s = "Hello world"; //s 实际上是指向字面值的字符串切片 类型是&str 不可变
  > ```
  >
  > 
  >
  > 编译时就知道内容，直接被编译到执行文件中
  >
  > 速度快、高效。但是不可变。

  > string 为支持可变性，存在heap中，通过`String::from`实现操作系统请求内存过程。
  >
  > 当String使用完成，需要使用某种方式进行回收。
  >
  > drop函数

- 常用

  - len() 字符串长度

  - 取值

    ```rust
    w.bytes // 字节
    w.chars // 字符 标量值
    // 字形簇
    ```

  - 切片 允许但是需注意字符范围 防止panic出现

- 创建

  - from

  ```rust
  // mut 可变 ::from 表示 String类型下面的函数
  let mut s = String::from("Hello World");
  s.push_str("!!!");
  s.push('!');
  // println! 宏
  println!("{}",s); // Hello World!!!
  
  let s1 = String::from("Hello");
  let s2 = String::from("World");
  let sa = s1 + &s2; // s1 不可用
  format!("{},{},{}"); // 不会取得所有权
  ```

- 变量与数据交互

  > 一个String类型变量由三个部分组成
  >
  > 栈中
  >
  > ​	ptr(指向字符串内容的内存指针)  
  >
  > ​	len(字符串存放所需的长度)
  >
  > ​	capacity(容量 String从操作系统总共获得的内存字节数)
  >
  > 堆中
  >
  > ​	字符串内容

- Move

  ```rust
  let s1 = String::from("Hello");
  let s2 = s1; // 此过程将s1的栈内容复制一份到s2,并没有复制heap中的内容 同时让s1失效(只有对象类型会进行失效操作)
  // String没实现copy接口
  // 当变量离开作用域，rust调用drop函数，释放heap内存
  // 其他语言s1,s2离开作用域，会尝试释放相同的内存 导致二次释放
  ```

- clone

  ```rust
  let s1 = String::from("Hello");
  let s2 = s1.clone(); // 栈内容和堆内容都进行复制
  ```

- copy trait(copy 接口)

  > 如果类型实现 `copy trait` ,旧的变量仍然可以使用
  >
  > 如果一个类型或一部分实现`drop trait` , 那么不能实现`copy trait`

  > 简单标量 u32/bool/char/f64/tuple(元组内元素实现`copy trait`)

  ```rust
  // &str和String
  
  ```

  

 #### 整形

| 位长度  | 有符号 | 无符号 |
| :------ | :----- | :----- |
| 8-bit   | i8     | u8     |
| 16-bit  | i16    | u16    |
| 32-bit  | i32    | u32    |
| 64-bit  | i64    | u64    |
| 128-bit | i128   | u128   |
| arch    | isize  | usize  |

 #### 浮点型

  ```rust
  let x = 2.0; // f64,更常用,效率相当
  let y: f32 = 3.0; // f32
  ```


 #### 布尔

  ```rust
  let x:bool=false; // 只能是true,false
  ```

 #### 字符

  ```rust
  let x:char='a'; // 必须使用utf8编码，4字节
  ```

 #### 复合类型


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




### 函数

 #### 基本结构

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
### 控制语句

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

### 前提知识

- **栈内存**与**堆内存**

  - 介绍

    > Stack LIFO
    >
    > 压栈与弹出 ，存储的所有数据大小必须已知，编译时未知大小或大小可能变化的数据必须放在heap上。
    >
    > Heap LILO
    >
    > 先请求一定大小的空间，并标记在用，返回指针(heap地址) ，此过程称为堆内存分配。
    >
    > 因为指针大小固定，可以将指针放在stack中，想要获取实际数据时再通过指针地址进行数据定位获取。

  - 对比

    > heap 相对于stack 访问数据要慢，多了指针索引的过程
    >
    > 数据之间存放位置也影响数据处理速度

###　所有权存在的原因
- 管理heap数据

  > 追踪代码哪部分正在使用heap中的数据
  >
  > 最小化heap的重复数据量
  >
  > 清理heap上未使用数据避免空间不足

### 所有权的规则

- 每个值都有一个变量，这个变量是该值的使用者
- 每个值同时只能有一个所有者
- 所有者超出作用域(`scope`)，该值将会被删除


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

### 返回与作用域

- 变量所有权遵循模式:
  - 值赋给另外的变量就会发生移动
  - 包含`heap`变量离开作用域时，值就会被`drop`清理,<red>除非</red>所有权移动到另外一个变量上。	

```rust
fn main(){
    let s1 = String::from("bb");
    let s2 = take_and_back(s1); // s1 失效 s2 获取move后的值
}
fn give_own()->String{
    String::from("aa")
}
fn take_and_back(a_string:String)->String{ // a_string 进入作用域
    a_string // a_string move drop
}
```



### 引用与租借

> 相当于指针
>
> **&** 运算符可以取变量的"引用"。
>
> 让函数使用某个值，但是不获取其所有权
>
> 引用必须一直有效，可以有多个不可变引用，或只能有一个可变引用

```rust
let s1 = String::from("hello"); //s1 是指针，指向heap的真实内容
let s2 = &s1; // s2实际是s1的指针，指向s1，没有获得所有权 称为租借
let s3 = &s1; // 借用的变量是不可变的，可以有多个
println!("s1 is {}, s2 is {}， s2 is {}", s1, s2, s3);

let s1 = String::from("hello");
let s2 = &s1;
let s3 = s1;
println!("{}", s2); // 编译错误，s2无效，s2租借过期

let s1 = String::from("hello");
let mut s2 = &s1; // 可变引用
let s3 = s2;
s2 = &s3; // 重新从 s3 租借所有权
println!("{}", s2);
s2.push_str("oob"); // 编译错误，禁止修改租借的值

let mut s1 = String::from("run"); // s1 是可变的
let s2 = &mut s1; // s2 是可变的引用 ，特定的作用域，对某一块数据，只能有一个可变引用。防止数据竞争
s2.push_str("oob");
println!("{}", s2);
// 数据竞争
// 两个或多个指针同时访问同一数据
// 至少一个指针用于写数据
// 没有使用任何机制对数据访问进行同步
// 可以通过创建新作用域，来允许非同时的创建多个可变引用
{
    let s3 = &mut s1;
}
// 不可同时拥有可变与不可变引用
let mut s = String::from("hello");
let r1 = &mut s;
let r2 = &mut s; // 多重引用，编译错误
println!("{}, {}", r1, r2);
```

#### 垂悬引用

> 指一个指针引用内存中的某个地址，但这块内容已经被释放并给其他程序使用
>
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

> 不持有所有权的数据类型(slice)
>
> 尽量不要在字符串中使用非英文字符，因为编码的问题，索引范围必须发生在有效的UTF-8字符边界内。

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

## 数据集合

- vector

  > 在内存中是连续的，也遵循租借规则

  ```RUST
  let v:Vec<i32>=Vec::new(); // 没有初始化元素类型时，需要用泛型指定类型
  let v=vec![1,2,3];
  let mut v=Vec::new();
  v.push(1);
  let third:&i32=&v[2]; // 下标越界会出现painc!
  match v.get(2){
      Some(third)=>println("number is {}",third),
      None=>println("out of index"),
  }
  ```

  

- HashMap

  > 同构 k 必须同一类型 v 必须同一类型
  >
  > 存在heap中

  ```rust
  use std::collections::HashMap;
  fn main(){
      // 赋值
      let mut scores = HashMap::new();
      scores.insert(String::from("key"),10);
      let key = String::from("key");
      scores.insert(&key,10);
     // 转换
      let teams = vec![String::from("Yellow"),String::from("Blue")];
      let scores = vec![10,20];
      let res:HashMap<_,_> = 
      // key.zip(value).collect()
      	teams.iter().zip(scores.iter()).collect();
      // 遍历
      for(k,v) in scores{
          println!("key {}, value {}",k,v);
      }
      // 替换现有值
      scores.insert(String::from("key"),25); // 替换为25
      // 不存在则插入
      let sco =scores.entry(String::from("key")).or_insert(50); // 25 返回value
  }
  ```

  

## 结构体

> struct

- ### 对象

```rust
struct mut Site { // mut标记为可变，struct 为可变则field都为可变
    domain: String,
    name: String,
    nation: String,
    found: u32
}
```

```rust
let runoob = Site {
    domain: String::from("domain"), // 变量名和field名一致可使用简写
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
>
> - std:fmt:Display
> - std:fmt:Debug
> - #[derive(Debug)]
> - {:?}
> - {:#?}

```rust
#[derive(Debug)] // 必须调用才可以输出结构体 或者手动实现Debug 或者实现Display
// 与结构体绑定
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

  - 方法与函数的不同
    - 方法是在struct/enum/trait上下文中定义
    - 方法的第一个参数是&self,表示方法被调用的struct实例

> 结构体方法的第一个参数必须是 &self，不需声明类型，因为 self 不是一种风格而是关键字。

```rust
struct Rectangle {
    width: u32,
    height: u32,
}
   
impl Rectangle { // 定义方法的块
    fn area(&self) -> u32 { // 不获取所有权 mut &self / self 都可以
        self.width * self.height
    }
    fn wider(&self, rect: &Rectangle) -> bool {
        self.width > rect.width
    }
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };
    let rect2 = Rectangle { width: 40, height: 20 };
    println!("rect1's area is {}", rect1.area()); // 方法调用时会自动解引用
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



## 代码组织

> package->crate->module
>
> 一个package
>
> - 只包含一个cargo.toml,描述如何构建crates
> - 只包含0或1个library crate
> - 可包含任意数量的binary crate
> - 必须包含一个crate(无类型要求)

#### 箱

> "箱"是二进制程序文件或者库文件，存在于"包"中。 crate

##### 惯例

> cargo将crate root交给rustc 构建library或binary
>
> 一个package可同时含有 main.rs和lib.rs
>
> binary crate放在 src/bin 目录下

- src/main.rs

  binary crate的crate root

  crate名称与package相同

- src/lib.rs

  package包含一个library crate

  library crate的crate root

  crate名称与package相同

#### 包

> 程的实质就是一个包，包必须由一个 Cargo.toml 文件来管理，该文件描述了包的基本信息以及依赖项。

#### 模块

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
// 绝对路径 从 crate 关键字开始描述 隐式的crate模块
crate::nation::government::govern();
// 相对路径 从 self 或 super 关键字或一个标识符开始描述
nation::government::govern();
```

#### 访问权限

> Rust 中有两种简单的访问权：公共（public）和私有（private）。
>
> 默认情况下，如果不加修饰符，模块中的成员访问权将是**私有的**,只有在与其平级的位置或下级的位置才能访问。
>
> 公共enum的所有变体都是公共的

```rust
mod back_of_house { // 文件 crate root不需要pub
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

### 模块拆分

> 在`lib.rs`中，及模块的根目录声明，然后同级创建同名的rs文件

```rust
mod font_module; // 目录的层级结构要与模块的结构一致
```



#### 模块引用

[**官方标准库**](https://doc.rust-lang.org/stable/std/all.html)

```rust
// 相当于import
mod second_module;
...
println!("{}", second_module::message());

// second_module.rs
pub fn message() -> String {
    String::from("This is the 2nd module.")
}

// use 关键字能够将模块标识符引入当前作用域：遵循私有性规则
mod nation {
    pub mod government {
        pub fn govern() {}
    }
    pub use government::governa;
    pub fn govern() {}
}
use std::{cmp::Ordering,io}; //引用相同库时，使用{}进行嵌套
use std::io::{self::Writer}; // 引入io及writer
use std::io::*; // 测试或预导入
pub use crate::nation::government::govern; // 外部应用可以使用 govern，重导出
use crate::nation::govern as nation_govern; // 针对同名使用本地别名
fn main() {
    nation::governa();
    nation_govern();
    govern();
}
```

## 异常处理

- 可靠性

  编译期提示

- 分类

  可恢复 `Result<T,E>`

  ​	文件未找到

  不可恢复`panic!`

  ​	使用场景

  	1. 演示某些概念`unwrap()`
  	2. 愿行代码 `unwrap()` `expect()`

```rust
// 常用
if let Ok(result) = function(params){
    println!("success result {}",result);
}else{
    // panic! 宏被调用时停止运行
    // 具体步骤
    // 1.打印错误日志 2.展开(unwind)、清理调用栈(stack) 3.退出程序
    panic!("error message");
}
```

- panic

  可能出现在代码，依赖的代码

  `set RUST_BACKTRACE=1`得到回溯信息

  ```toml
  [profile.release] # 生产环境时出现错误则中止
  panic="abort"
  ```

## Result枚举

```rust
    fn read_file()->Result<String,io::Error>{
        // 下面两条代码等同
        let mut f = File::open("hello.txt")?; // ?只能用于Result
        let mut f = match File::open("hello.txt") {
            Ok(file)=>file,
            Err(e)=>e,
        };
        let mut s = String::new();
        let s = f.read_to_string(&s)?;
         // 继续优化，代替上面所有代码
        File::open("hello.txt")?.read_to_string(&mut s)?;
        Ok(s)
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

## 依赖

- cargo.toml

  ```toml
  [dependencies]
  rand="0.5.5" # 包名=版本号
  ```

  


## 测试

### 并行/串行执行测试

cargo test 默认使用并行执行

并行执行试，各个测试案例之间不能互相依赖，不能影响共享的变量和依赖

`cargo test -- --test-threads=1` 指定测试线程数

`cargo test -- --showoutput` 成功时输出打印内容

`cargo test test_name` 通过测试名运行单个测试或包含`test_name`的测试

`#[ignore]`忽略测试 `cargo test -- --ignored` 运行被忽略的测试

[通过例子学 Rust]: https://www.bookstack.cn/read/rust-by-example-cn/

### 标准/错误输出

```rust
cargo run > output.txt // 标准输出到文件
println!(); // 标准输出
eprintln!(); // 错误输出
```

## 闭包

> 可以捕获所在环境的匿名函数

### 特性

1. 是一个匿名函数
2. 可以保存作为变量、参数
3. 可以在一个地方创建闭包，然后在另一个上下文中调用闭包完成运算
4. 可从其定义的作用域捕获值

### 示例

```rust
let res = |num|{
    1+num
};
// 手动加类型
let res = |num:i32|->i32{
    1-num
}
```

### 类型推断

1. 不要求标注参数及返回值类型
2. 通常代码短小，只在狭小的上下文工作，编译器能直接推断出类型
3. 可以手动添加类型

- 只能推断唯一类型，使用时确定
