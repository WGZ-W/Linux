# 简单的GDB使用

- [简单的GDB使用](#简单的gdb使用)
  - [开始调试](#开始调试)
  - [退出正在调试的函数](#退出正在调试的函数)
  - [打印](#打印)
    - [打印数组](#打印数组)
    - [打印内存的值](#打印内存的值)
    - [使用 "$_" 和 "$__"变量](#使用-_-和-__变量)
    - [打印调用栈帧中变量的值](#打印调用栈帧中变量的值)
    - [打印源代码行](#打印源代码行)
    - [打印结构体](#打印结构体)
    - [打印变量的类型和所在文件](#打印变量的类型和所在文件)
    - [按照派生类打印对象](#按照派生类打印对象)
  - [设置断点](#设置断点)
  - [使用参数](#使用参数)
  - [显示调用栈](#显示调用栈)
  - [多进程/线程](#多进程线程)
    - [查看线程信息](#查看线程信息)
    - [只允许一个线程运行](#只允许一个线程运行)
    - [使用 "$_thread" 变量](#使用-_thread-变量)
    - [使用 "$_exitcode" 变量](#使用-_exitcode-变量)

## 开始调试
```
(gdb) start     从头开始调试 

(gdb) r         开始运行到第一个断点
                从 main 开始

(gdb) c         继续执行

```

## 退出正在调试的函数
```
(gdb) finish    函数继续执行完，打印返回值

(gdb) return    不继续执行，直接返回
(gdb) return exp    修改函数的返回值
```

## 打印
```
(gdb) p [value]     打印内置变量的值

(gdb) x/s str       打印字符串的值（str是字符串指针）
```

### 打印数组
```
(gdb) p array

(gdb) set print elements [number-of-elements]   设置打印的个数
(gdb) set print elements 0                      没有限制
(gdb) p array[index]@num           index 是数组索引
                                    num 是连续多少个元素
(gdb) p *array@num                 表示从头开始打印连续的 num 个元素

(gdb) set print array-indexs on    打印索引下标
```

### 打印内存的值
```
(gdb) x/nfu addr    

含义是， 以 f 格式打印从 addr 开始的 n 个单元长度为 u 的内存值。
          
        n ： 输出单元的个数
        f ： 输出格式，x 是 16 进制输出，o 是 8 进制输出， t 是二进制
        u ： 表明单元的长度， b 是一个 byte ，h 是两个 byte ，w 是四个 byte， g 是八个 byte。

```

### 使用 "$_" 和 "$__"变量
> `"x" 命令会把最后检查的内存地址存在 "$_" 中, 并且把这个地址中的内容放在 "$__" 中。`


### 打印调用栈帧中变量的值
- 可以先切换到该栈帧中，然后打印
```
(gdb) bt
(gdb) f 1
(gdb) p b  
```
- 也可以直接打印
```
(gdb) p func2::b
```
`对于C++的函数名，需要用单引号括起来`

### 打印源代码行
```
(gdb) list -/+                  向上或者向下打印
(gdb) list main/function        打印指定函数
(gdb) list [number, number]     打印指定范围或者指定行数
```

### 打印结构体
```
(gdb) set print pretty on     打印规范的结构体
```

### 打印变量的类型和所在文件
```
(gdb) whatis variable           查看变量的类型

(gdb) ptype variable            查看详细的类型信息

(gdb) info variables name       
查看定义变量的文件（不会显示局部变量，即使是 static），所有包含匹配该表达式的变量

(gdb) info variables ^name$     正则表达式

```

### 按照派生类打印对象
```
(gdb) set print object on
```


## 设置断点
```

(gdb) i b           查看所有断点

(gdb) d [number]    删除编号为 number 的断点

```

## 使用参数
```
$>  gdb --args ./demo args1 args2
```

## 显示调用栈
```
(gdb) bt              显示函数调用栈

(gdb) frame           显示当前函数的栈

(gdb) bt full         显示所有栈中的局部变量的值

(gdb) bt full n       从内向外显示 n 个栈帧

(gdb) bt full -n      从外向内显示 n 个栈帧

(gdb) info locals     显示当前函数局部变量值

```

## 多进程/线程

### 查看线程信息
```
(gdb) info threads
```
### 只允许一个线程运行
用 GDB 调试多线程程序时，一旦程序断住，所有的线程都处于暂停状态。此时，当你调试其中一个线程时（比如执行 “step”， “next” 命令），所有的线程都会同时执行。
```
(gdb) set scheduler-locking on    让其他的线程暂停执行
```

### 使用 "$_thread" 变量
> $_thread 变量保存当前正在调试的线程号。
```
(gdb) watch variable      设置观察点，当 variable 的值发生变化，程序会暂停， \
                          接下来在 commands 语句中执行
```

### 使用 "$_exitcode" 变量
这个变量会记录程序退出时的返回值。
```
(gdb) p $_exitcode    
```




