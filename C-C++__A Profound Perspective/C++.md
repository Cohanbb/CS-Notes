# C++

- [C++](#c)
  - [关键字](#关键字)
  - [数据类型](#数据类型)
    - [基本数据类型](#基本数据类型)
    - [引用数据类型](#引用数据类型)
    - [带领域的枚举](#带领域的枚举)
    - [类类型](#类类型)
    - [类型转换](#类型转换)
  - [常量和变量](#常量和变量)
    - [初始化](#初始化)
    - [自动类型推断](#自动类型推断)
    - [new delete](#new-delete)
    - [const constexpr](#const-constexpr)
    - [nullptr 字面量](#nullptr-字面量)
    - [移动赋值和移动构造](#移动赋值和移动构造)
  - [表达式和语句](#表达式和语句)
    - [值类别](#值类别)
    - [代用运算符](#代用运算符)
    - [区间 for 循环](#区间-for-循环)
  - [函数](#函数)
    - [参数和返回值](#参数和返回值)
    - [内联函数](#内联函数)
    - [constexpr 函数](#constexpr-函数)
    - [函数重载](#函数重载)
  - [Lambda](#lambda)
  - [异常](#异常)
    - [throw](#throw)
    - [try catch](#try-catch)
    - [noexcept](#noexcept)
  - [命名空间](#命名空间)
  - [类和对象](#类和对象)
    - [C++ 结构体和共用体](#c-结构体和共用体)
    - [类](#类)
  - [继承和派生](#继承和派生)
    - [C++11 defaulted & deleted](#c11-defaulted--deleted)
    - [C++11 委托构造和继承构造](#c11-委托构造和继承构造)
  - [多态](#多态)
    - [override & final](#override--final)
  - [运算符重载](#运算符重载)
  - [I/O 类](#io-类)
  - [模版](#模版)
    - [函数模版](#函数模版)
    - [类模版](#类模版)
    - [C++11 模版别名](#c11-模版别名)
    - [C++11 可变参数模板](#c11-可变参数模板)
  - [标准模板库 STL](#标准模板库-stl)
    - [容器](#容器)
    - [算法](#算法)
    - [迭代器](#迭代器)
  - [参考文献](#参考文献)

C 语言是一个面向过程的编程语言，程序是纯粹的算法和数据结构的结合，但在实际的工程中存在一些问题：

* 缺乏抽象性和封装性，结构体内部无法封装函数
* 函数和运算符不能重载，难以扩展和维护
* 缺乏灵活性，代码难以复用

C++ 同样诞生于贝尔实验室，最初的目的是将 C 改良为『带类的 C』（然而现在远远不止如此），1983 年正式命名为 C++ (C Plus Plus)。1998 年，ANSI/ISO 发布了 C++ 标准以及标准模板库 STL。

于 2003 年对 C++98 进行了漏洞的修复，并于 2011 年、2014 年、2017 年和2020 年相继推出了新的 C++ 标准。

本文的内容基于 ANSI C 和 C++11，其中面向过程部分仅阐述了 C++11 相比于 ANSI C 的差异和新特性，如对 ANCI C 不熟悉，可阅读 [ANSI C](C.md)。如无特别说明，本文所有代码均使用 64 位 G++ 编译。

## 关键字

C++11 相比于 ANSI C 新增的关键字：

* and and_eq 等[代用运算符](#代用运算符)
* alignas alignof（C++11）（C11 提供了相似内容）
* bool true false [布尔数据类型](#基本数据类型)（C99 提供了相似的内容）
* throw try catch [异常处理](#异常)
* char16_t char32-t（C++11）[特殊的 char 类型](#基本数据类型)
* class [类](#类)
* constexpr（C++11）[常量表达式限定符](#const-constexpr)
* const_cast dynamic_cast 等[类型转换符](#类型转换)
* decltype（C++11）[类型指示符](#auto%20decltype)
* explicit 
* export
* friend 
* inline [内联函数](#内联函数)（C99 也加入）
* new delete [动态内存分配](#new-delete)
* namespace [命名空间](#命名空间)
* noexcept（C++11）[指定是否抛出异常](#noexcept)
* nullptr（C++11）[空指针](#nullptr-字面量)
* operator [运算符重载](#运算符重载)
* private public protected [访问说明符](#类)
* static_assert（C++11）
* template [模板](#模板)
* this [this 指针](#类)
* typeid typename
* virtual [虚函数](#多态)

## 数据类型

### 基本数据类型

[C 基本数据类型](C.md#基本数据类型)

1. `bool` 型

`bool` 型即布尔数据类型，值为 1 或 0，用来表示 `true` 或 `false`，占一个字节。与 C99 中的 `_Bool` 型相似。

* `bool` 型转 `int` 型，`true` 转化为 1，`false` 转化为 0。
* `int` 型转 `bool` 型，0 转化为 `false`，任何非零整数转化为 `true`（转化后值为 1）。

2. `long long int` 型（C99 也加入）

C++11 中引入了长长整数 `long long int`（简称 `long long`）以及 `unsigned long long`。标准规定该类型的存储位数不少于 `long int` 类型。经测试在 64 位 G++ 中 `long long` 的存储位数为 8，与 `long` 相同。在 32 位 G++ 下存储位数也为 8，大于 `long`。

  > 在 32 位计算机上 `long long` 如何占用 8 个字节参与运算？  
> 因为它在运算时使用两个通用寄存器存放，刚好 8 个字节。

3. `char16_t`、 `char32_t` 型

`wchar_t` 类型无法满足 Unicode 编码，故 C++11 引入了 `char16_t`、`char32_t` 型。

### 引用数据类型

C++ 中引入了引用数据类型，是一种复合数据类型，表示一个已定义的变量的别名。例如将变量 `b` 作为变量 `a` 的引用，则 `b` 和 `a` 所代表的是同一个内存地址上的数据。

引用变量的声明和初始化（声明时必须初始化）：

```cpp
// type &identifier = object;
int a;
int &b = a; // b 是 a 的引用
```

Demo：

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 1;
    int &b = a;
    cout << a++ << endl;
    cout << b << endl;
}
```

上述代码会依次输出 1 和 2，也就是说 `a` 与 `b` 表示的时同一个内存上的数据。

引用变量的本质是通过指针常量实现，所以一旦初始化后便无法改变其关联的地址：

```c
int a;
int &b = a;
int * const p = &a; // b 在本质上与 *p 相同
int c;
b = c;              // 等价于 *p = c
```

引用变量的应用场景主要是[函数传参和函数返回值](#函数)。

与指针变量一样，引用变量有十分严格的类型限制：

```c
float a = 1;
int b = a;  // Valid
int &c = a; // Invalid
int &d = 1; // Invalid
```

`const` 限定的引用，对引用的类型没有限制，只要该类型可以转换成引用的类型，可以关联字面量：

```cpp
int a = 1;
float b = 1.2;
const int &c = 1; // Valid
const int &d = a; // Valid
const int &e = b; // Valid
```

使用 `const` 限定引用，则不可通过该引用修改关联对象的值，一般用于函数传递参数，其效果与使用值传递一样，但效率更高。

C++11 右值引用

### 带领域的枚举

C++11 带领域的枚举（Scoped Enumatention）

### 类类型

C++ 可以实现面向对象编程，其核心是通过『类』实现，类可以通过 `class`、`struct` 和 `union` 创建。

具体内容在[类和对象](#类和对象)。

### 类型转换

C 语言中强制类型转换太过于松散和随意，安全性不够高且没有什么意义。C++ 对此做出了改进，引入了四种类型转换：

* dynamic_type
* const_cast
* static_cast
* reinterpret_cast

## 常量和变量

[C 常量和变量](C.md#常量和变量)

### 初始化

C++11 提供了五种初始化方式：

* 默认初始化：声明时不使用初始化器。
* 值初始化：使用了初始化器 `()` 或 `{}`（C++11），但初始化器是空的。
* `()` 直接初始化：使用初始化器 `()` 或 `{}`（C++11），且初始化器里有参数。
* 拷贝初始化：使用 `=` 进行初始化、函数值传递实参、函数返回一个值。
* 列表初始化：使用 `{}` 进行初始化。

`{}` 和 `()` 的区别：

* 使用 `{}` 初始化不允许『窄化』转换。
* 允许 `type identifier{}` 初始化器内部为空，进行值初始化。不允许 `type identifier()`，因为写法与函数声明冲突。
* `{}` 可以进行聚合类型（数组、特殊的类）的初始化，`()` 不可以。

对于基本数据类型：

* 如果是全局变量或静态变量，则默认初始化为零，否则初始化行为未知（不同编译器做法不同）。
* 值初始化进行零初始化，即初始化为 0。
* 直接初始化和拷贝初始化没有区别。
* 列表初始化不允许『窄化』转换。

Demo：

```cpp
int a0;                     // 默认初始化为 0

int main() {
    static int a1;          // 默认初始化为 0
    int a2;                 // 默认初始化为随机数
    int *a3 = new int;      // 默认初始化为随机数
    
    int a4{};               // 值初始化
    int *a5 = new int();    // 值初始化
    int a6 = int();         // 值初始化
    
    int a7(1);              // 直接初始化为 1
    int a8 = 1;             // 拷贝初始化为 1
    // int a9{1.0}; // 列表初始化不允许『窄化』转换
}
```

对于类类型：

* 如果是全局变量或者静态变量，先进行零初始化，再进行默认初始化，否则只进行默认初始化。默认初始化即调用默认构造函数。
* 值初始化与默认初始化没有区别。
* 直接初始化的本质是调用合适的构造函数。
* 拷贝初始化不能用于 `explicit` 修饰的构造函数。

Demo：

```cpp
#include <iostream>
using namespace std;

struct S {
    int a, b;

    // 默认构造函数
    S(): a(1), b(1) {
        cout << "调用默认构造函数" << endl;
    }
    
    // 拷贝构造函数
    S(const S &s): a(s.a), b(s.b) {
        cout << "调用拷贝构造函数" << endl;
    }

    // 构造函数 S(int, int = 1)
    S(int aa, int bb = 1): a(aa), b(bb) {
        cout << "调用构造函数 S(int, int = 1)" << endl;
    }
    
    // 重载 = 运算符
    S & operator = (const S &s) {
        cout << "使用赋值运算符" << endl;
        return *this;
    }

};

S s0;                       // 先零初始化，再默认初始化

int main() {
    S s1;                   // 默认初始化，隐式调用默认构造函数

    S s2{};                 // 值初始化，也是列表初始化，隐式调用默认构造函数
    S *s4 = new S();        // 值初始化，隐式调用默认构造函数
    S s3 = S();             // 值初始化，显式调用默认构造函数
    
    S s5(2, 2);             // 直接初始化，隐式调用构造函数 S(int, int)
    S *s6 = new S(2, 2);    // 直接初始化，隐式调用构造函数 S(int, int)
    S s7 = S(2, 2);         // 直接初始化，显式调用构造函数 S(int, int)
    S s8(s7);               // 直接初始化，隐式调用拷贝构造函数
    S s9 = S(s8);           // 直接初始化，显式调用拷贝构造函数
    S *s10 = new S(s9);     // 直接初始化，隐式调用拷贝构造函数
 
    S s11 = s9;             // 拷贝初始化，隐式调用拷贝构造函数
    S s12 = 3;              // 拷贝初始化，隐式调用构造函数 S(int, int)

    S s13{4};               // 列表初始化，隐式调用构造函数 S(int, int)
    S s14{4, 4};            // 列表初始化，隐式调用构造函数 S(int, int)
    S *s15 = new S{4, 4};   // 列表初始化，隐式调用构造函数 S(int, int)
    S s16{s14};             // 列表初始化，隐式调用拷贝构造函数
    // S s17{4.0, 4.0}; // 列表初始化，不允许『窄化』

    delete s4;
    delete s6;
    delete s10;
    delete s15;
    return 0;
}
```

对于数组：

* 如果进行默认初始化，则对数组的每个元素进行默认初始化，数组元素可能是基本数据类型，也可能是类类型，默认初始化规则为上文描述的规则。
* 如果进行值初始化，则对数组的每个元素进行值初始化。
* 无直接初始化，只能使用列表初始化。
* 除字符数组外无拷贝初始化。

Demo：

```cpp
int main() {
    int a[5]{1, 2, 3};
    string s[3]{"123", "456", "789"};
    string s[3]{{3}};
}
```

### 自动类型推断

C++11 允许声明一个变量或对象而不需要声明数据类型，只需要在前面声明 `auto`，并进行初始化，其类型会根据初始值被自动推断出来。

```cpp
// auto identifier = value;
// 根据 value 自动推导数据类型
auto a = 100;   // a 为 int 型
float b;
auto c = b;     // b 为 float 型
```

使用 `auto` 的注意事项：

* 在一个语句中声明多个变量或对象，这些变量或对象的数据类型必须一样。

```cpp
auto a = 1, b = 1.5;    // Invalid
```

* 无法推导为 `const` 类型

```cpp
const int a = 1;
auto b = a; // b 被推导为 int 型变量，而不是常量
```

* 无法推导为[引用数据类型](#引用数据类型)

```cpp
int a = 1, &b = a;
auto c = b; // c 被推导为 int 类型，而不是 int &
```

由于 `auto` 功能的限制，因此 C++11 又引入了 `decltype` 类型指示符。

```cpp
// decltype(statement) identifier = value; 
// 根据 statement 来设置数据类型
// statement 可以是变量、常量、表达式、函数名
const int a = 1, &b = a, *c = &a;
decltype(a) d = 1;      // d 为 const int 型
decltype(b) e = 1;      // e 为 const int & 型
decltype(c) f = 1;      // f 为 const int * 型

decltype(expr) g = 1;   // g 的类型为 expr 的值的类型
decltype(func) h = 1;   // h 的类型为 func 返回值的类型
```

### new delete

[C 动态内存管理](C.md#动态内存管理)

C++ 新增了 `new` 和 `delete` 关键字用于动态内存的创建和销毁，对应的功能就像 C 语言中的 `malloc` 和 `free`。

```cpp
// 动态创建一个 int 型指针 
int *p = new int;
int *p = (int *)malloc(sizeof(int));

// 动态创建一个一维数组
int *arr = new int [3];
int *arr = (int *)malloc(sizeof(int[3]));

// 动态创建一个多维数组
int (*arr2)[3] = new int[2][3];
int (*arr2)[3] = (int (*)[3])malloc(sizeof(int[2][3]));

// 释放分配的内存
delete p;
free(p);
delete[] arr;
free(arr);
delete[] arr2;
free(arr2);
```

与使用 `malloc` 和 `free` 不同，使用 `new` 和 `delete` 可以对创建的对象进行[初始化](#初始化)：

```cpp
int *p = new int(1);
int *q = new int[3]{1, 2, 3};
```

### const constexpr

C++ 中『常量表达式』具有非凡的意义，它在编译过程中即被计算出来，节省了程序运行时内存访问的开销。

常量表达式即由常量、字面量构成的表达式，以代码段中的立即数或常量区数据的形式存在。

C 语言中，字符串字面量和使用 `const` 限定的全局/静态变量是常量，存储在常量段。但使用 `const` 修饰的局部变量是『只读的变量』即『常变量』，譬如：

```c
const int N1 = 10;

int main() {
    const int N2 = 10;
    int a = N1;
    int b = N2;
}
```

它的汇编代码是：

```asm
    movl	$10, -12(%rbp)      # 10->N2
    movl	$10, -8(%rbp)       # N1->a，N1 等价于为 10
    movl	-12(%rbp), %eax     # N2->eax
    movl	%eax, -4(%rbp)      # eax->b
```

可看出 `N1` 是常量，被替换成了立即数 10。而 `N2` 是常变量，依然存储在栈区，它的值需要程序运行时访问内存寻址才能确定，加大了内存开销。

所以 `const` 的功能在 C 语言中十分的鸡肋。

C++ 对此作出了改进，虽然 `N2` 也存储在栈区，但是在编译过程中便把 `N2` 的值计算出来，把 `N2` 替换为了立即数 10，不需要访问内存进行寻址，其汇编代码为：

```asm
    movl	$10, -12(%rbp)      # 10->N2
    movl	$10, -8(%rbp)       # 10->a
    movl	$10, -4(%rbp)       # 10->b
```

同理使用 `const` 限定的静态局部变量也会被直接替换为立即数。

C++ 建议使用 `const` 的方式设置常量，而不用 `#define`。

在 C++ 中 `const` 限定的变量必须进行初始化，可以初始化为常量表达式，也可以初始化为非常量表达式：

```cpp
int a;
const int b = 1;    // 1 为常量表达式
const int c = a;    // a 不是常量表达式
```

C++11 引入了新的常量表达式的限定符 `constexpr`，通过此关键字限定常量的内容必须是表达式：

```cpp
int a;
constexpr int b = 1;    // Valid
constexpr int c = b;    // Valid
constexpr int d = a;    // Invalid，a 不是常量表达式
```

使用 `constexpr` 限定的指针等价于指针常量，即指针的值不能改变，而指针所指地址的内容可以改变，且在 G++ 编译器下 `constexpr` 指针只能指向静态存储区（全局变量、静态变量以及字符串字面量）的内容，MSVC 对此并无要求：

```cpp
static int a = 0;
int b = 1;
constexpr int *p = &a;  // 等价与 int * const p = &a;
*p = b;                 // Valid
p = &b;                 // Invalid
```

### nullptr 字面量

空指针表示不指向任何内容的指针，在 C 语言中空指针的定义：

```c
#define NULL ((void *)0)
```

在 C++11 之前空指针的定义：

```cpp
#define NULL 0
```

所以当一个空指针作为函数参数时会被认为是一个 `int` 型参数，譬如：

```cpp
void demo(char *p);
void demo(int a);

int main() {
    demo(NULL);
}
```

`demo()` 函数进行了重载，当传入实参为空指针 `NULL`，调用的函数为 `demo(int)` 而非 `demo(char *)`。

C++11 新增了 `nullptr` 字面量用来表示一个空指针，且不会被转换成整数类型（除了布尔型）。

在 cstddef 头文件中定义了 `std::nullptr_t` 类型：

```cpp
typedef decltype(nullptr) nullptr_t;
```

经测试在 64 位 G++ 编译器下占 8 个字节。

### 移动赋值和移动构造

## 表达式和语句

### 值类别

C 语言中值类别分为左值、右值和函数，C++11 对此进行了更加详细和和准确的划分。

### 代用运算符

### 区间 for 循环

C++11 引入了一种新的 `for` 循环，可以逐一迭代某个给定的范围（如数组）内的每一个元素。

```cpp
// for (declare : range) {
//     operation
// }
for (int i : {1, 2, 3, 4, 5}) {
    std::cout << i << std::endl;
}
```

## 函数

[C 函数](C.md#函数)

C++ 在 C 语言基础上对函数进行了改进和扩展，提高了函数的安全性、便捷性和可扩展性。

### 参数和返回值

1. 函数参数列表

C 语言在声明函数原型时可以不指出参数列表，而 C++ 不可，但可以使用 `...` 省略。

```cpp
void f(...);

void f(int a, int b) {
    std::cout << a + b << std::endl;
}
```

2. 函数参数默认值

C 语言中函数参数不可以设置默认值，C++ 可以设置参数的默认值，如果参数缺省，则使用默认值。

使用默认值的规则是：在函数声明（或直接在定义时）时从右至左逐个设置默认值，如：

```cpp
int f(int a = 1, int b = 2, int c =3);  // 或 int f(int = 1, int = 2, int = 3);
int f(int a, int b = 2, int c =3);      // 或 int f(int, int = 2, int =3);
int f(int a, int b = 2, int c);         // Invalid

int f(int a, int b, int c) {            // 或在定义时设置默认值设置
    return a + b + c;
}

int main() {
    int n = f();
}
```

3. 函数返回值缺省

C 语言中函数的返回值可以缺省，默认返回 `int` 型，但在 C++ 中不可缺省，必须规定返回值类型。

4. 引用作函数参数和返回值

因为引用的本质是指针，所以使用引用传递参数类似于使用地址传递。

Demo：

```cpp
void fun1(int x, int y) {   // 值传递
    ++x;
    ++y;
}

void fun2(int &x, int &y) { // 引用传递
    ++x;
    ++y;
}

void fun3(int *x, int *y) { // 地址传递
    ++(*x);
    ++(*y);
}

int main() {
    int x, y;
    x = 1;
    y = 2;
    fun1(x, y);
    fun2(x, y);
    fun3(&x, &y);
}
```

编译程序，使用 GDB 反汇编：

```asm
(gdb) disas main
Dump of assembler code for function main():
   0x00000000000011c4 <+0>:	    endbr64 
   0x00000000000011c8 <+4>:	    push   %rbp
   0x00000000000011c9 <+5>:	    mov    %rsp,%rbp
   0x00000000000011cc <+8>:     sub    $0x10,%rsp
   0x00000000000011d0 <+12>:	mov    %fs:0x28,%rax
   0x00000000000011d9 <+21>:	mov    %rax,-0x8(%rbp)
   0x00000000000011dd <+25>:	xor    %eax,%eax
   0x00000000000011df <+27>:	movl   $0x1,-0x10(%rbp)
   0x00000000000011e6 <+34>:	movl   $0x2,-0xc(%rbp)
   0x00000000000011ed <+41>:	mov    -0xc(%rbp),%edx
   0x00000000000011f0 <+44>:	mov    -0x10(%rbp),%eax
   0x00000000000011f3 <+47>:	mov    %edx,%esi
   0x00000000000011f5 <+49>:	mov    %eax,%edi
   0x00000000000011f7 <+51>:	callq  0x1149 <fun1(int, int)>
   0x00000000000011fc <+56>:	lea    -0xc(%rbp),%rdx
   0x0000000000001200 <+60>:	lea    -0x10(%rbp),%rax
   0x0000000000001204 <+64>:	mov    %rdx,%rsi
   0x0000000000001207 <+67>:	mov    %rax,%rdi
   0x000000000000120a <+70>:	callq  0x1162 <fun2(int&, int&)>
   0x000000000000120f <+75>:	lea    -0xc(%rbp),%rdx
   0x0000000000001213 <+79>:	lea    -0x10(%rbp),%rax
   0x0000000000001217 <+83>:	mov    %rdx,%rsi
   0x000000000000121a <+86>:	mov    %rax,%rdi
   0x000000000000121d <+89>:	callq  0x1193 <fun3(int*, int*)>
   0x0000000000001222 <+94>:	mov    $0x0,%eax
   0x0000000000001227 <+99>:	mov    -0x8(%rbp),%rcx
   0x000000000000122b <+103>:	xor    %fs:0x28,%rcx
   0x0000000000001234 <+112>:	je     0x123b <main()+119>
   0x0000000000001236 <+114>:	callq  0x1050 <__stack_chk_fail@plt>
   0x000000000000123b <+119>:	leaveq 
   0x000000000000123c <+120>:	retq   
End of assembler dump.
(gdb) disas fun1
Dump of assembler code for function fun1(int, int):
   0x0000000000001149 <+0>:	    endbr64 
   0x000000000000114d <+4>:	    push   %rbp
   0x000000000000114e <+5>: 	mov    %rsp,%rbp
   0x0000000000001151 <+8>:	    mov    %edi,-0x4(%rbp)
   0x0000000000001154 <+11>:	mov    %esi,-0x8(%rbp)
   0x0000000000001157 <+14>:	addl   $0x1,-0x4(%rbp)
   0x000000000000115b <+18>:	addl   $0x1,-0x8(%rbp)
   0x000000000000115f <+22>:	nop
   0x0000000000001160 <+23>:	pop    %rbp
   0x0000000000001161 <+24>:	retq   
End of assembler dump.
(gdb) disas fun2
Dump of assembler code for function fun2(int&, int&):
   0x0000000000001162 <+0>:	    endbr64 
   0x0000000000001166 <+4>:	    push   %rbp
   0x0000000000001167 <+5>:	    mov    %rsp,%rbp
   0x000000000000116a <+8>:	    mov    %rdi,-0x8(%rbp)
   0x000000000000116e <+12>:	mov    %rsi,-0x10(%rbp)
   0x0000000000001172 <+16>:	mov    -0x8(%rbp),%rax
   0x0000000000001176 <+20>:	mov    (%rax),%eax
   0x0000000000001178 <+22>:	lea    0x1(%rax),%edx
   0x000000000000117b <+25>:	mov    -0x8(%rbp),%rax
   0x000000000000117f <+29>:	mov    %edx,(%rax)
   0x0000000000001181 <+31>:	mov    -0x10(%rbp),%rax
   0x0000000000001185 <+35>:	mov    (%rax),%eax
   0x0000000000001187 <+37>:	lea    0x1(%rax),%edx
   0x000000000000118a <+40>:	mov    -0x10(%rbp),%rax
   0x000000000000118e <+44>:	mov    %edx,(%rax)
   0x0000000000001190 <+46>:	nop
   0x0000000000001191 <+47>:	pop    %rbp
   0x0000000000001192 <+48>:	retq   
End of assembler dump.
(gdb) disas fun3
Dump of assembler code for function fun3(int*, int*):
   0x0000000000001193 <+0>:	    endbr64 
   0x0000000000001197 <+4>:	    push   %rbp
   0x0000000000001198 <+5>:	    mov    %rsp,%rbp
   0x000000000000119b <+8>:	    mov    %rdi,-0x8(%rbp)
   0x000000000000119f <+12>:	mov    %rsi,-0x10(%rbp)
   0x00000000000011a3 <+16>:	mov    -0x8(%rbp),%rax
   0x00000000000011a7 <+20>:	mov    (%rax),%eax
   0x00000000000011a9 <+22>:	lea    0x1(%rax),%edx
   0x00000000000011ac <+25>:	mov    -0x8(%rbp),%rax
   0x00000000000011b0 <+29>:	mov    %edx,(%rax)
   0x00000000000011b2 <+31>:	mov    -0x10(%rbp),%rax
   0x00000000000011b6 <+35>:	mov    (%rax),%eax
   0x00000000000011b8 <+37>:	lea    0x1(%rax),%edx
   0x00000000000011bb <+40>:	mov    -0x10(%rbp),%rax
   0x00000000000011bf <+44>:	mov    %edx,(%rax)
   0x00000000000011c1 <+46>:	nop
   0x00000000000011c2 <+47>:	pop    %rbp
   0x00000000000011c3 <+48>:	retq   
End of assembler dump.
```

可以看出使用引用传递的 `fun2()` 与使用地址传递的 `fun3()` 调用过程以及函数的栈帧完全相同，使用 `lea` 指令直接传递实参的地址。而使用值传递的 `fun1()` 则是使用 `mov` 指令传递实参的数值。

引用传递在使用上与值传递由差异，依然以上述代码为例：

```cpp
fun1(1, 2);         // Valid
fun1(x + 1, y + 1); // Valid
fun2(1, 2);         // Invalid
fun2(x + 1, y + 1); // Invalid
```

对于值传递而言显然可以直接使用常量或右值表达式作为实参，但对与引用传递而言不可以，除非使用 `const` 限定的引用。

引用作为返回值与指针作为返回值一样，一般用来返回外部变量的引用，可以当作左值使用。

注意：与使用指针作为返回值一样，使用引用作为返回值，不能返回在函数内部定义的局部变量，因为在函数调用结束后其栈帧就被销毁。

5. C++11 返回值后置

### 内联函数

内联函数，在函数定义时前面加上 `inline` 即表明该函数为内联函数（C99 也加入）。内联函数在调用时直接把函数的代码在调用的位置『展开』，而不用进行栈帧的跳转和返回。

即使将一个函数定义为内联函数，编译器也未必会接受，如果定义的内联函数体过大，则编译器会把它当作正常函数。

C++ 中建议使用内联函数，而不是宏定义。

### constexpr 函数

`constexpr` 限定的函数，其本质也是内联函数。在 C++11 标准下的 `constexpr` 函数要求非常的严格：

* 内部只能有 `return` 语句以及不产生实际操作的语句（如 `using`、`typedef` 等）
* 函数的返回值和形参必须是字面量。

```cpp
constexpr int f(int a, int b) {
    return a * b;
}

int f2(int a, int b) {
    return a * b;
}

int main() {
    int a = f(1, 2);
    int b = f2(1, 2);
    return 0;
}
```

反汇编：

```asm
9       int main() {
   0x0000000000001140 <+0>:     endbr64 
   0x0000000000001144 <+4>:     push   %rbp
   0x0000000000001145 <+5>:     mov    %rsp,%rbp
   0x0000000000001148 <+8>:     sub    $0x10,%rsp

10          int a = f(1, 2);
   0x000000000000114c <+12>:    movl   $0x2,-0x8(%rbp)

11          int b = f2(1, 2);
   0x0000000000001153 <+19>:    mov    $0x2,%esi
   0x0000000000001158 <+24>:    mov    $0x1,%edi
   0x000000000000115d <+29>:    callq  0x1129 <f2(int, int)>
   0x0000000000001162 <+34>:    mov    %eax,-0x4(%rbp)

12          return 0;
   0x0000000000001165 <+37>:    mov    $0x0,%eax

13      }
```

可见 `constexpr` 函数在编译过程中已经计算出了一个常量结果，不需要函数的调用和返回。

### 函数重载

C 语言不支持函数的重载，C++ 支持。

## Lambda

## 异常

### throw

### try catch

### noexcept

C++11 中引入了一个关键词 `noexcept` 用来指明某个函数无法抛出异常。


## 命名空间

```cpp
namespace {

}
```

## 类和对象

### C++ 结构体和共用体

C++11 非受限的共用体 union

### 类

```cpp
// class class_identifier {
//     member_list    
//     member_functions
// };
class Student {
    private int age;
    public int age;
    protected int age;
    
    protected void func();
    
    Student();
    
    ~Student();
};
```

默认构造函数：

C++11 标准中，

## 继承和派生

### C++11 defaulted & deleted

### C++11 委托构造和继承构造


## 多态

### override & final


## 运算符重载

## I/O 类

## 模版

### 函数模版

### 类模版

tuple

bitset

C++11 initializer_list

C++11 function

### C++11 模版别名

### C++11 可变参数模板

## 标准模板库 STL

### 容器

Array

String

Vector

Deque

List

Forword List

Set & Multiset

Map & Multimap

Stack

Queue

Priority Queue

### 算法

### 迭代器

## 参考文献

* cppreference. C++ reference[EB/OL]. [https://en.cppreference.com/w/](https://en.cppreference.com/w/)  
* C++ Primer  
* C++ Primer Plus  
