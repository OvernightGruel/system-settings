## system 
### network
- systemctl enable dhcpcd

### user
- useradd -m -G wheel clay --> passwd clay --> ln -s /usr/bin/vim /usr/bin/vi --> visudo

### nvidia
- pacman -S nvidia nvidia-utils 
- nvidia-xconfig 

### package
- pacman -S wget man base-devel xorg xorg-apps 

### time
  - timedatectl set-local-rtc 1

### sound
  - yay -S alsa-utils
  - [alsamixer](https://xlui.me/t/archlinux-xfce4-alsa/)

### font
- pacman -S ttf-meslo-nerd-font-powerlevel10k nerd-fonts-terminus

## v2ray
- yay -S v2ray --> vim /etc/v2ray/config.json --> sudo systemctl start v2ray.service --> sudo systemctl enable v2ray.service
- vim /usr/share/applications/google-chrome.desktop "--proxy-server="socks5://127.0.0.1:10808""
- yay -S privoxy --> vim /etc/privoxy/config "forward-socks5 / 127.0.0.1:10808 ." --> vim .zshrc "export http\_proxy=http://127.0.0.1:8118 export https\_proxy=http://127.0.0.1:8118" --> sudo systemctl start privoxy --> sudo systemctl enable privoxy

## dwm
- yay -S picom feh
- /bin/startdwm
- ~/.Xresources
- ~/.xinitrc  --> run "startx" start dwm
- ~/.zprofile --> autostartdwm
- /etc/systemd/system/getty@tty1.service.d/override.conf  --> autologin 
