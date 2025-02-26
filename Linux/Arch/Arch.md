---
title: Arch
date: 2018-12-21 10:55:04
tags: 
    - Arch
    - 基础
categories: 
    - Linux
---

**目录 start**

1. [Arch](#arch)
    1. [包管理](#包管理)
        1. [Pacman](#pacman)
        1. [snap](#snap)
        1. [Yaourt](#yaourt)
        1. [Yay](#yay)
1. [Tips](#tips)

**目录 end**|_2021-06-11 21:58_|
****************************************

# Arch
> [参考: 为什么 Archlinux 不适合服务器使用](https://www.tuicool.com/articles/byAFZr)
> [参考: Arch Linux的用户都有理想主义倾向吗？](https://www.zhihu.com/question/49439472)
> [参考: ArchLinux你可能需要知道的操作与软件包推荐](https://www.viseator.com/2017/07/02/arch_more/)
> [参考: 长期使用Arch，Gentoo等滚动更新的发行版是怎样的一种体验？](https://www.zhihu.com/question/37720991?sort=created)

- [什么Linux发行版软件最多？](https://www.lulinux.com/archives/2787)
- [Arch Linux 安装、配置、美化和优化](http://www.cnblogs.com/bluestorm/p/5929172.html)

## 包管理
> [Creating Arch Linux Packages](https://www.theurbanpenguin.com/creating-arch-linux-packages/)

### Pacman 
> [Arch wiki: pacman ](https://wiki.archlinux.org/index.php/Pacman_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E5%88%A0%E9%99%A4%E8%BD%AF%E4%BB%B6%E5%8C%85)  
> Arch User Repository （常被称作 AUR），是一个为 Arch 用户而生的社区驱动软件仓库。Debian/Ubuntu 用户的对应类比是 PPA。

/etc/pacman.conf 追加
```conf
    [archlinuxcn]
    #The Chinese Arch Linux communities packages.
    SigLevel = Optional TrustAll
    #Server   = http://repo.archlinuxcn.org/$arch
    Server   = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

1. `pacman-mirrors` generate pacman mirrorlist for Manjaro Linux 

1. -S 安装
1. -R 卸载
    - -Rs 卸载以及没有被其他软件依赖的软件包
1. -Q 查询
    - -Qdt 查询未被依赖的软件包

### snap
> 使用 pacman 安装 
1. sudo pacman -S snapd
1. sudo systemctl enable --now snapd.socket
1. sudo ln -s /var/lib/snapd/snap /snap

- 例如安装 sudo snap install redis-desktop-manager
    - 可执行文件 /snap/bin/redis-desktop-manager.rdm

- 但是国内会很慢，此时可以手动下载安装 [参考: snapInstall](https://kuricat.com/gist/snap-install-too-slow-zmbjy)  
    - curl -H 'Snap-Device-Series: 16' http://api.snapcraft.io/v2/snaps/info/{{packageName}} 例如 `redis-desktop-manager`
    - sudo snap install xxx.snap --dangerous

### Yaourt
> [Arch User Repository](https://wiki.archlinux.org/index.php/Arch_User_Repository)`但是已经暂停开发了`

1. `/etc/pacman.conf` 追加
    ```conf
    [archlinuxcn]
    #The Chinese Arch Linux communities packages.
    SigLevel = Optional TrustAll
    Server   = http://repo.archlinuxcn.org/$arch
    ```
1. `sudo pacman -Syu yaourt` 同步

> 若遇到 签名错误  signature from ... is unknown trust

```sh 
    rm -R /etc/pacman.d/gnupg/
    rm -R /root/.gnupg/ 
    gpg --refresh-keys
    pacman-key --init && pacman-key --populate archlinux manjaro
    pacman-key --refresh-keys
    pacman -Syyu
```

### Yay

- `pacman -S yay` 下一代aur管理

************************

# Tips

- deepin-wine
- [企业微信](https://aur.archlinux.org/packages/deepin-wxwork/)
- [go-for-it](https://aur.archlinux.org/packages/go-for-it/)

- 无法识别 USB设备
    1. 查看是usb模块 `sudo modprobe usb-storage`
    1. 若报错 `modprobe: FATAL: Module usb-storage not found in directory /lib/modules/4.19**`
    1. 查看 `ls /lib/modules` 
    1. 内核滚动升级 grub 没有更新, `update-grub`即可

- ifconfig,route在net-tools中，nslookup,dig在dnsutils中，ftp,telnet等在inetutils中,ip命令在iproute2中。
    - sudo pacman -S net-tools dnsutils inetutils iproute2
