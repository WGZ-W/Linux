# Make的基本用法

- [Make的基本用法](#make的基本用法)
  - [自动变量](#自动变量)
  - [模式规则](#模式规则)
  - [.PHONY](#phony)
## 自动变量
```
demo.o : demo.c
    gcc $< -o $@

$< ：第一个条件
$@ ：目标
```

## 模式规则
```
% : %.c
    gcc $< -o $@

使用方法：
    make test（有 test.c）
```

## .PHONY
```
.PHONY : clean
clean :
    rm -rf *.o
```
> `无论是否有 clean 文件，都执行 clean 命令`