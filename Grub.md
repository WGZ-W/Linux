## Arch + Windows 双启动
- [Arch Linux+Windows双启动配置教程](https://zhuanlan.zhihu.com/p/471374126)

> 注意是在安装完 `Arch` 并且重启之后

先安装 `os-prober`
```
# pacman -S os-prober
```

查找 `Windows` 分区
```
# fdisk -l
```

将 Windows 的 EFI 分区挂载到 /mnt
```
# mount /dev/efi_partition /mnt
```

启用 `os-prober` ，将注释去除
```
# vim /etc/default/grub
GRUB_DISABLE_OS_PROBER=false
```

使用 `os-prober` 检测
```
# os-prober
```

备份，防止错误操作
```
# cp /boot/grub/grub.cfg /boot/grub/grub.cfg.bak
```

重新生成配置文件
```
# grub-mkconfig -o /boot/grub/grub.cfg
```

## 美化 Grub 启动器

- [更改 GRUB 主题](https://zhuanlan.zhihu.com/p/558980080)  
- [How To Configure GRUB2 Boot Loader Settings](https://ostechnix.com/configure-grub-2-boot-loader-settings-ubuntu-16-04/)
- [GRUB themes](https://github.com/jacksaur/Gorgeous-GRUB)


将主题文件夹移入 /boot/grub/themes/
```
# sudo cp -r demos/ /boot/grub/themes/
```

并给 `themes` 目录分配适当的权限：
```
$ sudo chown $USER /boot/grub/themes/
```

修改 `/etc/default/grub`
```
# vim /etc/default/grub
GRUB_THEME=/path/theme.txt
```