# ArchLinux

## installation

```bash
# Verify the boot mode: UEFI
ls /sys/firmware/efi/efivars

# Connect to the internet
ip link
ping baidu.com

# Update the system clock
timedatectl set-ntp true
timedatectl status

# Partition the disks
fdisk -l
fdisk /dev/the_disk_to_be_partitioned

# Format the partitions
mkfs.ext4 /dev/root_partition
mkfs.fat -F 32 /dev/efi_system_partition

# Mount the file systems
mount /dev/root_partition /mnt
mkdir -p /mnt/boot
mount /dev/efi_system_partition /mnt/boot

# Swapfile
dd if=/dev/zero of=/mnt/swapfile bs=1G count=32 status=progress
chmod 600 /mnt/swapfile
mkswap /mnt/swapfile
swapon /mnt/swapfile

# Select the mirrors
vim /etc/pacman.d/mirrorlist

# Install essential packages
pacstrap /mnt base linux linux-firmware vim dhcpcd amd-ucode reflector

# Configure the system
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab
# check /mnt/swapfile none swap defaults 0 0

arch-chroot /mnt

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc

vim /etc/locale.gen
locale-gen
vim /etc/locale.conf 
# LANG=en_US.UTF-8

vim /etc/hostname 
# myhostname（主机名）

vim /etc/hosts
# 127.0.0.1	localhost
# ::1		localhost
# 127.0.1.1	myhostname.localdomain	myhostname # 主机名.本地域名 主机名

passwd

# Grub
pacman -S grub efibootmgr
mkdir -p /boot/grub
grub-mkconfig -o /boot/grub/grub.cfg
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ArchLinux

# dhcpcd
systemctl enable dhcpcd

exit

reboot
```

## Post-installation

```bash
# timedate
timedatectl set-local-rtc 1

# reflector 
reflector --verbose -c China -l 30 --sort rate --save /etc/pacman.d/mirrorlist

# grub windows
pacman -S os-prober ntfs-3g
mkdir -p /boot/EFI/Windows
mount /dev/sda1 /boot/EFI/Windows
vim /etc/default/grub
# GRUB_DISABLE_OS_PROBER=false
os-prober
grub-mkconfig -o /boot/grub/grub.cfg

# essential packages
pacman -S wget man git base-devel openssh unzip

# nvidia
pacman -S nvidia nvidia-utils 
nvidia-xconfig

# user
useradd -m -G wheel zwq
passwd zwq
ln -s /usr/bin/vim /usr/bin/vi
visudo

# ---------------------------------------------------

# v2ray
wget https://github.com/v2fly/v2ray-core/releases/download/v4.45.0/v2ray-linux-64.zip
sudo mkdir /usr/local/v2ray
sudo unzip v2ray-linux-64.zip -d /usr/local/v2ray/ && sudo rm -f v2ray-linux-64.zip
sudo cp /usr/local/v2ray/config.json /usr/local/v2ray/config_bak.json
sudo vim /usr/local/v2ray/config.json

sudo vim /usr/local/v2ray/systemd/system/v2ray.service  # ExecStart=/usr/local/v2ray/v2ray -config /usr/local/v2ray/config.json
sudo cp /usr/local/v2ray/systemd/system/v2ray.service /lib/systemd/system
sudo systemctl start v2ray.service
sudo systemctl enable v2ray.service

vim /etc/profile
# export http\_proxy=http://127.0.0.1:10809
# export https\_proxy=http://127.0.0.1:10809

# --------------------------------------------------

install dtos

vim ~/.Xresources # Xft.dpi: 192

# sddm login manager
vim /etc/sddm.conf
[X11]
ServerArguments=-nolisten tcp -dpi 94
EnableHiDPI=true

[Wayland]
EnableHiDPI=true

# grub
https://draculatheme.com/grub

# ----------------------------------------------------

# paru
https://github.com/Morganamilo/paru

# chrome
google-chrome-stable --proxy-server="socks5://127.0.0.1:10808"

# ----------------------------------------------------


# xorg
- pacman -S xorg

# qtile
pacman -S qtile
cp /usr/share/doc/qtile/default_config.py ~/.config/qtile/config.py

# alacritty

# startx with xinit
pacman -S xorg-xinit
cp /etc/X11/xinit/xinitrc ~/.xinitrc
vim ~/.xinitrc # exec qtile start
vim ~/.Xresources # set dpi

Xft.dpi: 192

! These might also be useful depending on your monitor and personal preference:
Xft.autohint: 0
Xft.lcdfilter:  lcddefault
Xft.hintstyle:  hintfull
Xft.hinting: 1
Xft.antialias: 1
Xft.rgba: rgb

startx

# Automatic startx with xinit
vim ~/.bash_profile (copy from /etc/skel/.bash_profile) or ~/.zprofile

# Automatic login
vim /etc/systemd/system/getty@tty1.service.d/override.conf

# ------------------------------------------------------

# dwm
yay -S picom feh

# fcitx5
pacman -S fcitx5-im fcitx5-rime fcitx5-qt fcitx5-gtk fcitx5-configtool
pacman -S fcitx5-pinyin-zhwiki-rime fcitx5-pinyin-moegirl-rime
vim ~/.pam_environment
# GTK_IM_MODULE DEFAULT=fcitx
# QT_IM_MODULE  DEFAULT=fcitx
# XMODIFIERS    DEFAULT=\@im=fcitx
# SDL_IM_MODULE DEFAULT=fcitx

# config directory ~/.config/fcitx5

# sound
yay -S alsa-utils
[alsamixer](https://xlui.me/t/archlinux-xfce4-alsa/)

# font
pacman -S ttf-meslo-nerd-font-powerlevel10k nerd-fonts-terminus
```

-----

picom-jonaburg-git

alacritty
zsh/ohmyzsh

vim

sxiv 

dmenu
rofi

dunst

i3lock-color&xautolock

conky

Fcitx5 Themes
