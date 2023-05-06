# 修改镜像源

先下载 wget
```
$ sudo pacman -S wget
```

从网上下载全部镜像源
```
# wget -O /etc/pacman.d/mirrorlist https://www.archlinux.org/mirrorlist/all/