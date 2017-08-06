---
layout: post
title: 安装Archlinux
date: 2016-11-22 00:00:00 +0800
---

`cfdisk`分区选择GPT时，需要另外分一个boot分区；若选择DOS类型，需要将一个分区标记为boot

`mkfs.ext4 /dev/sda1`

`mkswap /dev/sda2 && swapon /dev/sda2`

`mount /dev/sda1 /mnt`

`vi /etc/pacman.d/mirrorlist`将中国的源放在前面

`pacstrap /mnt base base-devel net-tools`

`genfstab -U -p /mnt >> /mnt/etc/fstab`

`arch-chroot /mnt`

`vi /etc/locale.gen && locale-gen`选择解注en_US.UTF-8和zh_CN.UTF-8

`echo LANG="en_US.UTF-8" >> /etc/locale.conf`

`ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`

`echo 707host >> /etc/hostname`

`mkinitcpio -p linux`

`passwd`

`pacman -S grub-bios`

`grub-install /dev/sda`

`grub-mkconfig -o /boot/grub/grub.cfg`

`exit`

`umount /mnt`

`reboot`

`useradd -m -g users -G wheel -s /bin/zsh im707`

`passwd im707`

`systemctl enable dhcpcd`

`visudo`

`pacman -S zsh git`

`reboot`用上面设置的新账号登录

`sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`

`pacman -S vim xorg-xinit xorg-server i3 xf86-video-fbdev xf86-video-vesa xorg-apps xf86-video-intel xf86-video-nouveau dmenu wqy-microhei ttf-dejavu rxvt-unicode fcitx fcitx-configtool fcitx-gtk3 fcitx-qt5 feh gcc make linux-headers`

编辑`~/.xinitrc`加入：

```
VBoxClient-all &
[[ -f ~/.Xresources ]] && xrdb -merge -I$HOME ~/.Xresources

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"

fcitx &
feh --bg-scale .bg.jpg &

exec i3
```

点击Virtualbox下的Devices->install guest additions cd image。
`lsblk`浏览挂载设备

`mkdir ~/Tmp`

`mount /dev/sr0 ~/Tmp`挂载分区

`sudo ./VBoxLinuxAdditions.run`安装vbox增强功能

新建`~/.zprofile`加入：

```zsh
if [ -z "$DISPLAY" ] && [ -n "$XDG_VTNR" ] && [ "$XDG_VTNR" -eq 1 ]; then
	exec startx
fi
```

`reboot`重启之后VBoxClient-all才会生效

**必须在执行`startx`之前创建`.xinitrc`，否则会报`vboxvideo module not found`错误**

安装浏览器，设置`.Xresources`
新建`~/.Xresources`，填充以该[内容](http://codepad.org/8jSRXgMC)

下载个图片改名为`.bg.jpg`放在用户根目录当壁纸

`usermod -aG vboxsf im707`使得vbox的共享文件可访问
