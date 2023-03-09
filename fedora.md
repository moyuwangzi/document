# fedora基本使用

## 换源

### 备份

```sh
sudo mkdir /etc/yum.repos.d/backup
sudo cp -r /etc/yum.repos.d/*.repo /etc/yum.repos.d/backup/
```

### 清华源

```sh
sudo vi /etc/yum.repos.d/fedora.repo 

[fedora]
name=Fedora $releasever - $basearch
failovermethod=priority
baseurl=https://mirrors.tuna.tsinghua.edu.cn/fedora/releases/$releasever/Everything/$basearch/os/
metadata_expire=28d
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False

sudo vi /etc/yum.repos.d/fedora-updates.repo

[updates]
name=Fedora $releasever - $basearch - Updates
failovermethod=priority
baseurl=https://mirrors.tuna.tsinghua.edu.cn/fedora/updates/$releasever/Everything/$basearch/
enabled=1
gpgcheck=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False

sudo vi /etc/yum.repos.d/fedora-modular.repo

[fedora-modular]
name=Fedora Modular $releasever - $basearch
failovermethod=priority
baseurl=https://mirrors.tuna.tsinghua.edu.cn/fedora/releases/$releasever/Modular/$basearch/os/
enabled=1
metadata_expire=7d
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False

sudo vi /etc/yum.repos.d/fedora-updates-modular.repo

[updates-modular]
name=Fedora Modular $releasever - $basearch - Updates
failovermethod=priority
baseurl=https://mirrors.tuna.tsinghua.edu.cn/fedora/updates/$releasever/Modular/$basearch/
enabled=1
gpgcheck=1
metadata_expire=6h
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
skip_if_unavailable=False

sudo dnf makecache
```

### 阿里源

```sh
sudo wget -O /etc/yum.repos.d/fedora.repo http://mirrors.aliyun.com/repo/fedora.repo
sudo wget -O /etc/yum.repos.d/fedora-updates.repo http://mirrors.aliyun.com/repo/fedora-updates.repo

sudo yum makecache
```

## 查看哈希值

```sh
Windows
certutil -hashfile 文件名  SHA256
Linux
md5sum/sha1sum/sha256sum 文件名
```

## yum/dnf基本使用

dnf命令基本与yum一致，下面yum改成dnf即可，实际上yum已经被重定向至dnf了

### 更新

```sh
yum check-update                # 检查所有可用的软件包升级
yum check-update <package>      # 检查指定软件包是否可升级

yum upgrade                     # 升级所有软件包
yum upgrade <package>           # 升级指定软件包
```

### 安装

```bash
yum install <package>           # 安装软件包
yum reinstall <package>         # 重新安装软件包
```

### 卸载清理

```shell
yum remove <package>            # 卸载软件包
yum autoremove                  # 删除所有最初作为依赖项安装的不需要的软件包

yum clean packages              # 清除所有在可用仓库目录中缓存的所有软件包
yum clean dbcache               # Yum 使用时会创建一些 SQLite 数据库文件在可用的仓库中, 此命令将清楚缓存的副本
yum clean metadata              # 从可用仓库中清除所有缓存的 XML 元数据
yum clean expire-cache          # 清除过期的缓存
yum clean all                   # 从所有可用仓库中清除所有缓存文件（包括清除上面几条命令清理的若干数据）

```

### 查询/搜索 软件包

```shell
# yum list 命令格式
yum list [available|installed|extras|updates|obsoletes|all|recent] [pkgspec]

# 参数说明:
#       available       列出所有可安装的软件包 (系统的可用仓库中)
#       installed       列出所有已安装的软件包
#       extras          列出所有已安装但已不再可用仓库中的软件包
#       updates         列出所有已安装的软件包在可用仓库中可更新的软件包, 类似 yum check-update
#       obsoletes       列出所有在可用软件包和已安装软件包之间过时的依赖关系
#       all             列出在软件包仓库(未安装)和已安装的所有软件包 (yum list 不加参数默认为 all 参数)
#       recent          列出过去 7 天内添加到可用仓库中的所有软件包
#
#       pkgspec         操作指定的软件包, 没有该指定则默认操作所有包


yum repolist                            # 列出已配置的软件包仓库

yum info <package>                      # 查询软件包的详细信息

yum whatprovides COMMAND                # 查询某个命令属于哪个软件包

yum search <string>                     # 搜索软件包
yum search <string> | grep "package"    # 过滤搜索结果

rpm -i package.rpm                      # 离线安装已下载到本地的 rpm 软件包
```

## 内置可用环境

```sh
Available Environment Groups:
   Fedora Custom Operating System
   Minimal Install
   Fedora Server Edition
   Fedora Cloud Server
   Xfce Desktop
   LXDE Desktop
   LXQt Desktop
   Cinnamon Desktop
   MATE Desktop
   Sugar Desktop Environment
   Deepin Desktop
   Budgie Desktop
   Development and Creative Workstation
   Web Server
   Infrastructure Server
   Basic Desktop
   i3 desktop
Installed Environment Groups:
   Fedora Workstation
   KDE Plasma Workspaces
Installed Groups:
   Administration Tools
   Container Management
   LibreOffice
Available Groups:
   3D Printing
   Audio Production
   Authoring and Publishing
   Budgie
   Budgie Desktop Applications
   C Development Tools and Libraries
   Cloud Infrastructure
   Cloud Management Tools
   Compiz
   D Development Tools and Libraries
   Design Suite
   Development Tools
   Domain Membership
   Editors
   Educational Software
   Electronic Lab
   Engineering and Scientific
   FreeIPA Server
   Games and Entertainment
   Headless Management
   MATE Applications
   Milkymist
   Network Servers
   Neuron Modelling Simulators
   Office/Productivity
   Python Classroom
   Python Science
   Robotics
   RPM Development Tools
   Security Lab
   Sound and Video
   System Tools
   Text-based Internet
   Window Managers
```

## 禁止休眠

```sh
systemctl status sleep.target
systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
systemctl status sleep.target
```

## 常用软件

zsh zim
tmux gcc g++ git curl wget
英伟达驱动
htop btop emacs-nox neovim

```sh
sudo dnf groupinstall "C Development Tools and Libraries"
sudo dnf install tmux git curl wget htop emacs-nox neovim zsh
```

### zsh+zim

```sh
sudo dnf install zsh
chsh -s /usr/bin/zsh
curl -fsSL http://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh
```

修改`vim ~/.zimrc`
末尾添加
`zmodule romkatv/powerlevel10k`

```sh
zimfw install
p10k configure
```

### 字体 nerd fonts

```sh
git clone https://github.com/ryanoasis/nerd-fonts.git --depth 1
cd nerd-fonts
./install.sh
```

### 给命令改名

```sh
alias vim='nvim'
```

### grub 修改

```sh
sudo nvim /etc/default/grub
```

```sh
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```

### 显卡驱动安装

```sh
sudo dnf upgrade
```

启用第三方软件源，或者在软件商店设置处打开
```sh
sudo dnf install fedora-workstation-repositories
sudo dnf config-manager rpmfusion-nonfree-nvidia-driver --set-enable

```

```sh
sudo dnf upgrade --refresh
sudo dnf install gcc kernel-headers kernel-devel akmod-nvidia xorg-x11-drv-nvidia xorg-x11-drv-nvidia-libs xrandr
```

等几分钟等内核模块的自动加载，不要马上进行下一步

```sh
sudo akmods --force
sudo dracut --force

sudo cp -p /usr/share/X11/xorg.conf.d/nvidia.conf /etc/X11/xorg.conf.d/nvidia.conf
```

在`OutputClass`处添加一行

```sh
sudo vim  /etc/X11/xorg.conf.d/nvidia.conf
```

```sh
Option "PrimaryGPU" "yes"
```

如果用的不是gdm，而是sddm，还需要做如下修改

```sh
sudo vim /etc/sddm/Xsetup
```

```sh
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```

重启
测试安装成功与否

```sh
glxinfo | egrep "OpenGL vendor|OpenGL renderer"
```

安装x11-cuda

```sh
xorg-x11-drv-nvidia-cuda
## 测试
nvidia-smi
```

手动用run文件安装(弃用)，选用上述官方[安装手册](https://docs.fedoraproject.org/en-US/quick-docs/how-to-set-nvidia-as-primary-gpu-on-optimus-based-laptops/)方法安装

进不去启动器看这篇[文章](<http://ivo-wang.github.io/2018/02/18/a/>)

## 安装kde桌面环境

标准安装

```sh
## 查看可用的环境组
sudo yum grouplist
## 选择KDE环境
sudo yum -y groupinstall "KDE Plasma Workspaces"
sudo systemctl disable gdm
sudo systemctl enable sddm
## 最好先安装kde，再安装nVidia驱动
sudo reboot
```

## doom emacs安装

依赖包安装(ripgrep fd-find(调用命令为fd) emacs-nox)

```sh
sudo yum install ripgrep fd-find emacs-nox
git clone --depth 1 https://github.com/doomemacs/doomemacs ~/.config/emacs
~/.emacs.d/bin/doom install
```

将doom加入到环境变量

`vim ~/.zshrc`

```sh
export PATH=$PATH:/home/ygz/.emacs.d/bin
```

检测`doom`有无出错
`doom doctor`

### 安装LSP

markdown,shell,c++ 的LSP

```sh
sudo yum install marked
```

官方给的安装方式

```sh
## markdown
sudo npm install -g marked

## 安装shell LSP

sudo yum install shellcheck

## 安装clangd
sudo yum install clang-tools-extra
```

开启c++ LSP功能

打开`init.pl`,快捷键 SPC r p

```sh
:tools 
(lsp +eglot)
:lang (cc +lsp)
```

打开`config.pl`

```sh
(set-eglot-client! 'cc-mode '("clangd" "-j=3" "--clang-tidy"))
```

## gdm自动登录

在`/etc/gdm/custom.conf`文件中添加以下内容

```sh
[daemon]
AutomaticLogin=username
AutomaticLoginEnable=True
```

其中，`username`是要自动登陆的用户名。

说明：`username`不能是`root`，也就说无法实现`root`的自动登陆。

## sddm 主题、自动启动

### 自动登录

```sh
sudo vim /etc/sddm.conf.d/autologin.conf
```

```sh
[Autologin]
User=ygz
Session=plasma.desktop
```

### 主题

主题目录在`/usr/share/sddm/themes`

`sudo vim /etc/sddm.conf.d/theme.conf`

```sh
[Theme]
Current= # 当前主题名称
CursorTheme= # 当前光标主题
DisableAvatarsThreshold=7 设置有多少个用户可以使用头像
EnableAvatars=true # 是否加载头像
FaceDir=/usr/share/sddm/faces # 头像所在目录
Font= #当前字体
Theme=/usr/share/sddm/themes #主题所在目录
```

## 美化GRUB

### debian系

如果grub主题有sh文件就可以直接执行了，事后在更新一下`update-grub`，就可以了
没有`update-grub`命令，只需要执行

```sh
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

> 有的是grub2，那么命令就是这样的

```sh
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

### Fedora

若没有sh文件，那就手动复制到主题文件夹

```sh
cp -r /主题文件/ /boot/grub2/theme
##（不加-r不会复制文件夹）
nano /etc/default/grub
```

注释掉 `GRUB_TERMINAL_OUTPUT="console"`

添加`GRUB_THEME="theme.txt所在的路径/theme.txt"`

如果是 gpt 分区，以 uefi 启动方式启动的话，启动时读取的文件是
`/boot/efi/EFI/fedora/grub.cfg` ，
这个时候更新时需更新这个文件，命令如下：

```sh
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```

> 查看分区格式
> `sudo parted -l`

如果是一般的启动方式，启动时读取的文件是 `/boot/grub2/grub.cfg`，这个时候更新时需更新这个文件，命令如下：

```sh
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

还没有结束！

uefi的Fedora重启之后还会出现字体报错（没错就是那么玄学），虽然只是闪过一秒，但同样看着不舒服

只需要把/boot/grub 里的字体文件连同文件夹复制到uefi的文件夹即可

## 安装fcitx5
按照[fcitx5](https://fedoraproject.org/wiki/I18N/Fcitx5/zh-cn)選擇合適的包
```sh
sudo dnf install fcitx5 fcitx5-chinese-addons fcitx5-qt fcitx5-gtk fcitx5-configtool fcitx5-qt-module fcitx5-autostart fcitx5-lua fcitx5-rime
```
自己配置一下輸入法的基本設置
配置環境
```sh
sudo vim /etc/environment

INPUT_METHOD=fcitx5
GTK_IM_MODULE=fcitx5
QT_IM_MODULE=fcitx5
XMODIFIERS=\@im=fcitx5
SDL_IM_MODULE=fcitx
```
或者在 `~/.xprofile` 写入
```sh
export GTK_IM_MODULE=fcitx5
export QT_IM_MODULE=fcitx5
export XMODIFIERS="@im=fcitx5"

export LANG="zh_CN.UTF-8"
export LC_CTYPE="zh_CN.UTF-8"
```
### 安裝四葉草輸入法(不退推荐了)

#### 從源碼構建

操作系统： linux

python版本： 3

python依赖的库： jieba、pypinyin、opencc、requests
外加一個sudo dnf install opencc-tools

下载工具（三者任意一个均可）： aria2、wget、curl

解压工具（三者任意一个均可）： unzip、bsdtar、7z

rime基础库： librime

rime基础配置： librime-prelude

克隆此[仓库](https://github.com/fkxxyz/rime-cloverpinyin.git)，然后直接执行构建即可
`git clone https://github.com/fkxxyz/rime-cloverpinyin.git`

`./build`
> 根據錯誤信息註釋掉報錯的src中的代碼
完成后，会生成 cache 目录和 data 目录

data 是最终生成的目录
cache 是生成过程中下载和生成的中间文件
其中，执行 build 时，可以有个参数

`/build [minfreq]`
minfreq 代表360万词里面指定的最小词频，频率低于该值的词语会被筛选掉，达到精简词库的目的，默认是100，该值越小，最终生成的词库越大，为 0 表示不精简词库（会生成大约 100 兆左右的词库）。

构建完成后，可以打包，在 data 目录生成发布用的压缩包

`./pack [ver]`
ver 表示版本号，例如 1.1.2

#### 直接下載編譯好的文件
```sh
wget https://github.com/fkxxyz/rime-cloverpinyin/releases/download/1.1.4/clover.schema-build-1.1.4.zip
unzip -q clover.schema-1.1.4.zip -d $HOME/Downloads/clover
```
複製到rime用戶資料文件夾

|平台|	rime用户资料夹位置|
|:-:|:-:|
|ibus|	`~/.config/ibus/rime`|
|fcitx	|`~/.config/fcitx/rime`|
|fcitx5|	`~/.local/share/fcitx5/rime`|
新建文件
```sh
mv $HOME/Downloads/clover/* ~/.local/share/fcitx5/rime
sudo vi ~/.local/share/fcitx5/rime/default.custom.yaml
```
備份好上述文件，修改如下兩行
```sh
  page_size: 8
  schema_list:
    - schema: clover
```

### 安装雾凇拼音(词库更多)

备份好原有的配置
```sh
git clone https://github.com/iDvel/rime-ice.git
```
下载好复制到配置目录`~/.local/share/fcitx5/rime`即可
重新部署一下过一会就好了

或者用如下方法更方便
安装东风破[plum](https://github.com/rime/plum)输入法管理工具
```sh
git clone https://github.com/rime/plum.git
mv ~/Downloads/plum ~/app/
ln -s ~/app/plum/rime-install ~/.local/bin
```
安装更新所有文件
`bash rime-install iDvel/rime-ice:others/recipes/full`
只有词库文件(包含下面三个)
`bash rime-install iDvel/rime-ice:others/recipes/all_dicts`
拼音词库
`bash rime-install iDvel/rime-ice:others/recipes/cn_dicts`
英文词库
`bash rime-install iDvel/rime-ice:others/recipes/en_dicts`
opencc(emoji)
`bash rime-install iDvel/rime-ice:others/recipes/opencc`


### 输入法皮肤

参考github上作者自己的文档
[gruvbox](https://github.com/ayamir/fcitx5-gruvbox)
[nord](https://github.com/tonyfettes/fcitx5-nord)
[dracula](https://github.com/drbbr/fcitx5-dracula-theme)

## 环境变量zshrc文件中

```sh
export rime_dir="$HOME/.local/share/fcitx5/rime"
export PATH=$HOME/.local/bin:$PATH
```

## 安装aria2 且自启动
```sh
audo dnf install aria2

mkdir ~/app/aria2c
touch ~/app/aria2c/aria2.conf ~/app/aria2c/aria2.session
vim ~/app/aria2c/aria2.conf  
```
[aria2.conf](http://aria2c.com/archiver/aria2.conf)
```sh
chmod 777 ~/app/aria2c/aria2.session
## 自启动
在设置里加入这个脚本就算了
sudo aria2c --conf-path=/home/ygz/app/aria2c/aria2.conf -D
```

