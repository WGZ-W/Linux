

# 
## 
### Set the console keyboard layout and font
For example, to use one of the largest fonts suitable for HiDPI screens, run:
```shell
# setfont ter-132b
```

### Connect to the internet
- Ensure your network interface is listed and enabled
```shell
# ip link
```

- Wi-Fi—authenticate to the wireless network using iwctl. 

To get an interactive prompt do:
```shell
# iwctl
```
First, if you do not know your wireless device name, list all Wi-Fi devices:
```shell
[iwd]# device list
```
Then, to initiate a scan for networks (note that this command will not output anything):
```shell
[iwd]# station [name] scan
```
You can then list all available networks:
```shell
[iwd]# station [name] get-networks
```
Finally, to connect to a network:
```shell
[iwd]# station [name] connect [SSID]
```
To display the connection state, including the connected network of a Wi-Fi device:
```shell
[iwd]# station [device] show
```

- The connection may be verified with ping:
```shell
# ping archlinux.org
```

### Update the system clock
```shell
# timedatectl
```

### Partition the disks
```shell
# lsblk
```
The following partitions are required for a chosen device:
- One partition for the root directory `/`.
- For booting in UEFI mode: an EFI system partition.

Use a partitioning tool like fdisk to modify partition tables. For example:
```shell
# gdisk /dev/[the_disk_to_be_partitioned]
```

> Note:
> - Take time to plan a long-term partitioning scheme to avoid risky and complicated conversion or re-partitioning procedures in the future.
> - If the disk from which you want to boot `already has an EFI system partition`, do not create another one, but use the existing partition instead.
> - Swap space can be set on a swap file for file systems supporting it.

### Format the partitions
For example, to create an XFS file system on `/dev/root_partition`, run:
```shell
# mkfs.xfs /dev/root_partition
```
If you created a partition for swap, initialize it with mkswap(8):
```shell
# mkswap /dev/swap_partition
```
If you created an EFI system partition, format it to FAT32 using mkfs.fat(8).
> Warning: Only format the EFI system partition if you created it during the partitioning step. If there already was an EFI system partition on disk beforehand, reformatting it can destroy the boot loaders of other installed operating systems.
```shell
# mkfs.fat -F 32 /dev/efi_system_partition
```
Other mount points, such as `/efi`, are possible, provided that the used boot loader is capable of loading the kernel and initramfs images from the root volume. 
> Warning: To successfully boot Arch, the boot loader needs access to the kernel and initramfs image(s) which typically reside in the `/boot` directory.


### Mount the file systems
Mount the root volume to `/mnt`. For example, if the root volume is `/dev/root_partition`:
```shell
# mount /dev/root_partition /mnt
```
Create any remaining mount points under /mnt (such as /mnt/boot for /boot) and mount the volumes in their corresponding hierarchical order.
> Tip: Run mount(8) with the `--mkdir` option to create the specified mount point. Alternatively, create it using mkdir(1) beforehand.
For UEFI systems, mount the EFI system partition:
```shell
# mount --mkdir /dev/efi_system_partition /mnt/efi
```
For boot volume to `/mnt/boot `. 
```shell
# mount --mkdir /dev/boot_partition /mnt/boot
```
If you created a swap volume, enable it with swapon(8):
```shell
# swapon /dev/swap_partition
```

## Installation
### Select the mirrors
Packages to be installed must be downloaded from mirror servers, which are defined in `/etc/pacman.d/mirrorlist`. 

The higher a mirror is placed in the list, the more priority it is given when downloading a package. If it is not, edit the file accordingly, and move the geographically closest mirrors to the top of the list, although other criteria should be taken into account.

This file will later be copied to the new system by pacstrap, so it is worth getting right.

### Check the key
```shell
# pacman -S archlinux-keyring
```
### Install essential packages
> Note: No software or configuration (except for /etc/pacman.d/mirrorlist) gets carried over from the live environment to the installed system.

Use the pacstrap(8) script to install the base package, Linux kernel and firmware for common hardware:
```shell
# pacstrap -K /mnt base linux linux-firmware
```

The base package does not include all tools from the live installation, so installing more packages may be necessary for a fully functional base system. To install other packages or package groups, append the names to the pacstrap command above (space separated) or use pacman to install them while chrooted into the new system. In particular, consider installing:
- CPU microcode updates—`amd-ucode` or `intel-ucode`—for hardware bug and security fixes,
- a console text editor (e.g vim) to allow editing configuration files from the console,
- packages for accessing documentation in man and info pages: man-db, man-pages and texinfo.
```shell
# pacman -S vim 
```

## 3 Configure the system
### 3.1 Fstab
Generate an fstab file (use `-U` or `-L` to define by UUID or labels, respectively):
```shell
# genfstab -U /mnt >> /mnt/etc/fstab
```
Check the resulting `/mnt/etc/fstab` file, and edit it in case of errors.

### 3.2 Chroot
Change root into the new system:
```shell
# arch-chroot /mnt
```

### 3.3 Time
Set the time zone:
```shell
# ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
Run hwclock(8) to generate /etc/adjtime:
```shell
# hwclock --systohc   同步时间
```
This command assumes the hardware clock is set to UTC.

### 3.4 Localization
Edit `/etc/locale.gen` and uncomment en_US.UTF-8 UTF-8 and other needed locales. Generate the locales by running:
```shell
# locale-gen
```
Create the locale.conf(5) file, and set the LANG variable accordingly:
```shell
# vim /etc/locale.conf
LANG=en_US.UTF-8
```
If you set the console keyboard layout, make the changes persistent in vconsole.conf(5):  
在安装完成之后，永久修改字体 `/etc/vconsole.conf`，需要创建该文件
```shell
# vim /etc/vconsole.conf
FONT=LatGrkCyr-12x22
```

### 3.5 Root password
Set the root password:
```shell
# passwd
```

### 3.6 Boot loader
Choose and install a Linux-capable boot loader. If you have an Intel or AMD CPU, enable microcode updates in addition.
```shell
# pacman -S man-db man-pages texinfo
# pacman -S grub efibootmgr intel-ucode os-prober

# systemctl enable NetworkManager.service
# systemctl start NetworkManager.service
```

