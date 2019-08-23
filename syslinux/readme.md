
###### Get Syslinux binaries:
```

$ curl -o ~/syslinux-6.03.zip https://www.kernel.org/pub/linux/utils/boot/syslinux/syslinux-6.03.zip
$ unzip -d ~/syslinux-latest -x syslinux-6.03.zip
```

###### Create folder for chainloading from:
```
$ mkdir ~/pxeboot

$ cd ~/syslinux-latest/bios

$ cp core/pxelinux.0 ~/pxeboot
$ cp memdisk/memdisk ~/pxeboot
$ cp com32/menu/menu.c32 ~/pxeboot
$ cp com32/mboot/mboot.c32 ~/pxeboot
$ cp com32/chain/chain.c32 ~/pxeboot
```

###### Get your favourite netinstall .iso:
```
$ mkdir -P ~/pxeboot/images/ubuntu
$ co ~/pxeboot/images/ubuntu
$ curl -o ubuntu-16.10-x64-netboot.iso \
    http://archive.ubuntu.com/ubuntu/dists/yakkety/main/installer-amd64/current/images/netboot/mini.iso


    $ mkdir ~/pxeboot/pxelinux.cfg
```

###### Crate a simple menuentry for syslinux:
```
    $ echo '
    default menu.c32
    prompt 0
    timeout 300

    ONTIMEOUT chainlocal

    LABEL local
        MENU LABEL Boot local harddisk
        LOCALBOOT 0

    LABEL chainlocal
        MENU LABEL Chain boot local harddisk
        KERNEL chain.c32
        APPEND hd0

    LABEL ubuntu
        MENU LABEL Ubuntu 16.10 x64 Yakkety Yak (57 MB)
        KERNEL memdisk
        INITRD images/ubuntu/ubuntu-16.10-x64-netboot.iso
        APPEND iso raw
    ' > ~/pxeboot/pxelinux.cfg/default
```


###### Done!
