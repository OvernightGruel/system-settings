## 安装

- UEFI 
  - e
  - quiet splash nomodeset
  - F10

- English & Minimum Install & Graphics driver

## 系统设置

- 修改源

- 转中文

- 系统设置

- 关闭sudo密码

  ```bash
  sudo visudo
  #  %sudo ALL=(ALL:ALL) ALL             ---->            %sudo ALL=(ALL:ALL) NOPASSWD:ALL
  ```

- 双系统时间不一致

  ```bash
  sudo apt install ntpdate
  sudo ntpdate http://cn.pool.ntp.org
  sudo hwclock --localtime --systohc
  ```

## 软件

- sudo apt-get install vim git screenfetch

- oh my zsh

  ```bash
  # zsh
  sudo apt-get update
  sudo apt-get install zsh
  chsh -s $(which zsh)
  reboot
  
  # oh my zsh
  sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  # 替代方案
  wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
  sh install.sh
  
  # 主题 https://github.com/romkatv/powerlevel10k
  # 1. Install the recommended font. Optional but highly recommended.
  # 2. GNOME Terminal (the default Ubuntu terminal): Open Terminal → Preferences and click on the selected profile under Profiles. Check Custom font under Text Appearance and select MesloLGS NF Regular.
  # 3.git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
  # 4.Set ZSH_THEME="powerlevel10k/powerlevel10k" in ~/.zshrc and run "source ~/.zshrc"
  
  # 插件
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  # Set plugins=(其他插件名 autojump zsh-autosuggestions zsh-syntax-highlighting) in ~/.zshrc and run "source ~/.zshrc"
  ```

- v2ray

  ```bash
  wget https://github.com/v2ray/v2ray-core/releases/download/v4.28.2/v2ray-linux-64.zip
  sudo mkdir /usr/local/v2ray
  sudo unzip v2ray-linux-64.zip -d /usr/local/v2ray/ && sudo rm -f v2ray-linux-64.zip
  sudo cp /usr/local/v2ray/config.json /usr/local/v2ray/config_bak.json
  sudo vim /usr/local/v2ray/config.json
  
  sudo vim /usr/local/v2ray/systemd/system/v2ray.service  # ExecStart=/usr/local/v2ray/v2ray -config /usr/local/v2ray/config.json
  sudo cp /usr/local/v2ray/systemd/system/v2ray.service /lib/systemd/system
  sudo systemctl start v2ray.service
  sudo systemctl enable v2ray.service
  # 设置 网络 网络代理 手动 socks主机 127.0.0.1:10808
  ```

- chrome

- docker

  ```bash
  sudo apt-get update
  sudo apt-get install \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg-agent \
      software-properties-common
      
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  sudo apt-key fingerprint 0EBFCD88
  
  sudo add-apt-repository \
     "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) \
     stable"
     
  sudo apt-get update
  sudo apt-get install docker-ce docker-ce-cli containerd.io
  sudo chmod 666 /var/run/docker.sock
  ```

- 网易云音乐

  ```bash
  QT_SCALE_FACTOR=2 netease-cloud-music
  ```

- PyCharm Pro

- Typra

- Anaconda

  ```bash
  # https://www.anaconda.com/products/individual
  sh Anaconda3-2020.07-Linux-x86_64.sh
  # copy ~/.bashrc conda initialize to ~/.zshrc and run "source ~/.zshrc"
  
  # 换源
  1. pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
  2. vim ~/.condarc
  # channels:
  #  - defaults
  # show_channel_urls: true
  # channel_alias: https://mirrors.tuna.tsinghua.edu.cn/anaconda
  # default_channels:
  #   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  #   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  #   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  #   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro
  #   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
  # custom_channels:
  #   conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  #   msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  #   bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  #   menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  #   pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  #   simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
   3. conda clean -i
  ```

- cuda

  ```bash
  # https://developer.nvidia.com/cuda-toolkit-archive
  wget http://developer.download.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda_10.1.243_418.87.00_linux.run
  sudo sh cuda_10.1.243_418.87.00_linux.run
  vim ~/.zshrc
  # export PATH=/usr/local/cuda-10.1/bin:$PATH
  # export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64
  nvcc -V 
  
  # test
  cd /usr/local/cuda-10.1/samples/1_Utilities/deviceQuery
  sudo make
  ./deviceQuery  # PASS
  ```

- cuDNN

  ```bash
  # https://developer.nvidia.com/rdp/cudnn-download
  # Download cuDNN v8.0.4 (September 28th, 2020), for CUDA 10.1
  # cuDNN Runtime Library for Ubuntu18.04 (Deb)
  sudo dpkg -i libcudnn8_8.0.4.30-1+cuda10.1_amd64.deb
  
  # arch
  sudo cp cuda/include/cudnn*.h /usr/local/cuda/include
  sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
  sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
  ```

## 美化

- prepare

  ```bash
  sudo apt install gnome-tweak-tool chrome-gnome-shell
  ```

- https://extensions.gnome.org/

  - User Themes

- https://www.pling.com/s/Gnome/browse/

  - GTK3 theme & shell  https://www.pling.com/s/Gnome/p/1403328/
  - icon  https://www.pling.com/p/1405756/
  - cursor  https://www.pling.com/p/1355701/
  - grub  https://www.pling.com/s/Gnome/p/1307852/
