---
    title: APUE
    date: 2022-09-05 15:00:48
    tags:
---

使用 apue.h 所需要的库



## how to compile the .c file with "apue.h"
```
    $> gcc myls.c -I ~/apue/apue.3e/include \ 
    -L ~/apue/apue.3e/lib -lapue
```

- -I means specifying the directory to search the include "file".  

- -L means specifying the directory to search the libraries.  

- -llibrary is to search the liblibrary.a or liblibrary.so file.  

