版本选择

个人物理机：Ubuntu Desktop (ubuntu-24.04.4-desktop-amd64.iso)

VMware虚拟机：(ubuntu-24.04.4-live-server-amd64.iso)

- 只运行特定服务：Ubuntu Server (Minimal)

- 通用服务器使用：Ubuntu Server

WSL2: (ubuntu-24.04.4-wsl-amd64.wsl)

# 查看系统信息

<details>
<summary>查看内存硬件信息</summary>

```bash
sudo apt install dmidecode

# 查看内存槽的数目、哪个槽位插了内存以及内存的大小。
sudo dmidecode | grep -P -A5 "Memory\s+Device" | grep Size | grep -v Range

# 查看最大支持的内存容量。
sudo dmidecode | grep -P 'Maximum\s+Capacity'

# 查看内存槽上内存的速率，Unknown表示该位置未插内存条。
sudo dmidecode | grep -A16 "Memory Device" | grep 'Speed'
```

</details>

<details>
<summary>查看电池电量</summary>

```bash
# 安装工具
sudo apt update
sudo apt install upower

# 列出所有电源设备
# 输出可能会包含类似/org/freedesktop/UPower/devices/battery_BAT0的信息
upower -e

# 查看电池状态
upower -i /org/freedesktop/UPower/devices/battery_BAT0

# 简化输出信息
upower -i /org/freedesktop/UPower/devices/battery_BAT0 | grep -E "state|percentage"
```

</details>

# 修改系统配置

<details>
<summary>修改软件源</summary>

```bash
# ubuntu 24.04.4 LTS 清华源
sudo echo -e "Types: deb\nURIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu\nSuites: noble noble-updates noble-backports\nComponents: main restricted universe multiverse\nSigned-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg" | sudo tee /etc/apt/sources.list.d/tsinghua.sources > /dev/null
```

```bash
# ubuntu 22.04 LTS
# 备份当前的源列表
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

# 编辑源列表文件
sudo vim /etc/apt/sources.list

# 替换为阿里云的源，将如下内容加入到文件中
deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse

# 更新软件源和软件包
sudo apt update
sudo apt upgrade

```

```bash
# 备份当前的源列表
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

# 编辑源列表文件
sudo vim /etc/apt/sources.list

# 替换为阿里云的源，使用sed命令替换源
sudo sed -i 's|http://.*.ubuntu.com|http://mirrors.aliyun.com|g' /etc/apt/sources.list
sudo sed -i 's|http://security.*.ubuntu.com|http://mirrors.aliyun.com|g' /etc/apt/sources.list

# 更新软件源和软件包
sudo apt update
sudo apt upgrade
```

</details>

<details>
<summary>.bash_aliases</summary>

```bash
# python
export pybfsu=https://mirrors.bfsu.edu.cn/pypi/web/simple
export pyustc=https://mirrors.ustc.edu.cn/pypi/simple

# system

# list directory
alias llh='ls -lh'
alias list='ls -lha'

# docker
alias d='docker'
alias di='docker images'
alias dil='docker images | sed "s|registry.cn-shenzhen.aliyuncs.com/haroldfinch|\$ali|g"'
alias drm='docker rm'
alias drmi='docker rmi'
alias drmf='docker rm -f'
alias dps='docker ps'
alias dpsa='docker ps -a'

export ali=registry.cn-shenzhen.aliyuncs.com/haroldfinch

alias dif='docker images --format "\nRepository: {{.Repository}}\nTag: {{.Tag}}\nImage ID: {{.ID}}\nCreated: {{.CreatedAt}}\nSize: {{.Size}}" | sed "s|registry.cn-shenzhen.aliyuncs.com/haroldfinch|\$ali|g"'

alias dpsal='docker ps -a --format "\nContainer ID: {{.ID}}\nImage: {{.Image}}\nCommand: {{.Command}}\nCreated: {{.CreatedAt}}\nStatus: {{.Status}}\nPorts: {{.Ports}}\nContainer Name: {{.Names}}\n"'
alias dpsl='docker ps --format "\nContainer ID: {{.ID}}\nImage: {{.Image}}\nCommand: {{.Command}}\nCreated: {{.CreatedAt}}\nStatus: {{.Status}}\nPorts: {{.Ports}}\nContainer Name: {{.Names}}\n"'
```

</details>

<details>
<summary>修改用户密码</summary>

```bash
# 修改 root 用户密码（初始密码）
sudo passwd root
```

</details>

<details>
<summary>修改终端字体大小</summary>

```bash
# 1.打开配置文件
sudo vim /etc/default/console-setup

# 2.修改文件内容
FONTSIZE="8x16"
FONTSIZE="10x20"
FONTSIZE="12x24"
FONTSIZE="16x32"

# 3.保存关闭文件后，使配置生效
sudo setupcon
```

</details>

<details>
<summary>修改登录后的欢迎信息</summary>

修改 `/etc/update-motd.d/`目录下的脚本内容来更改登录后的欢迎信息

</details>

<details>
<summary>修改终端显示方向</summary>

```bash
# 1.打开配置文件
vim /etc/default/grub

# 2.修改文件内容
GRUB_CMDLINE_LINUX=""
GRUB_CMDLINE_LINUX="fbcon=rotate:1" # 向右旋转90度
GRUB_CMDLINE_LINUX="fbcon=rotate:3" # 向左旋转90度

# 3.保存并关闭文件后，更新 GRUB 配置以应用更改
update-grub

# 4.重新启动系统以应用新的显示设置
reboot
```

</details>


<details>
<summary>自定义 Bash 提示符的颜色</summary>

```shell
# 打开配置文件
vim ~/.bashrc

# 添加如下内容
# [\033[0;32m]表示将用户名与主机名设置为绿色
# [\033[0;34m]表示将当前工作目录设置为淡蓝色
# [\033[00m]用于重置颜色
# \$表示提示符
export PS1="\[\033[0;32m\]\u@\h\[\033[00m\]:\[\033[0;34m\]\w\[\033[00m\]\\$ "

# 保存文件并执行以下命令使更改生效
source ~/.bashrc
```

</details>

<details>
<summary>修改时间和时区</summary>

```bash
# 查看当前系统的时区设置
timedatectl

# 修改时区设置
sudo timedatectl set-timezone "Asia/Shanghai"
```

</details>

<details>
<summary>蓝牙配置</summary>

```bash
sudo apt update
sudo apt install bluez

bluetoothctl

# 列出已经配对的蓝牙设备
[bluetooth]# devices

# 查看特定设备的连接状态
[bluetooth]# info XX:XX:XX:XX:XX:XX

# 列出所有蓝牙适配器
[bluetooth]# list

# 查看适配器的详细信息
[bluetooth]# show

systemctl status bluetooth
```

</details>

<details>
<summary>修改开机搜寻网络时间</summary>

开机时出现这样的界面，

```plain
A start job is running for wait for Network to be Configured (2s / no limit)
```

解决办法

```bash
cd /etc/systemd/system/network-online.target.wants/
vim systemd-networkd-wait-online.service

# 改为如下配置
TimeoutStartSec=2sec
```

保存退出后，开机重启

</details>

# 软件安装

<details>
<summary>Docker</summary>

```bash
# 1.检查卸载老版本 docker
sudo apt remove docker docker-engine docker.io containerd runc

# 2.更新软件包
sudo apt update
sudo apt upgrade

# 3.安装 docker 依赖
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg lsb-release

# 4.添加 docker 密钥
# 使用curl下载GPG密钥
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg -o docker-ce.gpg
# 将下载的GPG密钥转换为适合trusted.gpg.d目录的格式
sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-ce-archive-keyring.gpg docker-ce.gpg
# 删除之前下载的未处理的GPG密钥文件（可选）
rm docker-ce.gpg

# 5.添加阿里云 docker 软件源
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"

# 6.安装docker
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# 7.配置用户组然后重启系统
sudo usermod -aG docker $USER
sudo reboot

# 8.检验 docker 是否安装成功
systemctl start docker
service docker restart
docker run hello-world
```

</details>

<details>
<summary>quickq VPN</summary>

```bash
# 先准备一个空目录
mkdir /root/quickq
cd /root/quickq
# 下载主程序文件
curl -k -o linux-quickq.tar.gz https://d.qwe6.link/quickq/download/linux-quickq.tar.gz
# 备用下载地址
# https://d.aa246.cc:666/dl/linux-quickq.tar.gz
# 解压
tar -zxvf linux-quickq.tar.gz
# 设置启动权限
chmod +x linux-quickq-server
# 启动客户端。注意，首次启动时耗时较长，需要自动下载核心程序文件。如果核心程序更新失败，可以手动下载解压
./linux-quickq-server

# 如何测试socks5端口
curl --socks5-hostname 127.0.0.1:10010 https://www.google.com.hk/
# 如何测试http端口
curl -x 127.0.0.1:11000 https://www.google.com.hk/
```

如需指定地区和标签

```bash
# 执行命令
./linux-quickq-server --help

# 输出如下
Version: 138
------------------------------
QuickQ Server
------------------------------

--route=1 || 3

    1 = Intelligent routing mode, direct connection to domestic websites
    3 = Global routing mode

--country=JP

    The country code is taken from the "Alpha-2 code" in the link "https://en.wikipedia.org/wiki/ISO_3166-1"

--tag=xx

    The tag parameter should be passed into an int value
    1   Netflix自制剧
    2   DMM

# 根据上面的帮助，比如想指定智能路由 + 地区为香港，可以如下调用
./linux-quickq-server --route=1 --country=HK
```

如需后台运行, 请不要使用 nohup

```bash
# 先安装模块
apt install screen -y
# 启动时, 自己定义目录和执行程序, 要先cd, 然后再执行
screen -dmS quickq
screen -x -S quickq -p 0 -X stuff "cd /root/quickq && ./linux-quickq-server"
screen -x -S quickq -p 0 -X stuff $'\015'
# 不使用时, 如何退出?
screen -x -S quickq -p 0 -X stuff $'^D' && sleep 3 && screen -S quickq -X quit
```

运行日志如下

```bash
Connect --> 新加坡 - VIP2387
Work Dir:  /root/test
Kernel PID: 1902452
Socks5 Port: 10010
Http Port: 11000

--------------------
Press "Ctrl+D" to exit
--------------------

2022/07/11 18:22:51 [Info] Reading config: ./config.qc
2022/07/11 18:22:51 [Warning] service started
2022/07/11 18:23:27 tcp:127.0.0.1:59550 accepted tcp:www.google.com.hk:443 [agentout]
2022/07/11 18:23:41 tcp:127.0.0.1:39924 accepted tcp:www.baidu.com:443 [agentout]
```

</details>

<details>
<summary>ping</summary>

```bash
sudo apt install iputils-ping
```

</details>

<details>
<summary>gcc 与 g++编译器</summary>

`build-essential`软件包是一个元软件包，它会安装一系列基本的开发工具，包括 `gcc`编译器、`g++`编译器等。

```bash
sudo apt update
sudo apt install build-essential
```

</details>

<details>
<summary>Open vSwitch</summary>

```bash
sudo apt update
sudo apt install openvswitch-switch
sudo systemctl start openvswitch-switch
sudo systemctl enable openvswitch-switch # 设置开机自启动
sudo systemctl status openvswitch-switch
```

</details>

<details>
<summary>FreeRDP远程连接Windows10</summary>

```bash
sudo apt update
sudo apt install freerdp2-x11

# 验证是否安装成功
xfreerdp /version

xfreerdp /u:用户名 /p:密码 /v:IP地址 /w:1920 /h:1080

# 推荐用这个，动态调整分辨率
xfreerdp /u:用户名 /p:密码 /v:IP地址 /dynamic-resolution

# scale 进行缩放显示，Windows只有100，140，180三个值
xfreerdp /u:用户名 /p:密码 /v:IP地址 /dynamic-resolution /scale:180
```

</details>

<details>
<summary>Rust</summary>

```bash
# 安装基础工具
sudo apt update
sudo apt install build-essential -y

# 下载并运行官方安装脚本
# 使用 rustup 镜像 清华源
export RUSTUP_DIST_SERVER=https://mirrors.tuna.tsinghua.edu.cn/rustup
export RUSTUP_UPDATE_ROOT=https://mirrors.tuna.tsinghua.edu.cn/rustup/rustup
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# 配置环境变量
source $HOME/.cargo/env

# 验证安装
rustc --version
cargo --version
rustup --version
```

</details>

<details>
<summary>uv</summary>

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

</details>

<details>
<summary>Google Chrome</summary>

```bash
# 1: Update Your System
sudo apt update
sudo apt upgrade

# 2: Setup GPG Keys
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmour -o /usr/share/keyrings/chrome.gpg

# 3: Adding Chrome PPA to System
sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/chrome.gpg]  \
http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list'

# 4: Installing Google Chrome
sudo apt update
sudo apt install google-chrome-stable

# 5: Launch Chrome Application
google-chrome
```

</details>

<details>
<summary>vim</summary>

```shell
set showmatch         " Highlight parentheses

set number            " set line number

set cindent           " C-style indent

set autoindent

set tabstop=4         " set tab width
set softtabstop=4
set shiftwidth=4      " The uniform indentation is 4

syntax on
```

</details>

<details>
<summary>tmux</summary>

```powershell
# set scroll screen like vim
setw -g mode-keys vi

# session index from 1
set -g base-index 1

# pane index from 1, not 0
set -g pane-base-index 1

# vim style to move cursor in pane
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
```

```bash
tmux source-file ~/.tmux.conf
```

</details>

<details>
<summary>fcitx5</summary>

```bash
# Uninstall old input method
sudo apt purge fcitx* ibus*

# Install
sudo apt install fcitx5 fcitx5-chinese-addons fcitx5-pinyin

sudo reboot
```

</details>

# shell

<details>
<summary>ubuntu.base.sh</summary>

```bash

```

</details>

<details>
<summary>ubuntu.rust.sh</summary>

```bash

```

</details>

<details>
<summary>ubuntu.python.sh</summary>

```bash

```

</details>
