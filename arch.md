## Arch Settings
- systemctl enable dhcpcd
- useradd -m -G wheel clay --> passwd clay --> ln -s /usr/bin/vim /usr/bin/vi --> visudo
- pacman -S wget man base-devel nvidia nvidia-utils xorg ttf-meslo-nerd-font-powerlevel10k 
- nvidia-xconfig 

## system
- timedatectl set-local-rtc 1

## v2ray
- yay -S v2ray --> vim /etc/v2ray/config.json --> sudo systemctl start v2ray.service --> sudo systemctl enable v2ray.service
- google-chrome-stable --proxy-server="socks5://127.0.0.1:10808"
- yay -S privoxy --> vim /etc/privoxy/config "forward-socks5 / 127.0.0.1:10808 ." --> vim .zshrc "export http\_proxy=http://127.0.0.1:8118 export https\_proxy=http://127.0.0.1:8118" --> sudo systemctl start privoxy --> sudo systemctl enable privoxy
