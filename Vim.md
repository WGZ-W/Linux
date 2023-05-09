# 我的 vim 使用
一些vim的基础用法
#
- [我的 vim 使用](#我的-vim-使用)
  - [搜索并且替换](#搜索并且替换)
  - [读入输入](#读入输入)
  - [编辑多个文件](#编辑多个文件)
  - [配置文件vimrc](#配置文件vimrc)
  - [自动补全](#自动补全)
  - [高级命令 for programmers](#高级命令-for-programmers)
    - [取消自动缩进( insert 模式)](#取消自动缩进-insert-模式)
    - [插入缩进](#插入缩进)
    - [<Tab> 的使用](#tab-的使用)
    - [Modelines](#modelines)
  - [程序中跳转](#程序中跳转)
## 搜索并且替换
```
    :s/foo/bar/g    匹配到的单词全部替换

    :s/foo/bar/gi   大小写敏感
    
    :4,8s/foo/bar/g
    
    :%s/foo/bar/g   整个文件
```

## 读入输入
```
    :r filename    读入文件

    :r !ls      读入命令的结果

    :r          读入当前文件
```

## 编辑多个文件
```
    :n      编辑下一个文件
    :N      编辑上一个文件

    :rew    回到第一个文件

    :args   列出打开的文件，标记出正在使用的文件
```

## 配置文件[vimrc](./vimrc.html)
```
    set number

    set relativenumber  设置相对的行号（比较nb）

```

## 自动补全
```
- Ctrl-P	    向上文搜索
- Ctrl-N		向下文搜索
- Ctrl-X		选择搜索类型
  	- Ctrl-D	宏定义包括 include 文件
  	- Ctrl-I	当前文件和 include 文件
  	- Ctrl-F	查找文件名字，当前目录中
  	- Ctrl-L	复制当前文件中的一行
```
```  
 :set ignorecase		设置补全忽略大小写
 :set infercase		    推断补全的大小写，不只是匹配
 :set complete=.,d,i,w,u,b,t		设置补全的范围
 :set dictionary=file,file,...	    指定搜索目录
```


## 高级命令 for programmers
### 取消自动缩进( insert 模式)
```
  Ctrl-D    取消一级缩进

  0Ctrl-D   取消所有级别缩进，光标返回第一列
            下一行光标也从第一列开始

  ^Ctrl-D   取消所有级别缩进，光标返回第一列
            下一行缩进级别不变 
```

### 插入缩进
```
  Ctrl-T    添加缩进，value = shiftwidth
  <Tab>     value = tabstop

  Ctrl-T 和 Ctrl-D 配合使用
```


### <Tab> 的使用
```
  set softtabstop=4     tab 移动4
  set tabstop=4         <Tab> 为4
  set shiftwidth=4
```

### Modelines
```
  /*vim:tabstop=8:expandtabs:shiftwidth=8*/
```
> 必须是这种结构，而且在第一行或者后五行中，vim自动生成这种
> 配置
  
## 程序中跳转
```
  '%',  在C语言中的花括号中对应的跳转

  'gD',   跳转到该文件中的变量定义位置

  '[I',   查看标识符的定义位置
  'Ctrl-O'  返回到标识符
```

