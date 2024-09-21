# Graphical user interface

## Display server
Xorg is the public, open-source implementation of the X Window System (commonly X11, or X). It is required for running applications with graphical user interfaces (GUIs).

Xorg can be installed with the xorg-server package.
```shell
$ sudo pacman -S xorg-server xorg-apps
```

## Display drivers

First, find the family of your card (e.g. NV110, NVC0, etc.) on nouveau wiki's code names page corresponding to its model/official name obtained with:

```
$ lspci -k | grep -A 2 -E "(VGA|3D)"
```

Then, install the appropriate driver for your card:
- For laptop with hybrid Intel/Nvidia graphics
```
$ sudo pacman -S xf86-video-nouveau
$ sudo pacman -S nvidia-open
```
- For 
```
sudo pacman -S nvidia-open
```
