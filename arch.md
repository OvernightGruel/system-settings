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
# reflector 
reflector --verbose -c China -l 30 --sort rate --save /etc/pacman.d/mirrorlist

# timedate
timedatectl set-local-rtc 1

# grub windows
pacman -S os-prober ntfs-3g
mkdir -p /boot/EFI/Windows
mount /dev/sda1 /boot/EFI/Windows
vim /etc/default/grub
# GRUB_DISABLE_OS_PROBER=false
os-prober
grub-mkconfig -o /boot/grub/grub.cfg

# essential packages
pacman -S wget man git base-devel openssh

# nvidia
pacman -S nvidia nvidia-utils 
nvidia-xconfig

# user
useradd -m -G wheel zwq
passwd zwq
ln -s /usr/bin/vim /usr/bin/vi
visudo

# ---------------------------------------------------

# xorg
- pacman -S xorg

# dwm
git clone https://github.com/OvernightGruel/dwm.git
make clean install
vim startdwm && cp startdwm /usr/bin

# startx with xinit
pacman -S xorg-xinit
cp /etc/X11/xinit/xinitrc ~/.xinitrc
vim ~/.xinitrc # exec startdwm
vim ~/.Xresources # set dpi
startx

# Automatic startx with xinit
vim ~/.bash_profile (copy from /etc/skel/.bash_profile) or ~/.zprofile

# Automatic login
vim /etc/systemd/system/getty@tty1.service.d/override.conf

# ------------------------------------------------------

# v2ray
yay -S v2ray --> vim /etc/v2ray/config.json --> sudo systemctl start v2ray.service --> sudo systemctl enable v2ray.service
vim /usr/share/applications/google-chrome.desktop "--proxy-server="socks5://127.0.0.1:10808""
yay -S privoxy --> vim /etc/privoxy/config "forward-socks5t / 127.0.0.1:10808 ." --> vim .zshrc "export http\_proxy=http://127.0.0.1:8118 export https\_proxy=http://127.0.0.1:8118" --> sudo systemctl start privoxy --> sudo systemctl enable privoxy

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
