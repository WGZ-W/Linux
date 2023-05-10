# Configure input methods
[Fcitx5 Arch wiki](https://wiki.archlinux.org/title/Fcitx5)

## 1. Installation
```
$ sudo pacman -S fcitx5-im fcitx5-chinese-addons
```
install basic framework, models, engines, and the configuration tool

## 2. Append environment variables
```
# vim  /etc/environment

GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```


## 3. Autostart
```
# vim ~/.xinitrc
fcitx5 &
```

## 4. Configuration
run `fcitx5-configtool` and put the `pinyin` engine into the candidate.