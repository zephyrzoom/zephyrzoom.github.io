cfdisk
if choose gpt type, need a boot partation. or dos type.
mkfs.ext4 /dev/sda1
mkswap /dev/sda2 && swapon /dev/sda2
mount /dev/sda1 /mnt
vi /etc/pacman.d/mirrorlist
pacstrap /mnt base base-devel net-tools
genfstab -U -p /mnt >> /mnt/etc/fstab
arch-chroot /mnt
vi /etc/locale.gen && locale-gen
echo LANG="en_US.UTF-8" >> /etc/locale.conf
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
echo 707host >> /etc/hostname
mkinitcpio -p linux
passwd
pacman -S grub-bios
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
exit
umount /mnt
reboot
useradd -m -g users -G wheel -s /bin/zsh im707
passwd im707
systemctl enable dhcpcd
visudo
pacman -S zsh git vim
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
pacman -S xorg-xinit xorg-server i3 virtualbox-guest-utils xf86-video-fbdev xf86-video-vesa xorg-server-utils xf86-video-intel xf86-video-nouveau
edit .xinitrc exec i3
mount virtualbox-addition.iso
use lsblk find it and then mount to some dir
install gcc make perl linux-headers
install dmenu wqy-microhei ttf-dejavu rxvt-unicode
create file ~/.Xresources fill with this, http://codepad.org/8hO4Gt5t.
xrdb ~/.Xresources
