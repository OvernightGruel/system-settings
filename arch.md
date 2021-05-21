## archlinux 

### network
- systemctl enable dhcpcd

### time
- timedatectl set-local-rtc 1

### user
- useradd -m -G wheel clay --> passwd clay --> ln -s /usr/bin/vim /usr/bin/vi --> visudo

### grub windows
- pacman -S os-prober ntfs-3g
- mount /dev/sda1 /boot/EFI/Windows
- os-prober
- grub-mkconfig -o /boot/grub/grub.cfg

### package base
- pacman -S wget man base-devel openssh 

### nvidia
- pacman -S nvidia nvidia-utils 
- nvidia-xconfig 

### xorg
- pacman -S xorg xorg-apps

### bspwm sxhkd
- pacman -S bspwm sxhkd
- cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm
- cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd
- wget https://sxhkd.service /usr/lib/systemd/system/  &&  systemctl enable sxhkd
- vim ~/.config/sxhkd/sxhkdrc   ----> change base terminal

### startx with xinit
- pacman -S xorg-xinit
- cp /etc/X11/xinit/xinitrc ~/.xinitrc
- vim ~/.xinitrc
- startx 

### startx with lxdm(display manager)
- pacman -S lxdm-gtk3
- vim ~/.dmrc  --> set session
- systemctl enable lxdm
- vim /etc/lxdm/lxdm.conf   ---> change theme: gtk theme in /usr/share/themes; lxdm theme in /usr/share/lxdm/themes

### sound
  - yay -S alsa-utils
  - [alsamixer](https://xlui.me/t/archlinux-xfce4-alsa/)

### font
- pacman -S ttf-meslo-nerd-font-powerlevel10k nerd-fonts-terminus

## v2ray
- yay -S v2ray --> vim /etc/v2ray/config.json --> sudo systemctl start v2ray.service --> sudo systemctl enable v2ray.service
- vim /usr/share/applications/google-chrome.desktop "--proxy-server="socks5://127.0.0.1:10808""
- yay -S privoxy --> vim /etc/privoxy/config "forward-socks5t / 127.0.0.1:10808 ." --> vim .zshrc "export http\_proxy=http://127.0.0.1:8118 export https\_proxy=http://127.0.0.1:8118" --> sudo systemctl start privoxy --> sudo systemctl enable privoxy

## dwm
- yay -S picom feh
- /bin/startdwm
- ~/.Xresources
- ~/.xinitrc  --> run "startx" start dwm
- ~/.zprofile --> autostartdwm
- /etc/systemd/system/getty@tty1.service.d/override.conf  --> autologin 
