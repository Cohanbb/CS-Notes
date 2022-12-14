# C

- [C](#c)
  - [简介](#简介)
    - [发展历程](#发展历程)
    - [编译和链接](#编译和链接)
  - [基本数据类型 常量和变量](#基本数据类型-常量和变量)
    - [基本数据类型](#基本数据类型)
    - [C99 新增数据类型](#c99-新增数据类型)
    - [常量和变量](#常量和变量)
    - [类型转换](#类型转换)
  - [表达式和语句](#表达式和语句)
    - [值类别](#值类别)
    - [运算符](#运算符)
    - [优先级和结合性](#优先级和结合性)
    - [语句结构](#语句结构)
  - [复合数据类型](#复合数据类型)
    - [指针](#指针)
    - [一维数组](#一维数组)
    - [C99 变长数组](#c99-变长数组)
    - [多维数组](#多维数组)
    - [C-风格字符串](#c-风格字符串)
    - [指针数组和数组指针](#指针数组和数组指针)
    - [枚举类型](#枚举类型)
    - [结构体类型](#结构体类型)
    - [共用体类型](#共用体类型)
    - [不完全数据类型](#不完全数据类型)
    - [mutable volatile restrict](#mutable-volatile-restrict)
    - [typedef](#typedef)
  - [预处理](#预处理)
    - [宏定义](#宏定义)
    - [条件编译](#条件编译)
    - [文件包含](#文件包含)
  - [函数](#函数)
    - [函数返回值](#函数返回值)
    - [函数参数](#函数参数)
    - [主函数](#主函数)
    - [函数调用栈帧](#函数调用栈帧)
    - [函数指针](#函数指针)
  - [I/O](#io)
    - [标准 I/O 和文件 I/O](#标准-io-和文件-io)
    - [标准 I/O 库函数](#标准-io-库函数)
    - [延伸：POSIX I/O](#延伸posix-io)
  - [内存管理](#内存管理)
    - [内存组织方式](#内存组织方式)
    - [动态内存管理](#动态内存管理)
    - [缓冲区溢出](#缓冲区溢出)
  - [C 标准库及常用库函数](#c-标准库及常用库函数)
  - [参考文献](#参考文献)

## 简介

### 发展历程

C 语言是一个系统级的高级程序设计语言，是需要编译执行的静态语言，广泛应用于操作系统、驱动程序、嵌入式软件、系统级应用和游戏的开发，C 语言是最接近底层的高级语言。

C 语言发展历程：

* C 语言诞生于美国贝尔实验室，1989 年美国国家标准协会（ANSI）发布了第一个 C 语言标准，简称 C89 或 ANSI C。
* 1990 年 ANSI C 标准被国际标准化组织（ISO）和国际电工委员会（IEC）采纳为国际标准，简称 C90。
* 1994、1996 年 ISO 发布了对 C90 的勘误和补充，简称 C95。
* 1999 年 ISO 与 IEC 采纳了重大修订的第二个的 C 语言标准，简称 C99。
* 2011 年 ISO 与 IEC 采纳了第三个 C 语言标准，简称 C11。
* 2018 年又采纳了最新的 C 语言标准，简称 C17 或 C 18。

C89 是影响力最为深远、编译器支持度最好的 C 语言标准，而 C99 标准至今仍没有得到部分编译器的完整支持，对 C99 标准支持度最好的编译器是 GCC 和 Clang。

本文的内容基于 C89、C99 和部分 C11 标准，所有代码均使用 64 位 GCC 编译。

### 编译和链接

C 语言的编译完整过程分为两大部分：『编译』和『链接』。编译部分通过编译器完成，链接部分通过操作系统的链接器完成。

* 编译部分需要完成的操作有：预处理、编译和汇编。
* 链接部分需要完成的操作有：链接。

在 UNIX/Linux 系统上 C 语言编译和链接的过程：

1. 预处理

    对源程序进行预处理，预处理器对源文件进行文件包含、宏的替换和去除注释等处理，生成一个真实的、完整的 C 语言程序（ASCII 码文件）。

    **.c** $\longrightarrow$ **.i**

2. 编译

    编译器对于程序进行词法分析、语法分析和语义分析，根据分析结果进行代码优化，并生成一个符号汇总表，包含函数名和[静态存储区](#内存组织方式)的相关信息。将 C 语言程序编译成汇编语言程序（ASCII 码文件）。

    **.i** $\longrightarrow$ **.s**（Windows **.c** $\longrightarrow$ **.asm**）

3. 汇编

    汇编器将汇编程序转换为机器指令，生成可重定向的二进制目标文件，记录目标文件中所用到的符号并设定符号的值（地址等信息）。目标文件可以链接库文件生成可执行文件，也可以生成静态链接库（.a | Windows .lib）、动态链接库（.so | Windows .dll）。

    **.s** $\longrightarrow$ **.o**（Windows **.asm** $\longrightarrow$ **.obj**）

4. 链接

    由操作系统的链接器完成，链接库文件和目标文件，通过符号表的合并和重定向修补内部引用和外部引用等一系列复杂的操作，最终生成可执行文件。其中静态链接库被并入可执行文件，而动态链接库则在可执行文件执行时由运行时（runtime）加载，GCC 默认使用动态链接库。

    **.o** $\longrightarrow$ **.out**（Windows **.obj** $\longrightarrow$ **.exe**）

## 基本数据类型 常量和变量

C 语言中数据类型分为三大类：

1. 对象（object）类型：在内存中创建的具有完整定义的类型。
2. 函数（function）类型：所定义的函数均为函数类型。
3. 不完全（imcomplete）类型：`void` 型、未指定长度的数组、未定义内容的结构体和共用体。

其中对象类型又分为基本数据类型和复合数据类型。

### 基本数据类型

C 语言的基本数据类型分为两大类：

* 以**整数**形式存储的数据类型：

  * 字符型 `char`、整数型 `int`、短整数型 `short int`（简称 `short`）、长整数型 `long int`（简称 `long`）。
  * 有符号字符型 `signed char`、无符号字符型 `unsigned char`、无符号短整数型 `unsigned short`、无符号整数型 `unsigned int`、无符号长整数型 `unsigned long`。

  > 为什么会有 `signed` 这个关键字？除了 `char` 之外的整数型都默认为有符号型，而 `char` 根据编译器的不同可能是有符号型，也可能是无符号型。所以引入了 `signed char`。 

* 以**浮点数**形式存储的数据类型：

  单精度浮点数型 `float`、双精度浮点数型 `double`、长双精度浮点数型 `long double`。
  
  > 注：浮点数在计算机中存储的实际值未必等于（大概率不等于）准确值。

在 32 位或 64 位编译器下：

|类型|存储字节|范围|
|--|--|--|
| int | 4 | [-2<sup>31</sup>, 2<sup>31</sup> - 1] |
| unsigned int | 4 | [0, 2<sup>32</sup> - 1] |
| short | 2 | [-2<sup>15</sup>, 2<sup>15</sup> - 1] |
| unsigned short | 2 | [0, 2<sup>16</sup> - 1] |
| long | 32 位 4、64 位 8 | [-2<sup>31</sup>, 2<sup>31</sup> - 1] 或 [-2<sup>63</sup>, 2<sup>63</sup> - 1] |
| unsigned long | 32 位 4、64 位 8| [0, 2<sup>32</sup> - 1] 或 [0, 2<sup>64</sup> - 1] |
| char | 1 | [-128, 127] 或 [0, 255] |
| unsigned char | 1 | [0, 255] |
| signed char | 1 | [-128, 127] |
| float | 4 | [1.2E-38, 3.4E+38] 精度 6 ~ 7|
| double | 8 | [2.3E-308, 1.7E+308] 精度 15 ~ 16|
| long double | 8 或 12 或 16| 略|

这些数据类型是如何存储的？为什么不同数据类型的存储字节数不同？请学习[数据的表示与运算](../System/数据的表示与运算.md)。

### C99 新增数据类型

1. C95 补充了宽字符类型以及头文件 wchar_t.h，通过 `typedef unsigned int wchar_t` 实现，并不是一个独立的类型。而 C++ 中 `wchar_t` 是一个基本数据类型。

2. C89 没有布尔数据类型，C99 引入了 `_Bool` 类型，占 1 个字节，值为 1 或 0，表示真或假。在新增头文件 stdbool.h 中定义了宏 `bool`、`true` 和 `false`。

```c
_Bool a1 = 0;
_Bool b1 = 1; 
_Bool c1 = 2;       // 只要赋值不为 0 则赋值为 1

#include <stdbool.h>
bool a2 = false;    // bool 定义为 _Bool，false 定义为 0
bool b2 = true;     // true 定义为 1
```

3. 长长整数型 `long long int`（简称 `long long`）以及 `unsigned long long int`（简称 `unsigned long long`）。C99 规定 `long long` 的范围不小于 `long`，经测试在 64 位 GCC 中 `long long` 的存储位数为 8，与 `long` 相同。在 32 位 GCC 下存储位数也为 8，大于 `long`。

> 在 32 位计算机上 `long long` 如何占用 8 个字节参与运算？  
> 因为它在运算时使用两个通用寄存器存放，刚好 8 个字节。

  考虑到不同平台下 `int`、`long` 和 `long long` 所占的位数不统一，故 C99 引入了头文件 stdint.h，里面包含了由 `typedef` 定义的整数型，如 `int8_t`、`int16_t`、`int32_t`、`int63_t` 分别表示 8、16、32、64 位整数型，`uint8_t` ......  表示无符号的整数型。

4. 复数类型 `_Complex` 以及虚数类型 `_Imaginary`，在头文件 complex.h 中引入，在此不多介绍。

### 常量和变量

程序中值可以发生改变的数据和不可发生改变的数据称为『变量』和『常量』。

1. 常量

真正的常量在编译过程就可以确定数值，其本质是字面量，直接使用数值表示的数据称为字面量。体现为[二进制代码段](#内存组织方式)的『立即数』或存储在『[.rodata 常量段](#内存组织方式)的数据』（字符串字面量和 `const` 限定的全局/静态变量）。

字面量的表示方法：

```c
/* 整数字面量 */
100;        // (const int)100
100u;       // (const unsigned int)100
100U;       // 同上
100l;       // (const long int)100
100L;       // 同上
100ul;      // (const unsigned long int)100
0144;       // 八进制 144 即十进制 100，(const int)100
0x64;       // 十六进制 64 即十进制 100，(const int)100
0X64;       // 同上

/* 字符型字面量 */
'a';        // 单引号表示字符，字符 a，(const int)97
97;         // 字符 a 的 ASCII 码，(const int)97
L'啊';      // 宽字符 wchar_t 型，本质是 unsigned int

/* 浮点数字面量 */
3.1415;     // 小数 3.1415，(const double)3.1415
31415E-5;   // 科学计数法 3.1415，(const double)3.1415
31415e-5;   // 同上
3.1415l;    // (const long double)3.1415
3.1415L;    // 同上

/* 字符串字面量 */
"hello"     // 双引号表示字符串，hello\0，字符串都以 '\0' 结尾，(const char *) 型
```

在 C99 标准中：

* 可以使用十六进制表示浮点数字面量：

```c
0x1.2p3;    // 十六进制 1.2 乘 2^3，即十进制 9.0
0x1.2P3;    // 同上
```

* 新增了复合字面量，用来表示复合数据类型的常量。看起来就像强制类型转换，但强制类型转换是右值表达式，复合字面量是[左值表达式](#值类别)。

```c
/**
 * (type){initializer_list}
 * (type){initializer_list,}
 */
(int []){1, 2, 3};  // 可看作匿名数组 {1, 2, 3}
(int [3]){1, 2, 3}; // 同上

int *p = (int []){1, 2, 3}; // 等价于 int *p = array_identifier； 
char *str = (chat []){"HelloWorld!"};
```

ASCII 码表

![](image/Pasted%20image%2020220718021209.png)

转义字符:

|转义字符|意义|ASCII码值|
|-|-|-|
|\\a|响铃（BEL）|007|
|\\b|退格（BS），将当前位置移到前一列|008|
|\\f|换页（FF），将当前位置移到下页开头|012|
|\\n|换行（LF），将当前位置移到下一行开头|010
|\\r|回车（CR），将当前位置移到本行开头|013|
|\\t|水平制表（HT），跳到下一个 TAB 位置|009|
|\\v|垂直制表（VT）|011|
|\\\\|代表一个反斜线字符 '\\'|092|
|\\'|代表一个单引号字符|039|
|\\"|代表一个双引号字符|034|
|\\?|代表一个问号|063|
|\\0|空字符（NULL），所有字符串都以 \0 结尾|000|
|\\ddd|八进制数所代表的任意字符|1～3 位八进制|
|\\xhh|十六进制所代表的任意字符|1~2 位十六进制|

符号常量：

就是使用一个标识符来表示的常量，其定义方式有两种：

```c
/**
 * 1.使用 #define
 * #define constant_identifier value
 */
#define LENGTH 100

/**
 * 2.使用 const
 * const type constant_identifier = value;
 */
const int WIDTH = 50;
```

在 C 语言中：

* 使用 `#define` 定义的是真正的常量，不会占用内存，因为早在预处理阶段就全部被替换成字面量了。
* 使用 `const` 限定的全局/静态变量是真正的常量，存储在 [.rodata 常量段](#内存组织方式)，占用内存，但在编译过程就计算出了值，而不需要程序运行时访问内存寻址，节省了内存开销。
* 使用 `const` 限定的局部变量不是真正的常量，而是『常变量』，或者说『只读的变量』，它存储在栈区而不是常量段，需要在程序运行时访问内存寻址才能确定它的值。

C 语言中使用 `const` 可以不进行初始化，但是之后也无法改变变量的值，所以最好还是初始化。

Demo：

```c
#define A 10
const int B = 20;

int main() {
    static const int C = 30;
    const int D = 40;
    int a = A;
    int b = B;
    int c = C;
    int d = D;
}
```

使用 GCC 编译生成汇编代码，截取部分代码：

```asm
    .globl	B
	.section	.rodata
	.align 4
	.type	B, @object
	.size	B, 4
B:
	.long	20
	.text
	.globl	main
	.type	main, @function
main:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	movq	%rsp, %rbp
	.cfi_def_cfa_register 6
	movl	$40, -20(%rbp)
	movl	$10, -16(%rbp)
	movl	$20, -12(%rbp)
	movl	C.1913(%rip), %eax
	movl	%eax, -8(%rbp)
	movl	-20(%rbp), %eax
	movl	%eax, -4(%rbp)
	movl	$0, %eax
	popq	%rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE0:
	.size	main, .-main
	.section	.rodata
	.align 4
	.type	C.1913, @object
	.size	C.1913, 4
C.1913:
	.long	30
```

分析：

* `A` 在预编译阶段就已经被替换成了字面量 10，`movl	$10, -12(%rbp)` 表示代码段中直接使用立即数表示 `A`。
* `B` 存储在 .rodata 常量段，编译时确定了值为 20，`movl	$20, -8(%rbp)` 表示代码段的 `B` 被替换为立即数 20，无需访问内存寻址。
* `C` 也存储在 .rodata 常量段，编译过程中即确定了值为 30，但在代码段中未被替换成立即数，而是替换成了 `C` 的符号 `C.1913`。
* `D` 虽然被 `const` 修饰，但依然存储在栈上，依然需要访问 `C` 的内存寻址才能取得它的值。

既然使用 `const` 修饰的变量是『常变量』，地址在栈上，所以使用指针是可以修改它的值的：

```c
const int a = 10;
int *p = (int *)&a;
*p = 20; // a = 20
```

2. 变量

* 变量的作用域：变量可以被使用的范围，如果在外部定义，则作用域为全局，是全局（global）变量。如果在函数或复合语句内定义，则作用域为块作用域，是局部（local）变量。

* 变量的生存期：变量从在内存中创建到被销毁的存留时间。

变量的声明和定义：

```c
/**
 * 变量的声明：
 * storage_class type variable_identifier;
 * storage_class 默认为 auto 或 static
 */
int a;
auto int b;

/**
 * 变量的初始化：
 * storage_class type variable_identifier = value;
 */
static int c = 2；
```

> 在 C89 标准中，变量的声明必须放在其作用域最开始的位置，C99 标准不需要。

标识符（identifier），其必须满足：

* 其中只能使用字母、数字和下划线
* 第一个字符不能是数字
* C 语言保留的关键字不能用作标识符
* 以下划线开头的变量多用于表示标准库中的变量，尽量不要在程序中使用
* 以双下划线或下划线加大写字母开头的变量名多用于表示编译器中的变量，尽量不要在程序中使用

四种存储类型（storage_class）：

* `auto` 类型，局部变量的默认类型，存储在内存中的栈区，作用域和生存期都在局部代码块内，离开作用域就会被编译器销毁。

* `register` 类型，存储在 CPU 寄存器而不是内存中的变量，用于存储使用频率高的变量，一般编译器会自动将使用频繁的变量存储至寄存器。
  
  > 1. 要注意的是由于机器字长的限制，只推荐将一些存储位数少的类型声明为 register 类型，如 char、short、int。
  > 2. 寄存器变量没有内存地址，不能使用 `&` 运算符。
  > 3. 即使在程序中声明了一个寄存器变量，编译器也未必会把这个变量存储在寄存器。

* `static` 类型，存储在内存中的静态存储区。如果修饰局部变量，则称作静态局部变量，在整个程序的生命周期保持存在，不会因进出作用域而创建和销毁。如果修饰全局变量，则称作静态全局变量，将全局变量的作用域限制在文件内部。

* `extern` 类型，可以理解为在其他文件中声明了一个全局变量或函数，要在本文件中使用这个全局变量或函数，一般用于多个文件共享一个全局变量或函数。

### 类型转换

1. 自动类型转换

当参与运算的两个数据是不同的数据类型，存储位数低的数据类型会自动转换为存储位数高的数据类型，即：

$$
\begin{aligned}
char \longrightarrow short \longrightarrow int &\longrightarrow long \longrightarrow float \longrightarrow double \\
signed &\longrightarrow unsigned
\end{aligned}
$$

```c
7 / 3.5; // 7 会自动转换为 double 型
```

2. 隐式强制类型转换

C 语言允许将另外一种类型的语赋值给另外一种类型，值将被转换成被赋值的类型。

```c
float a = 1.5;
int b = a; // 1.5 被强制转化为 1 
```

3. 显式强制类型转换

在 C 语言中可以使用 `(类型)变量或表达式` 来进行强制类型转换：

```c
/**
 * 强制类型转换形式：
 * (transform_type)expression
 */
float a = 3 / 2;            // a 的值为 1
float b = (float)3 / 2;     // b 的值为 1.5
float c = (float)(3 / 2);   // c 的值为 1
```

注意：

由整数（`int`、`long`）转换为单精度浮点型有时候会出现问题：

Demo：

```c
#include <stdio.h>

int main() {
    int a = 16777217;   // 2 的 24 次方加 1
    float b = a;
    printf("%f\n", b);  // 将会输出 16777216.00000
    return 0;
}
```

这种情况与 `float` 型在内存中的存储格式有关，`float` 类型中有 23 个比特位用于表示整数部分，而 `int` 型有 32 个比特位，当要转换为 `float` 型的整数需要大于 23 个比特位存储时，就需要进行舍入到最后一位为 0，故值会发生改变。

存储位数高的类型转换为存储位数低的类型，可能会出问题：

* `double` 型赋值给 `float` 型，精度下降，值可能会超出取值范围。 
* 浮点数赋值给 `int` 型，只保留整数部分，值可能超出取值范围。
* `long` 型赋值给 `int` 或 `short` 型，值可能超出取值范围，通常只复制右边的字节。

## 表达式和语句

### 值类别

一个表达式包含至少一个操作数以及零或多个运算符。C 语言中表达式以两个独立的属性刻画：类型和值类别，值类别有三种：左值、右值和函数。

左值（locator value，lvalue）是指一个在内存中『有确定位置的对象』，程序员可以访问和使用的对象。而右值则是一个『值』，由编译器进行访问和使用，一般临时存放在寄存器中。左值表达式的结果通常是在程序中声明和定义的对象，右值表达式的结果则通常是字面量或在表达式求值过程中产生的临时对象。

注意：字符串字面量和复合字面量是左值，存储在内存中。其他的字面量均为右值，临时存放在寄存器中。

左值也可以作为右值使用，但右值不能作为左值使用：

```c
int a, b;   // a、b 是左值
a = b;      // b 可以作为右值
2 = b；     // Invalid，2 不能作为左值
```

左值表达式的应用场景：

* 作为 `&` 运算符的操作数
* 作为赋值运算符的左操作数（必须为可修改的左值）
* 自增和自减运算符的操作数（必须为可修改的左值）
* 作为成员访问运算符的左操作数

左值指内存中有确定位置的对象，但并不是所有左值都可以被修改：

```c
const int a = 1;
a = 2;  // Invalid
int b[] = {1, 2, 3};
b++;    // Invalid
```

可修改的左值指任何非数组、非不完整数据类型且非 `const` 限定的左值表达式，且如果它是结构体或共用体，则其成员中也没有任何成员使用 `const` 限定。

### 运算符

运算符根据操作数的数量分为单目运算符、双目运算符和三目运算符。

1. 算术运算符

|运算符|作用|
|-|-|
|+|（单目）正值|
|-|（单目）负值|
|+|两个操作数相加|
|-|左操作数减去右操作数|
|\*|两个操作数相乘|
|\/|左操作数除以右操作数|
|\%|右操作数除左操作数的余数|

2. 关系运算符

|运算符|作用|
|-|-|
|\==|等于|
|!=|不等于|
|<|小于|
|<=|小于等于|
|>|大于|
|>=|大于等于|

3. 逻辑运算符

|运算符|作用|
|-|-|
|&&|逻辑与|
|\|\||逻辑或|
|!|（单目）逻辑非|

4. 位运算符

|运算符|作用|
|-|-|
|&|位与|
|\||位或|
|~|（单目）位非|
|^|位异或|
|<<|位左移|
|>>|位右移|

5. 赋值运算符：`=` 以及与算术运算符和位运算符结合的 `+=`、`-=`、`*=`、`/=`、`%=`、`>>=`、`<<=`、`&=`、`^=`、`|=`。

```c
a += 1; // 等价于 a = a + 1;
```

6. 指针运算符：`&` 和 `*`，`&` 运算符可以获取一个对象的内存地址，`*` 运算符可以读取一个指针存储的内存地址上的值。

```c
int a = 1;
int *p = &a;    // p 存储 a 的内存地址
*p;             // 1
```

7. 成员访问运算符：`.` 和 `->`，使用 `.` 运算符访问结构体变量或共用体变量的成员，也可以使用 `->` 运算符通过指针间接成员。`x->y` 等价于 `(*x).y`。

8. 自增和自减运算符：`++` 自增运算符，值加 1，`--` 自减运算符，值减 1。
   
   >注意：这两个运算符前置和后置会带来不同的效果，前置会直接将操作数的值加或减 1，后置会先将操作数保留一个副本然后再让操作数加或减 1，使用这个副本参与表达式中的其他运算。  
   > 建议：非必要不使用后置的自增或自减运算符，因为前置不会产生不必要的工作。

```c
int i = 1, j = 1;
int k1 = ++i;   // k1 为 2，i 为 2
int k2 = j++;   // k2 为 1，j 为 2
```

9. MISC

`sizeof` 运算符返回一个表达式或者类型所占的字节数，返回的类型是 `size_t` 型，在 64 位编译器中为 `unsigned long int` 型，另外还有 `ssize_t` 型，在 64 位编译器中位 `long int` 型。

```c
/**
 * sizeof 的两种用法：
 * 1. sizeof (type)
 * 2. sizeof expression
 */
int a;
printf("%ld\n", sizeof (int));      // 输出 4
printf("%ld\n", sizeof a);          // 输出 4
printf("%ld\n", sizeof (sizeof a)); // 输出 8，因为是 unsigned long int 型
```

逗号运算符，连接两个表达式，先计算左边的表达式再计算右边的表达式，但最终整个表达式的结果为右侧表达式的值。

```c
int i;
printf("%d\n", (i = 1, i = 2, i = 3)); 
```

将输出 3，也就是最右边的表达式的值。

C 语言为一的三目运算符 `?:`，条件运算符：

```c
condition ? statement1 : statement2;
```

### 优先级和结合性

![](image/Pasted%20image%2020220721191023.png)

### 语句结构

1. 选择结构

```c
/* if-else 型 */
if (condition1) {
    statement
} else if (condition2) {
    statement
} 
  /**
   * ......
   */ 
  else {
    statement
}

/* switch-case 型 */
switch (expression) {
    case value1:
        statement
    case value2:  
        statement
    /**
     * ......
     */
    break; // 只有遇到 break 才会终止
    default:
        statement
}
```

除此之外还可以使用三目运算符 `?:` 来表示选择结构：

```c
condition ? statement1 : statement2;
```

若 condition 为真，则执行 statement1，否则执行 statement2。

2. 循环结构

```c
/* while 型 */
while (condition) {
    statement
}

/* do-while 型 */
do {
    statement
} while (condition)

/* for 型 */
for (initializing; condition; addition) {
    statement
}
```

3. 跳转语句

* break & continue

`break` 和 `continue` 都能使程序跳过代码块中的后续代码。二者的区别是，如果在一个循环中使用 `break` 或 `continue`，`break` 的作用是跳过循环的剩余部分到达下一个语句，`continue` 的作用是跳过循环的剩余部分并进行下一轮循环。

```c
/* break */
while (condition) {
    statement1;
    break;      // 将跳转到 statement3
    statement2;
}
statement3;

/* continue */
while (condition) {
    statement1;
    continue;   // 将跳转到 while 进行下一轮循环
    statement2;
}
```

* return

使用 `return` 关键字可以结束当前执行的函数，并返回一个具体的值到调用该函数的位置。

使用 `return` 语句可以用来规避 `if-else` 多层嵌套，如：

```c
void f(int a[], int 4) {
    char *result;
    if (a[0] < 0) {
        if (a[1] < 0) {
            if (a[2] < 0) {
                if (a[3] < 0) {
                    result = "valid\n";
                } else {
                    result = "a[3] >= 0\n";
                }
            } else {
                result = "a[2] >= 0\n";
            }
        } else {
            result = "a[1] >= 0\n";
        }
    } else {
        result = "a[0] >= 0\n";
    }
    return result;
}

/* 可改写为： */
void f(int a[], int 4) {
    if (a[0]) return "a[0] >= 0\n";
    if (a[1]) return "a[1] >= 0\n";
    if (a[2]) return "a[2] >= 0\n";
    if (a[3]) return "a[3] >= 0\n";
    return "valid\n";
}
```

* goto

使用 `goto` 关键字将控制无条件转移至目标位置，对应汇编语言中的 `jmp`。

```c
/**
 * goto label
 * label 是跳转目标的标签
 */
    int n = 1;
A:;
    int a[n];
    if (n++ < 10)
        goto A;
```

## 复合数据类型

### 指针

指针是一种复合数据类型，可以指向内存地址上的某个数据，存储这个数据的内存地址。如果指针指向数据的类型为 `type`，则指针的类型为 `type *`。所有指针变量所占的字节数都一样，在 32 位编译器下 4 字节，在 64 位编译器下 8 字节。

* 基本数据类型指针

  如 `int *`、`char *`、`float *`、`double *` 等。

* 复合数据类型指针

  如数组指针、指针的指针、结构体指针、枚举指针、共用体指针等。

* `void *` 指针，由于不知道指向的类型，故无法访问 `void *` 指针所指的对象。

`void *` 指针存在的意义在于：可以自动转换成其他类型的指针，其他类型的指针也可以自动转换成 `void *` 指针。可用于传递未知类型的对象，一般用于实现 C 语言的泛型编程。

* 空指针：指针不指向任何内容，值为 `NULL`。

C 语言中的空指针 `NULL`：

```c
#define NULL ((void *)0)
```

C++ 中的空指针 `NULL`：

```cpp
#define NULL 0
```

* 函数指针：指向函数的指针。具体可见[函数指针](#函数指针)。

指针变量的声明和初始化：

```c
/**
 * 指针变量的声明：
 * date_type pointer_identifier;
 */
int *p;

/**
 * 指针变量的初始化：
 * date_type pointer_identifier = address;
 */
int a = 1；
int *p = &a;
int *q = 0x55555555;        // C89 可以这样直接指定内存
int *r = (int *)0x55555555; // C99 后必须进行强制类型转换
```

注意：

* 局部指针变量不要忘记初始化，如果不初始化，则指针的值是无法预测的内存地址，是一种野指针。如果暂时无法确定指针的值，则初始化为 `NULL`。
* 指针变量有严格的类型限制：

```c
float a = 1.0;
float *p = &a;  // Valid
int *p = &a;    // Invalid
```

不同类型的指针不能进行赋值，不过如果需要，可以通过强制类型转换实现：

```c
int *p = (int *)&a;
```

* 指针的类型不会影响指针的值，但会影响指针操作的字节数，譬如 `int *`  指针一次操作 4 个字节的数据，`double *` 指针一次操作 8 个字节的数据。所以指针的强制类型转换可能导致越界访问：

```c
int a = 1;
double *p = (double *)&a;
*p = 2; // 造成越界访问！
```

`const` 限定的指针：

```c
/**
 * 常量指针：
 * const type *pointer_identifier = value;
 * 或 date_type const *pointer_identifier = value;
 * 指针常量：
 * type * const pointer_identifier = value;
 */
int a = 1;
const int b = 2;
const int *p = &a;  // 常量指针，指向 a
int const *q = &b;  // 常量指针，指向 b
int * const r = &a; // 指针常量，指向 a
int * const s = &b; // 指针常量，指向 b
```

* 常量指针：不可通过该指针修改所指向的内容。可以指向普通变量也可以指向 `const` 限定的变量，但 `const` 限定的变量只能被常量指针所指向。一般用作函数的形参。

* 指针常量：只读的指针，不可修改指针的值。可以指向普通变量，也可以指向 `const` 限定的变量。

指针的运算法则：

* 指针的加/减运算：`p + i` 表示指针所指向位置后移 `i` 个单位的位置，`p - i` 表示指针所指位置前移 `i` 个单位的位置。 指针减指针，得到两指针之间的元素个数。
* 指针的关系运算：`p > q` 代表 `p` 指向的内存地址比 `q` 指向的内存地址大。`p == q` 代表 `p` 和 `q` 指向同一内存地址。
* 指针的逻辑运算：`!p` 代表 `p == NULL`，`p` 代表 `p != NULL`。

### 一维数组

数组是一种复合数据类型，代表一段内存上连续存储的相同类型的数据。如果数组中数据的类型为 `type`，数据的个数为 `n`，则数组的类型为 `type [n]`。

一维数组的声明和初始化：

```c
/**
 * 一维数组的声明：
 * storage_class type array_identifier[length];
 * length 代表数组的长度即元素个数，必须是常量表达式
 */
int a[10];
 
/**
 * 数组的初始化：
 * storage_class type array_identifier[length] = {value_list};
 * length 必须是常量表达式，可以省去
 */
int a1[5] = {1, 2, 3, 4, 5};    // {1, 2, 3, 4, 5}
int a2[] = {1, 2, 3, 4, 5};     // {1, 2, 3, 4, 5}
int a3[5] = {1, 2, 3, 4};       // {1, 2, 3, 4, 0} 未初始化的元素被设为 0
int a4[] = {1, 2, 3, 4};        // {1, 2, 3 ,4}
```

C99 中引入了新的复合数据类型的初始化方式：指定初始化器（designated initializer），在初始化列表中可以指明位置：

```c
/* 使用 {[index] = value, ...} */
int a5[5] = {[4] = 5, [0] = 1, 2, 3, 4};    // {1, 2, 3, 4, 5}
int a6[5] = {[4] 5, [0] 1, 2, 3, 4};        // 同上
```

GCC 对此进行了拓展，还可以使用这种方式进行初始化：

```c
int a[10] = {[0 ... 8] = 1, 2}; // 0~8 位为 1，9 位为 2
```

这种初始器同样应用于[结构体类型](#结构体)。

如果在声明数组时没有进行初始化，则后续无法再对数组进行初始化，如：

```c
int b[5];
b[5] = {1, 2, 3, 4, 5}; // Invalid
```

数组名不是可修改的左值，无法进行赋值，如：

```c
int c[3] = {1, 2, 3};
int d[3];
d = c; // Invalid
```

数组在声明后会创建在一段大小固定的连续的内存上，使用 `数组名[下标]` 的方式访问数组元素，下标 0 表示数组的第一个元素。

```C
int e[3] = {1, 2, 3};
printf("%d %d %d\n", e[0], e[1], e[2]); // 1 2 3
```

数组名代表数组首元素的地址即 `array == &array[0]`，这与对数组名取地址 `&array` 在数值上相同，但代表的含义和数据类型不同。`array` 仅代表数组首元素的地址，而 `&array` 代表整个数组的地址。要注意的是 `sizeof array` 是整个数组的字节数。

> 数组在内存中一旦创建便无法改变其大小和位置，故数组的首元素地址无法被改变，而数组名代表数组首元素的地址，所以数组名不是可修改的左值。

Demo：

```c
int main() {
    int f[3] = {1, 2, 3};
    int *p = f;         // p 指向数组 f 的首地址
    int *q = &f;        // Invalid，&f 是 int (*)[3] 类型，而 q 是 int * 类型
    int (*r)[3] = &f;   // r 是一个数组指针，指向数组 f
}
```

在 vscode 中调试上述代码，查看变量信息：

![](image/Pasted%20image%2020220718201834.png)

可看出 `p == r`，但 `p` 指向的内容是整数 1，而 `r` 指向的内容是一维数组 {1, 2, 3}。使用 GDB 查看变量信息，可知 `p` 的类型为 `int *`，而 `r` 的类型为 `int (*)[3]`。

根据分析结果，一维数组 `type array[N]` 的数据类型为 `type [N]`，其元素类型为 `type`，其数组名的类型为 `type *`。

数组是通过数组名来标识，而数组名是一个地址，也可认为是一个指针，所以可以用指针来表示数组名、访问数组元素：

```c
int g[3] = {1, 2, 3};
int *p = g; // p 等价于 g

/* 则 *(p + i) 等价于 g[i] */
*p;         // 等价于 g[0] 
*(p + 1);   // 等价于 g[1]
*(p + 2);   // 等价于 g[2]

/* 也可以这样 */
p[0];       // 等价于 g[0]
p[1];       // 等价于 g[1]
p[2];       // 等价于 g[2]
```

其实本质上数组便是通过指针实现的，`p[i]` 会被编译器转化为 `*(p + i)`。

Demo:

```c
#include <stdio.h>

int main() {
    int a[5] = {1, 2, 3};
    printf("%d", a[2]);
    printf("%d", 2[a]);
    return 0; 
}
```

上面这段代码可以正常编译，且输出结果为 33，也就是说 `a[2]` 和 `2[a]` 都表示数组的第三个元素，这是由于编译器将 `a[2]` 转化为 `*(a + 2)`，将 `2[a]` 转化为 `*(2 + a)` 导致的。 

### C99 变长数组

C99 中引入了变长数组（Variable Length Array，VLA），在声明一个数组时可以使用变量指定数组的长度：

```c
int len;
int vla[len]; // 变长数组
```

注意：

1. VLA 只能用作局部变量，也就是只能在函数参数或函数内部中使用
2. VLA 在声明时不能进行初始化
3. VLA 数组一旦被创建，其长度就无法改变
4. VLA 不能作为结构体或联合体的成员

一般情况下 VLA 在函数参数或函数内部中使用：

```c
int sum(int, int a[*]); // 函数声明中的变长数组
int sum(int n, int a[n]) {
    int i = 0, sum = 0;
    while (i < n) {
        sum += a[i++];
    }
    return sum;
}
```

VLA 虽然可以使用变量指定数组的长度，但其本质依然是静态数组，一旦创建其内存大小就无法改变。

于此相比在 C 语言中更常用的方法是[动态创建数组](#动态内存管理)。

### 多维数组

多维数组的声明和初始化：

```c
/**
 * 多维数组的声明（以二维为例）：
 * storage_class type array_identifier[length1][length2];
 */
int a[2][3];

/**
 * 多维数组的初始化（以二维为例）：
 * storage_class type array_identifier[length1][length2] = {{value_list1}, {value_list2}};
 */
int a[2][3] = {{1, 2, 3}, {4, 5, 6}};
```

多维数组的本质是以数组为元素的数组。上述代码中，数组 `a` 有两个元素 `a[0]` 和 `a[1]`，这两个元素的数据类型是 `int [3]`（即长度为 3 的 `int` 型一维数组）。 

数组名代表数组首个元素的地址，即数组名 `a` 代表 `a[0]` 的地址 `&a[0]`，`a[0]` 是 `int [3]` 型，所以数组名 `a` 是 `int (*)[3]` 类型。

`a[0]` 也是一个数组，故 `a[0]` 是这个数组的数组名，所以 `a[0] == &a[0][0]`。

Demo：

```c
int main() {
    int b[2][3] = {{1, 2, 3}, {4, 5, 6}};
    int (*p)[3] = b;        // b 代表多维数组首元素 {1, 2, 3} 的地址，int (*)[3] 型
    int (*q)[2][3] = &b;    // &b 代表整个多维数组的地址，int (*)[2][3] 型
    int *r = b[0];          // b[0] 代表一维数组首元素 1 的地址，int 型
}
```

在 vscode 中调试上述代码，查看变量信息：

![](image/Pasted%20image%2020220718201452.png)

可看出多维数组 `b` 以两个一维数组 `b[0]` {1, 2, 3}、`b[1]` {4, 5, 6} 作为元素，`p` 指向的内容是一维数组 {1, 2, 3}，`q` 指向的内容是多维数组 {{1, 2, 3}, {4, 5, 6}}，`r` 指向的内容是整数 1。使用 GDB 查看变量的信息可知 `p` 的类型是 `int (*)[3]`，`q` 的类型是 `int (*)[2][3]`，`r` 的类型是 `int *`。

根据分析结果，多维数组 `type array[N][M]` 的数据类型为 `type [N][M]`，其元素的类型为 `type [M]`，其数组名的类型为 `type (*)[M]`。

与一维度组相同，多维数组也可以使用指针表示：

```c
int c[2][3] = {{1, 2, 3}, {4, 5, 6}};
int (*p)[3] = c;

/* 则 *(*(p + i) + j) 等价于 c[i][j] */
**p; // 等价于 c[0][0]
*(*(p + 0) + 1);    // 等价于 c[0][1]
*(*(p + 1) + 1);    // 等价于 c[1][1] 

/* 也可以这样： */
p[i][j];            // 等价于 c[i][j]
```

### C-风格字符串

C 语言中没有 C++ 中的 `string` 这一数据类型，C-风格字符串本质上是以 `'\0'` 结尾的字符数组。

譬如字符串『12345』的数据类型是 `char [6]`，为什么是 6 而不是 5，因为末尾还有一个元素  
 `'\0'`。
 
字符数组的定义和初始化，以下几种方式都对：

```c
char str[] = {'h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd', '!'};
char str[12] = {'h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd', '!'};
char str[] = "hello world!";
char str[] = {"hello world!"};
char str[13] = "hello world!";  
```

如果以字符串的方式初始化字符数组，则字符数组的末尾元素是 `'\0'`。

但以字符的方式初始化字符数组，不会在末尾加上 `'\0'`。

```c
char str1[5] = {'1', '2', '3', '4', '5'};
char str2[5] = "12345"; // Error，末尾元素是 \0 故应该有 6 个元素
char str3[] = {'1', '2', '3', '4', '5'};
char str4[] = "12345";
printf("%ld\n", sizeof str3);   // 5
printf("%ld\n", sizeof str4);   // 6
```

C-风格字符串变量有两种定义方式 `char *` 和 `char []`：

```c
char *str1 = "123";
char str2[] = "123";
str1[0] = '0';  // Invalid
str2[0] = '0';  // Valid
```

查看汇编代码：

```asm
```

二者的区别是：

* 前者是一个字符指针的初始化，`str1` 是一个指向字符串『123』的指针，字符串字面量存储在内存中的 .rodata 常量段，是只读存储区，所以 `str1` 是一个常量指针。
* 而后者是一个字符数组的初始化，`str2` 是一个字符数组，存储在内存中的栈区，它的值是可以进行修改的。所以在初始化时应当使用字符数组，其他时候使用字符指针更方便一些。

string.h 头文件定义的库函数：

1. `strcpy()` & `strncpy()` 字符串复制

```c
/* 拷贝 src 到 dest */
char * strcpy(char *dest, const char *src);

/* 拷贝 src 的至多 count 个字符到 dest */
char * strncpy(char *dest, const char *src, size_t count);
```

这两个函数在进行拷贝时不遇到 `'\0'` 或达到最大拷贝数量便不会停止拷贝，故可能会发生数组越界导致缓冲区溢出。

> 针对缓冲区溢出问题 C11 提出了一系列新的库函数，只需在原本的库函数名后面加上 `_s`，如 `strcpy_s()`。在编译时会检查是否存在缓冲区的溢出，在此不作过多介绍。

2. `strcat()` & `strncat()` 字符串连接

```c
/* 将 src 连接到 dest 的后面 */
char *strcat(char *dest, const char *src);

/* 将 src 至多 count 个字符连接到 dest 后面 */
char *strncat(char *dest, const char *src, size_t count); 
```

同样有缓冲区溢出的隐患。

4. `strcmp()` & `strncmp()` 字符串比较

```c
/* 比较两个字符串 */
int strcmp(char const *s1, char const *s2);

/* 比较两个字符串的前几位 */
int strncmp(char const *s1, char const *s2, size_t count);
```

逐个比较两个字符串的字符，直到发现不相同的字符。

 * 如果这个字符的 ASCII 码 `s1` > `s2` 将返回大于 0 的数，反之小于 0。
 * 如果 `s1` 等于 `s2`，返回等于 0 的数。

5. `strlen()` 返回字符串长度

```c
/* 返回字符串 s 的长度（不包含末尾的 '\0'） */
int strlen(char const *s);
```

如果字符数组不以 `'\0'` 结尾，同样有缓冲区溢出的隐患。

6. `strstr()` 字符串查找

```c
/* 在 s1 中查找 s2，如果找到了返回指向该位置的指针，否则返回 NULL */
char *strstr(char const *s1, char const *s2);
```

7. `strrev()` 字符串反转

```c
char *strrev(char *s);
```

8. `toupper()` & `tolower()` 字符大小写转换

9. memcpy & memmove

```c
void *memcpy(void *dest, const void *src, size_t count);
void *memmove(void *dest, const void *src, size_t count);
```

与 `strcpy()` 类似，区别是可以应用于任意数据类型，`memmmove()` 是更安全的函数，因为即使 `src` 和 `dest` 有内存的重叠，也会保证在 `src` 被覆盖之前拷贝至 `dest`。

10. `memcmp()`

```c
int memcmp(const void *a1,const void *a2, size_t count);
```

与 `strcmp()` 类似，不同点是可以应用于任意的数据类型。

### 指针数组和数组指针

指针数组：由指针构成的数组，本质是数组。数据类型 `type *p[]`。

数组指针：指向数组的指针，本质是指针。数据类型 `type (*p)[]`。

根据优先级顺序，`[]` 的优先级高于 `*` 先确定数据类型为数组类型，故 `type *p[]` 代表指针类型的数组。而 `()` 与 `[]` 的优先级相同，结合性从做到右，先确定数据类型为指针，故 `type (*p)[]` 代表指向数组的指针。

```c
char *array[3] = {"Hello", "World", "!"};   // 指针数组
char *(*ptr)[3] = &array;                   // 数组指针
```

上述代码可以编译通过，在 vscode 中调试：

![](image/Pasted%20image%2020220720221241.png)

可见 `array` 是一个数组，数组的元素是 `char *` 型，`ptr` 是一个指针，指向的内容是 `char (*)[3]` 类型。

### 枚举类型

枚举就是把可能的值一一列举，枚举变量只能取其中的一个值。

枚举类型的定义：

```c
/**
 * 枚举的定义：
 * enum identifier {
 *     element1,
 *     element2,
 *     ......
 * };
 */
enum Week {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
};
```

枚举变量的声明和初始化：

```c
enum Week today = Monday;   // 值只能在 week 的成员中取
printf("%d\n", today);      // 输出 0
```

输出 0 的原因是在缺省状态下默认第一个成员的整数值为 0，并依次递增 1。

如果对枚举的成员赋值，则未赋值的成员的值为前一个成员的值加 1。

```c
enum demo {
    A=2,
    B=4,
    C,
    D
};

printf("%d %d\n", C, D); // 5 6
```

### 结构体类型

C 语言中没有 `class` 这一类型，但提供了 `struct` 类型以表示由多个成员组成的数据类型。

结构体的定义以及结构体变量的声明和初始化：

```c
/**
 * 结构体的定义：
 * struct struct_identifier {
 *     member_list
 * };
 * 结构体变量的声明
 * struct struct_identifier variable_identifier;
 */
struct Student {
    int id;
    char sex;
    int age;
    char name[10];
};
struct Student Allen;

/**
 * 定义匿名结构体变量：
 * struct {
 *     member_list
 * } identifier;
 */
struct {
    int id;
    char sex;
    int age;
    char name[10];
} Allen, Bob;
```

结构体变量的初始化：

```c
struct Student A = {1, 'm', 20, "Allen"};   // 必须按照定义中的成员顺序来
struct Student B = A;                       // 同类型结构体变量可以直接赋值
```

C99 标准新增的指定初始化器（designated initializer）：

```c
struct Student C = {
    .age = 18,
    .name = "Bob",
    .id = 2,
    .sex = 'f'
};
```

GCC 对此进行了拓展，还可以使用这种方式进行初始化：

```c
struct Student D = {
    age : 20,
    name : "David",
    id : 3,
    sex : 'm'
};
```

访问结构体变量的成员：

```c
struct Student A;
int a = A.age;  // 可以使用 . 访问结构体便利的成员

struct Student *p;
int b = p->age; // 也可以使用结构体指针访问成员
```

当结构体当中含有长度未知的数组（柔性数组），如：

```c
struct s {
    int a;
    int b[]; // 柔性数组
};

struct s s1 = {1, {2, 3}}; // Invalid，因为 b 的长度未知
struct s *s2 = malloc(sizeof(struct s) + (sizeof(int) * 5)); // 柔性数组为 b[5]
```

结构体内存对齐：

Demo：

```c
#include <stdio.h>

struct demo {
    char a;
    int b;
    short c;
};

int main() {
    printf("%ld\n", sizeof (struct demo));
}
```

理论上上述代码似乎应该输出 1+4+2=7，然而实际上编译运行的结果为 12。

使用 `offsetof(type, member)` 可得到一个成员相对于这个类型的起始位置的偏移字节数，依次求出 a、b、c 的偏移量：

```c
printf("%ld\n", offsetof(struct demo, a));  // 0
printf("%ld\n", offsetof(struct demo, b));  // 4
printf("%ld\n", offsetof(struct demo, c));  // 8
```

也就是说 `char a` 存储在第 0 个字节，`int b` 存储在第 4～7 字节，`short c` 存储在 8～9 字节。这是由于内存对齐导致：

* 结构体第一个成员存储在 0 偏移处
* 其他成员需要对齐到某个数字的整数倍，取编译器默认对齐数和该成员的字节数的较小值
* 结构体的总大小为成员最大对齐数的整数倍

>为什么要内存对齐？
>
>1. 并非所有硬件都可以访问到内存中所有地址上的数据
>2. 可以使内存的访问速度加快，详见[数据的表示与运算](../System/数据的表示与运算.md)。

位段：

C 语言中没有位段数据类型，需要借助结构体来实现。位段的类型只能是 `int`、`unsigned int` 型，使用整数定义所占的**比特**数，如：

```c
struct demo {
    unsigned int a : 1; // 1 bit
    unsigned int b : 2; // 2 bits
    unsigned int c : 3; // 3 bits
};
```

这段代码定义了 3 个位段 a、b、c，分别为 1 比特、2 比特和 3 比特。

### 共用体类型

共用体是特殊的结构体，共用体成员共享同一段内存地址，任何时刻共用体变量中只有一个成员是带有值的。

共用体的定义以及共用体变量的声明与结构体相同，只是把关键字 `struct` 改为 `union`。

但是共用体变量的初始化只允许一个元素被赋值：

```c
union data {
    int A;
    float B;
    char C[10];
};

union date a = {1};         // 不指定成员则赋值给第一个成员
union data c = {B: 1.2};    // 赋值给指定成员
union data b = {.B = 1.2};  // 同上
union data d;
d.B = 1.2;
```

共用体变量任何时刻只有一个成员是带有值的，即最后一次被赋值的成员带有值。

共用体所占内存大小满足其存储位数最大的成员即可，因为所有成员共享一段内存。

### 不完全数据类型

`void` 型、未指定长度的数组、未定义内容的结构体和共同体被称为不完全数据类型。

不完全数据类型有什么用？

1. 应用于头文件中，不定义完整的数据类型，而是在源文件中定义，可以提高代码的灵活度。也可以实现抽象模型的封装，防止用户直接访问结构体成员，而是使用接口访问。
2. 用在一些特殊场景：

```c
struct A;
struct B;

struct A {
    struct B *p;
}

struct B {
    struct A *p;
}
```

### mutable volatile restrict

`const` 限定符限定一个变量是只读的，当 `const` 修饰结构体变量，结构体变量内部的成员都无法被改变。如果在某个成员前面加上 `mutable`，则表示改成员可以改变。

```c
const struct S {
    int x;          // 不可改变
    mutable int y;  // 可以改变
} s1;
```

`volatile` 限定符与 `const` 相反，用来表明一个变量是不稳定的，禁止编译器进行优化。

C99 引入了 `restrict` 限定符，只用于限定指针，告知编译器所有修改该指针所指向内容的操作全部都是基于该指针的，即不存在其它进行修改操作的途径。使得编译器进行更好的优化。

### typedef

`typedef` 关键字的作用是给复杂的数据类型起一个别名，可以在定义复杂的数据类型时使用，如：

```c
typedef struct Student {
    int age;
    int id;
    float heigtht;
    float weight;
} STU;

STU a; // struct Student a;

typedef int (*PTR_ARR)[20];
PTR_ARR p; // int (*p)[20]
```

## 预处理

预处理包括：

* 预处理指令：`#define`、`#include` 等
* 预处理运算符：`#`、`##` 等
* 预定义宏：

  * `__LINE__`：当前编译行的行号
  * `__FILE__`：当前编译源程序的文件名
  * `__DATE__`：当前编译源程序的创建日期
  * `__TIME__`：当前编译源程序的创建时间
  * `__STDC__`：判断编译器是否为标准 C

### 宏定义

`#define` 可以定义一个宏以表示某个字符串，代码中的宏都会被替换成其对应的字符串。

有两种形式：

1. 不带参数的宏定义

```c
/* #define macro string */
#define PI 3.14                                 // 程序中的 PI 会被替换成 3.14
#define PRINT fprintf(stdout, "helloworld!\n")  // PRINT 会被替换为后面的语句
#define HELLO "hello\
world"                                          // 可以使用 \ 连接下一行
```

`#define` 与 `typedef` 的区别：前者是简单的文本替换，后者则代表了一种数据类型。

Demo：

```c
#define INT1 int *
typdef int* INT2

int main() {
    INT1 a1, b1;        // a1 为 int * 型，b1 为 int 型
    INT2 b2, b2;        // a2、b2 都为 int * 型
    int a = 1;
    const INT1 a3 = &a; // a3 为常量指针，指向的地址可变，但所指地址的内容不能变
    const INT2 a4 = &a; // a4 为指针常量，不可改变指向的地址，但可以改变该地址的内容
} 
```

2. 带参数的宏定义

```c
/* #define macro(parameter_list) string */
#define ADD(X,Y) ((X)+(Y))
ADD(1,2); // 预处理阶段会被替换为 ((1)+(2))

#define SUM(X,Y) printf(#X"+"#Y"=%d\n", ((X)+(Y))) // # 把参数转换为字符串
SUM(1,2); // 替换为 printf("1""+""2""=%d\n", ((1)+(2)))

#define LINK(X,Y) (X##Y) // ## 用于连接两个标记
LINK(hello,world); // 连接为 helloworld
```

C99 标准引入了可变参数的宏定义：

```c
/* 宏可以带有可变参数，用 ... 表示，__VA_ARGS__ 在预处理时被实际的参数替换 */
#define PRINT(fmt,...) fprintf(stdout, fmt"\n", ##__VA_ARGS__)
PRINT("%d","1");        // fprintf(stdout, "%d""\n","1");
PRINT("helloword!");    // fprintf(stdout, "helloword!""\n");
```

上面的代码中包含一些预处理运算符，`\` 是行连接运算符，`#` 是字符串化运算符，`##` 是标记连接运算符，除此之外还有 `#@` 是字符化运算符。

宏定义时最好在参数左右以及整个字符串的左右加上括号，以免替换后出现运算符优先级的问题。

Demo：

```c
#define MUL(X,Y) X*Y        // 期望该宏用于表示两个参数的乘积

int main() {
    int a = MUL(1 + 2,3);   // 期望 a 为 3 乘 3
    int b = 5 / MUL(1,2);   // 期望 b 为 5 除以 2
}
```

预处理之后程序被替换成了：

```c
int main() {
    int a = 1 + 2*3;
    int b = 5 / 1*2;
}
```

所得的程序并非期望中的结果，因为出现了运算符优先级的问题，所以最好在参数左右以及整个字符串的左右加上括号，变成 `#define MUL(X,Y) ((X)*(Y))`。

`#undef` 用于撤销之前定义过的宏，也就是说宏的生命周期从 `#define` 开始到 `#undef` 结束。

为什么要使用宏定义？

>函数调用时需要压入参数并跳转到调用函数的栈帧，等函数执行完再返回调者的栈帧。这种转移操作要求在调用函数前要保存现场并记忆执行的地址，返回i后要恢复现场，并按原来保存地址继续执行。因此，函数调用要有一定的时间和空间方面的开销，将影响程序执行的效率。  
>而宏在预处理时把代码展开，不会在程序执行时产生额外的空间和时间方面的开销，所以宏比函数更加高效。

C99 中引入了内联函数，内联函数是一种特殊的函数，在函数定义时加上 `inline` 用来表明是内联函数。内联函数在调用时直接把函数的代码拷贝到调用的位置，而不用进行栈帧的跳转和返回。

```c
inline int sum(int a, int b) {
    return a + b;
}

int main() {
    int a = sum(1, 2);  // int a = 1 + 2; 
}
```

>内联函数与宏定义的区别？
>
>1. 宏在预处理阶段被替换，内联函数在编译阶段被调用时展开。
>2. 宏定义只时文本的替换，而内联函数是真正的函数，可以检查参数的类型，且具有返回值，应用的场景更广泛、安全。

Demo：

```c
#define SQRT(X) ((X)+(X))  

int sqrt(int);

int main() {
    int i = 1, j = 1;
    int a = SQRT(++i);
    int b = sqrt(++j);
}

inline int sqrt(int x) {
    return x * x;
}
```

运行程序，`a` 的值为 4，`b` 的值为 6，宏定义不能使用具有副作用的表达式作为参数。

### 条件编译

1. `#if` `#else` `#elif`

与 C 语言中的 `if-else if-else` 类似，根据满足的条件选择编译的代码片段。

```c
#if condition
    statement
#elif condition
    statement
/**
 * ......
 */
#else 
    statement
#endif
```

2. `#ifdef` `#ifndef`

`#ifdef` 用于判断某个宏是否被定义，`#ifndef` 则相反，判断某个宏是否未被定义。

```c
#ifdef macro_identifier
    statement
#elif condition
    statement
/**
 * ......
 */
#else
    statement
#endif
```

3. `#pragma` 的作用是设定编译器状态，或指定编译器完成一些特殊的行为。

可提供的编译器指令很多，其中常用的有：

```c
#pragma once                            // 保证文件只被编译一次
#pragma message(message)                // message 会在编译输出窗口显示
#pragma code_seg(["section-name"][""])  // 设置程序中存储函数代码的代码段
```

4. `#error` 的作用是在编译程序时只要遇到 `#error` 就会生成一个错误的消息，并停止编译。

```c
#error message // 将显示错误 message 并停止编译
```

5. `#line` 的作用是改变 `__LINE__` 和 `__FILE__` 的内容，前者存储正在编译行的行号，后者存储正在编译的文件的文件名。

Demo：

```c
/* #line line "file" */
#include <stdio.h>
#line 100 "test.c"

int main() {
    printf("%s: %d\n", __FILE__, __LINE__);
}
```

将会输出：test.c: 102

### 文件包含

`#include` 可以使一个文件的全部内容都包含（即复制）到本文件中，有两种形式：

```c
#include <file> // 使用尖括号包含 file
#include "file" // 使用双引号包含 file
```

区别在于，使用尖括号时编译器会到 C 标准库中寻找要包含的文件。使用双引号时，编译器先在当前目录（或指定目录）下寻找要包含的文件，如果寻找不到再到 C 标准库中寻找。

如果要包含 C 标准库中的文件，则使用尖括号；如果要包含其他位置的文件，则使用双引号。

Demo：两个文件 test1.c 和 test2.c 如下：

```c
/* test1.c */
int main() {
    int a = 1;
}

/* test2.c */
#include "test1.c"
```

使用 GCC 进行预处理 `gcc -E test2.c -o test2.i`，查看 test2.i 文件：

```bash
# 1 "test2.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 31 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 32 "<command-line>" 2
# 1 "test2.c"
# 1 "test1.c" 1
int main() {
    int a = 1;
}
# 1 "test2.c" 2
```

可见将 test1.c 的内容完全复制到了 test2.c。

当文件嵌套包含时会出现问题，譬如:

```mermaid
graph TB
A[test1.c] --> |#include| B[test2.h] --> |#include| C[file.h]
A --> |#include| D[test3.c] --> |#include| C
```

由此以来会有两份 file.h 的内容被拷贝到 test1.c 中，则可能会导致内容的重复定义。

为解决这种情况，可以在每个文件开头写入：

```c
#ifndef __TEST_H__ // 假设该文件为 test.h
#define __TEST_H__
/**
 * 文件内容
 */
#endif
```

或者在文件开头写入：

```c
#pragma once // 保证文件只被编译一次
```

## 函数

函数是 C 语言中重要的概念，程序执行时会在不同的函数之间跳转和返回。函数的三大要素是：

1. 返回值类型
2. 函数名
3. 参数列表

C 语言是需要编译的语言，程序中函数在被调用之前必须声明函数原型。

```c
/**
 * 函数原型的声明：
 * (extern) return_type function_identifier(parameter_list);
 * 函数在声明时候参数列表可以只写数据类型，extern 表示引用其他文件的函数
 */
int Add(int a);
int Add(int);
int Add(); // 不指出参数列表，在 C 语言中也对

/**
 * 函数的定义：
 * return_type function_identifier(parameter_list) {
 *     statement
 * }
 */
int Add(int a) {
    return ++a;
}
```

### 函数返回值

C 语言中返回值类型若缺省，则默认为 int 型。

函数的返回值有两种：

1. 返回一个值：使用变量作为返回值。
2. 返回一个地址：以指针作为返回值。

当以指针作为函数的返回值时需要注意：

Demo：

```c
int *f() {
    int a[] = {1, 2, 3};
    return a;
}

int main() {
    int *p = f();
    return 0;
}
```

编译该程序会有警告 warning: function returns address of local variable。这是因为数组 `a` 是函数 `f()` 内的局部变量，定义在函数 `f()` 的栈帧中，`f()` 调用结束后栈帧被销毁，所以数组 `a` 也被销毁，所以指针 `p` 所指向的内容是不存在的，或者说是内存中的脏数据。这种情况如何编写才正确？可以使用 `malloc()` 函数在堆上建立数组，这样就不会随着栈帧一起被销毁。详见[动态创建数组](#动态内存管理)。

### 函数参数

函数定义时的参数叫做形式参数，函数调用时使用的参数叫做实际参数，传递参数方式有两种：

1. 值传递

使用变量、常量的值作为函数的参数，函数调用时将实参的值压入调用函数栈（或寄存器）作为形参的值。也就是说实参和形参除了值相等之外没有任何关系，形式参数作出的任何改变都不影响实际参数。

2. 地址传递

使用内存地址作为函数的参数，函数调用时将实参的值压入调用函数栈（或寄存器）作为形参的值。因为参数是一个内存地址，故如果对形参上的内容进行更改，则实参上的内容也会被更改。

Demo：

```c
#include <stdio.h>

void test0(char c) {
    c = 'c';
}

void test1(char *q) {
    q = q + 1;
}

void test2(char **r) {
    *r = *r + 1;
}

int main() {
    char a[] = {'a', 'b'};
    char *p = a; 
    test0(*p);
    printf("%c ", *p);
    test1(p);
    printf("%c ", *p);
    test2(&p);
    printf("%c\n", *p);
    return 0;
}
```

上述代码将输出：a a b，说明值传递无法改变实参的值，地址传递可以改变实参上的内容，而不可改变实参的值。

Demo：

```c
void fun1(int x, int y) {   // 值传递
    ++x;
    ++y;
}

void fun2(int *x, int *y) { // 地址传递
    ++(*x);
    ++(*y);
}

int main() {
    int x, y;
    x = 1;
    y = 2;
    fun1(x, y);
    fun2(&x, &y);
}
```

编译程序，使用 GDB 反汇编：

```asm
(gdb) disas main
Dump of assembler code for function main():
   0x0000000000001193 <+0>:	    endbr64 
   0x0000000000001197 <+4>:	    push   %rbp
   0x0000000000001198 <+5>:	    mov    %rsp,%rbp
   0x000000000000119b <+8>:	    sub    $0x10,%rsp
   0x000000000000119f <+12>:	mov    %fs:0x28,%rax
   0x00000000000011a8 <+21>:	mov    %rax,-0x8(%rbp)
   0x00000000000011ac <+25>:	xor    %eax,%eax
   0x00000000000011ae <+27>:	movl   $0x1,-0x10(%rbp)
   0x00000000000011b5 <+34>:	movl   $0x2,-0xc(%rbp)
   0x00000000000011bc <+41>:	mov    -0xc(%rbp),%edx
   0x00000000000011bf <+44>:	mov    -0x10(%rbp),%eax
   0x00000000000011c2 <+47>:	mov    %edx,%esi
   0x00000000000011c4 <+49>:	mov    %eax,%edi
   0x00000000000011c6 <+51>:	callq  0x1149 <fun1(int, int)>
   0x00000000000011cb <+56>:	lea    -0xc(%rbp),%rdx
   0x00000000000011cf <+60>:	lea    -0x10(%rbp),%rax
   0x00000000000011d3 <+64>:	mov    %rdx,%rsi
   0x00000000000011d6 <+67>:	mov    %rax,%rdi
   0x00000000000011d9 <+70>:	callq  0x1162 <fun2(int*, int*)>
   0x00000000000011de <+75>:	mov    $0x0,%eax
   0x00000000000011e3 <+80>:	mov    -0x8(%rbp),%rcx
   0x00000000000011e7 <+84>:	xor    %fs:0x28,%rcx
   0x00000000000011f0 <+93>:	je     0x11f7 <main()+100>
   0x00000000000011f2 <+95>:	callq  0x1050 <__stack_chk_fail@plt>
   0x00000000000011f7 <+100>:	leaveq 
   0x00000000000011f8 <+101>:	retq   
End of assembler dump.
(gdb) disas fun1
Dump of assembler code for function fun1(int, int):
   0x0000000000001149 <+0>:	    endbr64 
   0x000000000000114d <+4>:	    push   %rbp
   0x000000000000114e <+5>:	    mov    %rsp,%rbp
   0x0000000000001151 <+8>:	    mov    %edi,-0x4(%rbp)
   0x0000000000001154 <+11>:	mov    %esi,-0x8(%rbp)
   0x0000000000001157 <+14>:	addl   $0x1,-0x4(%rbp)
   0x000000000000115b <+18>:	addl   $0x1,-0x8(%rbp)
   0x000000000000115f <+22>:	nop
   0x0000000000001160 <+23>:	pop    %rbp
   0x0000000000001161 <+24>:	retq   
End of assembler dump.
(gdb) disas fun2
Dump of assembler code for function fun2(int*, int*):
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
```

可以看出使用值传递的 `fun1()` 使用 `mov` 指令传递实际参数的值，而使用地址传递的 `fun2()` 则使用 `lea` 指令直接传递实参的地址。

数组作为函数的参数：

数组作为函数的参数，形参需要使用 `type array[]` 以传递数组名，并使用 `int n` 来传递数组的长度，如：

```c
int f(int a[], int n);
int f(int a[][3], int n); // 二维数组传递数组名
```

> 注意：
> 
> 1. 使用地址作为参数还有一个好处，譬如使用结构体变量作为参数时，值传递需要在调用函数栈上创建一个新的结构体变量的副本，消耗内存影响执行效率。而使用地址传递则不会产生副本。
> 2. 但是使用地址传递可能会改变地址上的值，所以如果不需要改变地址上的值，最好使用 `const` 限定形参为常量指针，以起到保护的作用。

可变参数的函数：

通过宏 在最后一个参数的位置使用 `...` 表示可变参数，`var_list` `var_args` `var_`。

```c

```

### 主函数

`main` 函数是 C 语言程序的入口函数，它有两种形式：

```c
int main(void) {
    statement
}

int main(int argc, char *argv[]){
    statement
}
```

参数 `argc` 代表程序所运行的环境传递给程序的参数数量。`argv` 是一个指针数组

在 C89 标准规定 `main` 函数最后必须加上 `return 0`，但是 C99 标准可不加。

### 函数调用栈帧

### 函数指针

```c
/**
 * 函数指针的声明：
 * 1. return_type (*pointer_identifier)(parameter_list);
 * 2. typedef return_type (*TYPE)(parameter_list);
 */
int (*fp)(int, int);
typedef int (*FUNC_PTR)(int, int);

/**
 * 函数指针的初始化：
 * return_type (*pointer_identifier)(parameter_list) = function_identifier;
 */
int func(int, int);             // 函数的声明
int (*fp)(int, int) = func;     // 函数指针初始化
int (*fp)(int, int) = &func;    // 这样也可以

/**
 * 函数指针的调用：
 * p(parameter_list);
 * (*p)(parameter_list);
 */
fp(1, 2);       // 直接通过函数指针调用
(*fp)(1, 2);    // 通过函数指示符调用
```

Demo：

```c
int max(int x, int y) {
    return x > y ? x : y; 
}

int main() {
    typedef int (*FUNC_PTR)(int, int);
    FUNC_PTR fp = max;
    int z = fp(100, 50);
    return 0;
}
```

回调函数：

以函数指针作为函数的参数的函数称为回调函数。

Demo：

```c
#include <stdio.h>

typedef void (*FUNC_PTR)();

void fun_A() {
    printf("function A");
}

void fun_B(FUNC_PTR p) {
    p();
    printf("function B");
}

int main() {
    FUNC_PTR fp = fun_A;
    fun_B(fp);
}
```

运行后输出：function Afunction B

> 为什么要使用回调函数？
> 
> 将调用者与被调用者分离，只需要了解被调用函数的原型，使函数的在处理相似的事件时更为灵活。

## I/O

### 标准 I/O 和文件 I/O

什么 I/O？什么是标准 I/O？什么是文件 I/O？

I/O 即 Input/Output 输入/输出，表示程序从某个文件输入数据或向某个文件输出数据，也就是进行对文件的读取和写入的操作。注意输入对应文件的读取，输出对应文件的写入。

简单来说，标准 I/O 是 C 标准库规定的一组负责文件的输入输出的库函数，是高级的磁盘 I/O 操作。文件 I/O 指 POSIX 标准定义的负责文件的输入输出的[系统调用](../OS/操作系统概述.md)接口，是低级的磁盘 I/O 操作。

* 标准 I/O 是基于流（stream）的带有缓冲区的 I/O，标准 I/O 的转移过程是：数据 $\rightarrow$ 流 $\rightarrow$ 内核 $\rightarrow$ 文件。
* 文件 I/O 是基于文件描述符（file descriptor）的不带缓冲区的 I/O，文件 I/O 数据的转移过程是，数据 $\rightarrow$ 内核 $\rightarrow$ 文件。
* 标准 I/O 通过 C 标准库中的库函数实现，实现过程是：程序 $\rightarrow$ C 标准库 $\rightarrow$ 系统调用 $\rightarrow$ 内核函数 $\rightarrow$ 操作磁盘。
* 文件 I/O 则通过 POSIX 提供的系统调用接口直接访问内核，实现过程：程序 $\rightarrow$ 系统调用 $\rightarrow$ 内核函数 $\rightarrow$ 操作磁盘。
* 标准 I/O 相比于文件 I/O 的好处，一是有缓冲区，避免进行频繁的系统调用，二是可移植性强，可以在不同的操作系统上使用。

在 C 语言中使用 `FILE` 来表示一个流，在 stdio.h 文件中声明，是通过结构体实现：

```c
typedef struct {
    ......
} FILE;
```

一般通过文件流指针的方式表示一个流：

```c
FILE *stream;
```

当 C 语言 `main` 函数运行，将会打开三个预定义的流供程序使用，被称为标准流：

* `FILE *stdin`：标准输入流，用于程序中的输入
* `FILE *stdout`：标准输出流，用于从程序中的输出
* `FILE *stderr`：标准错误流，用于输出程序的错误和诊断信息

### 标准 I/O 库函数

1. 文件的打开与关闭

* `fopen()` & `fclose()`

```c
FILE *fopen(const char *filename, const char *opentype);
int fclose(FILE *stream);
```

`fopen()` 打开一个流，打开失败返回 `NULL`，`opentype` 指定了打开文件的方式：

  * `r`：以只读模式打开文件
  * `w`：以只写模式打开文件并将文件的内容清空，如果文件不存在，则新建一个文件。
  * `a`：打开一个文件并在原内容后面追加内容，如果文件不存在，则新建一个文件。
  * `r+`：以读写模式打开一个文件，打开的位置在文件的开头。
  * `w+`：以读写的模式打开一个文件并把内容清空，如果文件不存在，则新建一个文件。
  * `a+`：以读写的模式打开一个文件并在文件后追加内容，如果文件不存在，则新建一个文件。

`fclose()` 关闭一个流，关闭成功返回 0，否则返回 `EOF`。

> `EOF` 表述文件的结尾，其值等于 -1，ASCII 文件可以使用 `ch == EOF` 来判断 `ch` 是否是文件的结尾，但二进制文件最好使用 `feof(ch)` 来判断。

2. 无格式 I/O

* `fgetc()` & `fputc()` 

```c
int fgetc(FILE *stream);
int fputc(int ch, FILE *stream);
```

`fgetc()` 从指定的流 `stream` 中读取一个字符并返回该字符，每次读取后位置指针向后移一个位置，当读取到文件的结尾返回 `EOF`。

`fputc()` 向指定流 `stream` 写入一个字符，`ch` 表示写入的字符，写入成功返回输出的字符值，否则返回 `EOF`。

```c
char a = fgetc(stdin);
fputc(a, stdout);
```

与此有相同作用的是 `getc()` 和 `putc()`，区别是二者使用宏定义实现，避免了函数调用的操作，但是不能使用具有副作用的表达式。

* `fgets()` & `fputs()` 

```c
char *fgets(char *str, int count, FILE *stream);
int fputs(const char *str, FILE *stream);
```

`fgets()` 从指定流 `stream` 中读取最多 `count` 个字符到字符串 `str` 中，读取成功返回 `str`，否则返回 `NULL`。

`fputs()` 将字符串 `str` 写入指定流 `stream`，写入成功返回非负数，否则返回 `EOF`。

```c
char a[10];
fgets(a, 5, stdin);
fputs(a, stdout);
```

与此有相同作用的是 `gets()`（C11 已移除） 和 `puts()`，区别是二者使用宏定义实现，避免了函数调用的操作，但是不能使用具有副作用的表达式。

* `fread()` & `fwrite()`

```c
size_t fread(void *buffer, size_t size, size_t count, FILE *stream);
size_t fwrite(const void *buffer, size_t size, size_t count, FILE *stream);
```

`fread()` 从给定流 `stream` 读取至多 `count` 个对象，每个对象的读取大小为 `size` 字节，并将读取的内容保存在 `buffer` 中。读取成功返回读取的对象数。

`fwrite()` 写入 `count` 个来自 `buffer` 的对象到流 `stream`，每个对象的大小为 `size`，写入成功返回写入的对象数。

3. 格式化 I/O

* `fscanf()` & `fprintf()`

```c
int fscanf(FILE *stream, const char *format, ...);
int fprintf(FILE *stream, const char *format, ...);
```

`fscanf()` 从指定流 `stream` 进行格式化读取。`format` 是格式化控制符，`...` 是可变参数。

`fprintf()` 向指定流 `stream` 进行格式化写入。`format` 是格式化控制符，`...` 是可变参数。

`scanf()` 和 `printf()` 是 `fscanf()` 和 `fprintf()` 的简化版本，不需要第一个参数，只能是标准输入流和标准输出流。

* `sscanf()` & `sprintf()`

```c
int sscanf(const char *buffer, const char *format, ...);
int sprintf(char *buffer, const char *format, ...;
```

`sscanf()` 从指定字符串 `buffer` 进行格式化读取。`format` 是格式化控制符，`...` 是可变参数。

`sprintf()` 向指定字符串 `buffer` 进行格式化写入。`format` 是格式化控制符，`...` 是可变参数。

### 延伸：POSIX I/O

本节将介绍 UNIX/Linux 系统上的 I/O 系统调用，通过 POSIX C 库的接口实现，帮助理解系统调用。

* `open()`

```c
int open(const char *filename, int flags[, mode_t mode]);
```

返回文件描述符，一般从 3 开始，因为 0 表示标准输入、1 表示标准输出、2 表示标准错误，打开失败返回小于 0 的数。

* `read()`

```c
ssize_t read(int file_descriptor, void *buffer, size_t count);
```

将文件最多 `count` 个字符读取到 `buffer` 中，返回读到的数据量，读取失败返回小于 0 的数。

* `write()`

```c
ssize_t write(int file_descriptor, void *buffer, size_t count);
```

将 `buffer` 中的最多 `count` 个字符写入文件，返回写入的数据量，写入失败返回小于 0 的数。

* `close()`

```c
int close(int file_descriptor);
```

关闭一个文件，关闭成功返回 0，否则返回值小于 0。

Demo：

```c
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main() {
    int fd = open("demo.txt", O_RDWR);
    
    if (fd < 0) {
        return 0;
    } else {
        char str[] = "HelloWorld!";
        write(fd, str, 11);
        char str2[12];
        read(fd, str2, 11);
    }
    close(fd);
    return 0;
}
```

## 内存管理

### 内存组织方式

一个程序并不是连续的存储在内存之中，而是分散在不同的内存段之中：

|区域|作用|
|-|-|
|栈 stack|存储函数的参数值，局部变量值，由编译器进行分配和释放。|
|堆 heap|通过 malloc、new 等方式分配的内存块，编译器不会进行释放，由程序员进行分配和释放。|
|未初始化的数据段 .bss|存储未初始化的或初始化为 0 的全局变量和静态变量。|
|已初始化的数据段 .data|存储已经初始化的全局变量和静态变量。|
|常量段 .rodata|字符串字面量和 const 修饰的全局/静态变量存储在此区。|
|代码段 .text|存储程序的二进制代码（机器语言）。|

![](image/Pasted%20image%2020220526153112.png)

其中 .bss、.data、.rodata 是数据的静态存储区，存储了全局变量、静态变量、字符串字面量，栈和堆是数据的动态存储区。

静态存储区的内容在编译过程中即可确定，而动态存储区的内容则是程序运行时才可以确定。

### 动态内存管理

* `malloc()`

```c
void *malloc(size_t size);
```

在堆上分配 `size` 字节内存，返回指向这段内存的指针。

动态创建一维数组：

```c
int a[10];                                  // 静态创建 int[10]
int *p = (int *)malloc(sizeof (int) * 10);  // 动态创建 int[10]
int *p = (int *)malloc(sizeof (int [10]));  // 同上
```

动态创建多维数组（以二维数组为例）：

```c
int a[2][3];                                            // 静态创建 int[2][3]
int (*p)[3] = (int (*)[3])malloc(sizeof (int) * 6);     // 动态创建 int[2][3]
int (*p)[3] = (int (*)[3])malloc(sizeof (int [2][3]));  // 同上
```
* `calloc()`

```c
void *calloc(size_t num, size_t size);
```

在堆上创建一个 `num` 长度的数组，数组元素的字节数为 `size`，并初始化每一位为 0。

* `realloc()`

```c
void *realloc(void *p, size_t new_size);
```

将之前由 `malloc()`、`calloc()` 或 `realloc()` 分配的内存重新分配为 `new_size` 字节。

在 GCC 编译器下使用 `realloc()`，如果原内存区域后面有足够的空间就可以直接追加内存。如果原内存区域后面没有足够的空间用来追加，则会重新开辟一段内存，并将原来的数据拷贝至新的内存区域，然后再追加内存。

* `free()`

```c
void free(void *ptr); 
```

释放之前由 `malloc()`、`calloc()` 或 `realloc()` 分配的内存。

被释放内存的指针其实依然可以访问这段内存，这是不合理的，是一种野指针。故在 `free()` 之后应把指针置为 `NULL`。

Demo：

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *a = (int *)malloc(8 * sizeof (int));
    int *b = (int *)calloc(10, sizeof (int));
    for (int i = 0; i < 8; ++i)
        a[i] = i;
    int *c = (int *)realloc(a, 20 * sizeof (int)); // breakpoint 
    int *d = (int *)realloc(c, 5 * sizeof (int));
    free(d);
    free(b);
    return 0;
}
```

在第 9 行（注释处）打断点进行调试，运行代码命中断点，查看数组 `a` 和数组 `b` 的值：

![](image/Pasted%20image%2020220723203442.png)

单步调试到下一行 `int *d = (int *)realloc(c, 5 * sizeof (int));`，查看数组 `c` 的值：

![](image/Pasted%20image%2020220723203615.png)

可见 `realloc()` 函数在原来内存的基础上扩充了 12 个 0。

单步调试至下一行 `free(d);`，查看数组 `d` 的值：

![](image/Pasted%20image%2020220723203707.png)

可见 `realloc()` 函数在原来内存基础上缩减了内存。

单步执行至下一行 `free(b)`，查看数组 `d` 的值：

![](image/Pasted%20image%2020220723204515.png)

此时 `d` 的内存已经被释放，所以打印出的是脏数据。

### 缓冲区溢出

1. 栈溢出

```c
#include <string.h>
int main() {
    char src[] = "12345";
    char des[] = "123";
    strcpy(des, src);
}
```

运行程序，没有报错，使用 GDB 调试，查看局部变量：

```bash
(gdb) info locals
src = "5\000\063\064\065"
des = "1234"
```

发生了栈溢出。

2. 堆溢出


## C 标准库及常用库函数



## 参考文献

* cppreference. C reference[EB/OL]. [https://en.cppreference.com/w/](https://en.cppreference.com/w/)
* Computer System: A Programmer's Perspective
* Trevis Rothwell, James Youngman. The GNU C Reference Manual[EB/OL] .[https://www.gnu.org/software/gnu-c-manual/](https://www.gnu.org/software/gnu-c-manual/)
* Richard M. Stallman, Roland McGrath, Andrew Oram, and Ulrich Drepper. The GNU C Library Reference Manual[EB/OL]. [https://www.gnu.org/software/libc/manual/](https://www.gnu.org/software/libc/manual/)
* C Primer Plus
