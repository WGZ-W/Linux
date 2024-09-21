# 安装窗口管理器

```
# pacman -S xorg xorg-server xorg-xinit
```
- [简单配置窗口管理器](https://zhuanlan.zhihu.com/p/408552473)  

## 下载
使用 `dwm` 作为窗口管理器  
```
$ git clone https://git.suckless.org/dwm
```

## 配置
Editing 修改 `config.def.h`，delete 删除 `config.h` ，使用 use `sudo make clean install` 重新编译安装就可以配置好 `dwm` 了。

## 启动
```
$ vim ~/.xinitrc
exec dwm
```

# 安装终端模拟器

- [简单配置终端模拟器](https://zhuanlan.zhihu.com/p/406153971)  

## 下载

使用 `st` 作为模拟终端
```
$ git clone https://git.suckless.org/st
```
## 配置
Editing 修改 `config.def.h`，删除 `config.h` ，使用 `sudo make clean install` 重新编译安装就可以配置好 `st` 了。

## 半透明状态（alpha）
首先，我们需要安装一个系统依赖项 `picom`。
```
$ sudo pacman -S picom
```
安装完毕之后还需要用 `picom &` 来启动它，可以将该语句加入 `.xinitrc` 当中，让我们在每次启动图形界面的同时自动启动它。
```
$ vim .xinitrc
picom &
```  

从 `suckless` 的官网上下载 `最新` 的 `alpha` 补丁  
- [Alpha diff](https://st.suckless.org/patches/alpha/)

然后 `cd` 到 `st` 的源文件目录下，使用 `patch` 命令把补丁文件应用到配置文件上。
```
$ patch  < /path/to/patch.diff
```

`alpha` 补丁在 `config.def.h` 当中添加了一个 `alpha`变量，大家可以修改该数值取得不同程度的透明效果，数值越小越透明，数值越大越不透明。
# feh 背景
## Download
```
$ sudo pacman -S feh
```

## Setting
```
$ feh --bg-fill /path/image
```

## Running in the background
```
$ vim .xinitrc
~/.fehbg &
```

# .xinitrc
```
~/.fehbg &
picom &
exec dwm
```

# System profile
```
$ sudo pacman -S neofetch
```
To use, type
```
$ neofetch
```