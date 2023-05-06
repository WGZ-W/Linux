# 安装 Arch （请同时参考Wiki）

- [安装 Arch （请同时参考Wiki）](#安装-arch-请同时参考wiki)
  - [1 Pre-installation](#1-pre-installation)
    - [1.1 Set the console keyboard layout](#11-set-the-console-keyboard-layout)
    - [1.2 Connect to the internet](#12-connect-to-the-internet)
    - [1.3 Update the system clock](#13-update-the-system-clock)
    - [1.4 Partition the disks](#14-partition-the-disks)
      - [1.4.1 Example layouts](#141-example-layouts)
    - [1.5 Format the partitions](#15-format-the-partitions)
    - [1.6 Mount the file systems](#16-mount-the-file-systems)
    - [1.7（可选）文件验证 (invalid or corrupted package)](#17可选文件验证-invalid-or-corrupted-package)
  - [2 Installation](#2-installation)
    - [2.1 Select the mirrors](#21-select-the-mirrors)
    - [2.2 Install essential packages](#22-install-essential-packages)
  - [3 Configure the system](#3-configure-the-system)
    - [3.1 Fstab](#31-fstab)
    - [3.2 Chroot](#32-chroot)
    - [3.3 Time zone](#33-time-zone)
    - [3.4 Localization](#34-localization)
    - [3.5 Network configuration](#35-network-configuration)
    - [3.6 Root password](#36-root-password)
    - [3.7 Boot loader](#37-boot-loader)
      - [3.7.1 安装引导程序](#371-安装引导程序)
  - [4 Reboot](#4-reboot)
  - [5 Post-installation](#5-post-installation)

## 1 Pre-installation
### 1.1 Set the console keyboard layout
```
# setfont /usr/share/kbd/consolefonts/LatGrkCyr-12x22.psfu.gz
```
Console fonts are located in `/usr/share/kbd/consolefonts/` and can likewise be set with setfont.  
```
# ls /usr/share/kbd/consolefonts/
```



### 1.2 Connect to the internet
Ensure your network interface is listed and enabled, for example with ip-link:
```
# ip link
```
打开网络设备
```
# ip link set "interface" up
```

Wi-Fi—authenticate to the wireless network using iwctl.（详情见Wiki）

```
# iwctl --passphrase "passwd" station "device" connect SSID
```
or Using wpa_supplicant
```
#  ip link

# ip link set wlan0 up     打开WiFi设置

# iwlist wlan0 scan       扫描WiFi结果

# iwlist wlan0 scan | grep ESSID   WiFi名字

# wpa_passphrase 网络名 密码 > 文件名

example：
# wpa_passphrase WJ ..... > internet.conf

# wpa_supplicant -c internet.conf -i wlan0 &
连接互联网，需要分配动态 IP 地址

# dhcpcd &         动态分配 IP 地址
```
The connection may be verified with ping
```
# ping archlinux.org
```

### 1.3 Update the system clock
Use timedatectl(1) to ensure the system clock is accurate:
```
# timedatectl
```

### 1.4 Partition the disks
When recognized by the live system, disks are assigned to a `block device` such as `/dev/sda`, `/dev/nvme0n1` or `/dev/mmcblk0`. To identify these devices, use `lsblk or fdisk`.
```
# fdisk -l
```
Results ending in rom, loop or airoot may be ignored.  
The following partitions are required for a chosen device:  
- One partition for the root directory `/`.
- For booting in UEFI mode: `an EFI system partition`.  

Use `gdisk` or `parted` to modify partition tables. For example:
```
# gdisk /dev/the_disk_to_be_partitioned
```
> Note:
> - If the disk from which you want to boot `already has an EFI system partition`, do not create another one, but use the existing partition instead.

#### 1.4.1 Example layouts
`UEFI with GPT`

| Mount point | Partition | Partition type | Label |Suggested size|
| ----------- | --------- | -------------- | ----- | -------- |
| /mnt/boot | /dev/efi_system_partition | EFI system partition | EF00| At least 300 MiB. If multiple kernels will be installed, then no less than 1 GiB.|  
| [SWAP] | /dev/swap_partition | Linux swap | 8200 | More than 512 MiB |
| /mnt | /dev/root_partition | Linux x86_64 root(/) | 8300 | Remainder of the device |

### 1.5 Format the partitions
Once the partitions have been created, each newly created partition must be formatted with an appropriate `file system`. See `File systems#Create a file system` for details.  
For example, to create an XFS file system on /dev/root_partition, run:
```
# mkfs.xfs -f /dev/root_partition
```
If you created a partition for swap, initialize it with mkswap(8):
```
# mkswap /dev/swap_partition
```
> Note: For stacked block devices replace `/dev/*_partition` with the appropriate block device path.  

If you created an EFI system partition, format it to FAT32 using mkfs.fat(8).
> Warning: Only format the EFI system partition if you created it during the partitioning step. If there already was an EFI system partition on disk beforehand, reformatting it can destroy the boot loaders of other installed operating systems.

```
# mkfs.fat -F 32 /dev/efi_system_partition
```

### 1.6 Mount the file systems
Mount the root volume to /mnt. For example, if the root volume is /dev/root_partition:
```
# mount /dev/root_partition /mnt
```
Create any remaining mount points (such as /mnt/boot) and mount the volumes in their corresponding hierarchical order.
> Tip: Run mount(8) with the --mkdir option to create the specified mount point. Alternatively, create it using mkdir(1) beforehand.

For UEFI systems, mount the EFI system partition:
```
# mount --mkdir /dev/efi_system_partition /mnt/boot
```
If you created a swap volume, enable it with swapon(8):
```
# swapon /dev/swap_partition
```
> 注意： 挂载分区一定要遵循顺序，先挂载根（root）分区（到 /mnt），再挂载引导（boot）分区（到 /mnt/boot 或 /mnt/efi，如果单独分出来了的话），最后再挂载其他分区。否则您可能遇到安装完成后无法启动系统的问题。


### 1.7（可选）文件验证 (invalid or corrupted package)
需要下载，可解决问题
```
# pacman -S archlinux-keyring
```

## 2 Installation
### 2.1 Select the mirrors
文件 `/etc/pacman.d/mirrorlist` 定义了软件包会从哪个镜像下载。

```
## Arch Linux repository mirrorlist
## China

Server = https://mirrors.bfsu.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.cqu.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.dgut.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.neusoft.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.nju.edu.cn/archlinux/$repo/os/$arch
Server = https://mirror.redrock.team/archlinux/$repo/os/$arch
Server = https://mirrors.sjtug.sjtu.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.xjtu.edu.cn/archlinux/$repo/os/$arch

```
在列表中越前的镜像在下载软件包时有越高的优先权。  
这个文件接下来还会被 pacstrap 拷贝到新系统里，所以请确保设置正确。
### 2.2 Install essential packages
> Note: No software or configuration (except for `/etc/pacman.d/mirrorlist`) get carried over from the live environment to the installed system.

Use the pacstrap(8) script to install the base package, Linux kernel and firmware for common hardware:
```
# pacstrap /mnt base linux linux-firmware vim networkmanager
```


## 3 Configure the system

### 3.1 Fstab
Generate an fstab file (use -U or -L to define by UUID or labels, respectively):
```
# genfstab -U /mnt >> /mnt/etc/fstab
```
Check the resulting /mnt/etc/fstab file, and edit it in case of errors.

### 3.2 Chroot
Change root into the new system:
```
# arch-chroot /mnt
```

### 3.3 Time zone
Set the time zone:
```
# ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
Run hwclock(8) to generate /etc/adjtime:
```
# hwclock --systohc   同步时间
```
This command assumes the hardware clock is set to UTC. See System time#Time standard for details.

### 3.4 Localization
Edit /etc/locale.gen and uncomment en_US.UTF-8 UTF-8 and other needed locales. Generate the locales by running:
```
# locale-gen
```
Create the locale.conf(5) file, and set the LANG variable accordingly:
```
# vim /etc/locale.conf
LANG=en_US.UTF-8
```
If you set the console keyboard layout, make the changes persistent in vconsole.conf(5):  
在安装完成之后，永久修改字体 `/etc/vconsole.conf`，需要创建该文件
```
# vim /etc/vconsole.conf
FONT=LatGrkCyr-12x22
```

### 3.5 Network configuration
Create the hostname file:
```
# vim /etc/hostname
myhostname
```
Complete the network configuration for the newly installed environment. That may include installing suitable `network management` software.  
edit the /etc/hosts file for every device in your LAN, see hosts(5)  
Examples
```
# The following lines are desirable for IPv4 capable hosts
127.0.0.1       localhost
# 127.0.1.1 is often used for the FQDN of the machine
127.0.1.1       thishost.example.org   thishost
192.168.1.10    foo.example.org        foo
192.168.1.13    bar.example.org        bar
146.82.138.7    master.debian.org      master
209.237.226.90  www.opensource.org
# The following lines are desirable for IPv6 capable hosts
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters
```

### 3.6 Root password
Set the root password:
```
# passwd
```

### 3.7 Boot loader
Choose and install a Linux-capable boot loader. If you have an Intel or AMD CPU, enable microcode updates in addition.
```
# pacman -S man-db man-pages texinfo

# pacman -S grub efibootmgr intel-ucode os-prober

# systemctl enable NetworkManager.service
# systemctl start NetworkManager.service
```
#### 3.7.1 安装引导程序
选择一个启动引导器标识，在本例被称为 `Arch`。这将在 `esp/EFI/` 中创建一个与标识同名的目录来储存 `EFI` 二进制文件，而且这个名字还会用于在 `UEFI` 启动菜单中区分 `GRUB` 启动项；
执行下面的命令来将 `GRUB EFI` 应用 `grubx64.efi` 安装到 `esp/EFI/Arch/`，并将其模块安装到 `/boot/grub/x86_64-efi/`。  
Then follow the below steps to install GRUB to your disk:
- 1. Mount the EFI system partition and in the remainder of this section, substitute `esp` with its mount point.
- 2. Choose a boot loader identifier, here named `GRUB`. A directory of that name will be created in esp/EFI/ to store the EFI binary and this is the name that will appear in the UEFI boot menu to identify the GRUB boot entry.
- 3. Execute the following command to install the GRUB EFI application grubx64.efi to esp/EFI/GRUB/ and install its modules to /boot/grub/x86_64-efi/.
```
# grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=Arch
```

使用 `grub-mkconfig` 工具来生成 `/boot/grub/grub.cfg`：
```
# grub-mkconfig -o /boot/grub/grub.cfg
```
> 警告： 这是安装的最后一步也是至关重要的一步，请按上述指引正确安装好引导加载程序后再重新启动。否则重启后将无法正常进入系统。


## 4 Reboot
```
输入 exit 或按 Ctrl+d 退出 chroot 环境。

可选用 umount -R /mnt 手动卸载被挂载的分区：这有助于发现任何「繁忙」的分区，并通过 fuser(1) 查找原因。

最后，通过执行 reboot 重启系统，systemd 将自动卸载仍然挂载的任何分区。不要忘记移除安装介质，然后使用 root 帐户登录到新系统。
```

## 5 Post-installation
连接网络
```
# nmtui
```

获取 sudo 命令
```
# pacman -S sudo
```

创建用户
```
# sudo useradd -m "name"
# sudo passwd "name"
# sudo vim /etc/sudoers
```
在 user privilege，将创建的用户和 root 这一行的权限一样

修改镜像源同上
```
# vim /etc/pacman.d/mirrorlist
```