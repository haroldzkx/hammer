# Configure

<details>
<summary>Update Tencent Mirror</summary>

```bash
sed -e 's|^mirrorlist=|#mirrorlist=|g' \
    -e 's|^#baseurl=http://dl.rockylinux.org/$contentdir|baseurl=https://mirrors.tencent.com/rocky|g' \
    -i.bak \
    /etc/yum.repos.d/[Rr]ocky-*.repo

dnf makecache
```

</details>

# Install

<details>
<summary>KDE Desktop</summary>

```bash
# 启用 CRB 仓库（提供大量开发/依赖包）
sudo dnf config-manager --set-enabled crb

# 安装 EPEL 仓库（提供 KDE 相关包）
sudo dnf install epel-release -y

# 刷新缓存
sudo dnf clean all
sudo dnf makecache

# 安装完整 KDE Plasma 环境（官方组名）
sudo dnf groupinstall "KDE Plasma Workspaces" -y

# 禁用 GNOME 的 gdm
sudo systemctl disable gdm --now

# 启用 KDE 的 sddm
sudo systemctl enable sddm --now

# 设置默认启动图形界面
sudo systemctl set-default graphical.target

reboot
```

</details>

<details>
<summary>System Tools</summary>

```bash
sudo dnf groupinstall "Standard" -y
which wget curl unzip zip

sudo dnf groupinstall "Development Tools" -y
gcc --version
make --version
git --version

sudo dnf groupinstall "Headless Management" -y
sudo dnf groupinstall "Legacy UNIX Compatibility" -y
sudo dnf groupinstall "Network Servers" -y
sudo dnf groupinstall "Scientific Support" -y

sudo dnf groupinstall "Security Tools" -y
which aide firewalld

sudo dnf groupinstall "Smart Card Support" -y
rpm -qa | grep pcsc

sudo dnf groupinstall "System Tools" -y
```

</details>

<details>
<summary>Divide Disk</summary>

|特性	|Standard Partition|	LVM|	LVM Thin Provisioning|
|-|-|-|-|
|空间分配	|固定大小，创建后难调整	|预分配，可在线扩容（XFS 不可缩）|	按需分配，虚拟空间可超配|
|灵活性	|极低，调整需离线、易丢数据|	高，跨盘整合、在线扩容、快照|	极高，超配、动态扩容、共享池|
|空间利用率	|低，易闲置浪费	|中，预分配有闲置|	极高，共享物理空间、无闲置|
|性能|	最佳，无抽象层开销|	接近原生，轻微元数据开销|	略低，有元数据与分配开销|
|管理复杂度|	最低，直观易维护|	中，需理解 PV/VG/LV|	高，需监控超配、防空间耗尽|
|高级功能|	无|	快照、条带、镜像|	快照、超配、共享池|
|应用场景|	新手/桌面/高性能首选|	服务器/生产环境首选|	虚拟化/存储池场景|

|Mount Point|Capacity|Patition Type|
|-|-|-|
|/boot|	1G|	标准分区|
|/boot/efi|	512MB|	标准分区|
|/|	150G|	LVM|
|swap|	内存 ≤ 2GB：swap = 2× 内存<br/>2GB < 内存 ≤ 8GB：swap = 内存<br/>8GB < 内存 ≤ 64GB：swap = 1/2 内存<br/>内存 > 64GB：swap = 8GB～16GB	|LVM|
|/home|	剩下的空间|	LVM|
|/data|		LVM| Thin|

```bash
[user@localhost ~]$ lsblk
NAME            MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda               8:0    0 476.9G  0 disk 
├─sda1            8:1    0   488M  0 part /boot/efi
├─sda2            8:2    0     1G  0 part /boot
└─sda3            8:3    0 475.5G  0 part 
  ├─rl_192-root 253:0    0   150G  0 lvm  /
  ├─rl_192-swap 253:1    0    16G  0 lvm  [SWAP]
  └─rl_192-home 253:2    0 309.5G  0 lvm  /home
[user@localhost ~]$ lsblk -o NAME,TYPE,SIZE,MOUNTPOINT,FSTYPE
NAME            TYPE   SIZE MOUNTPOINT FSTYPE
sda             disk 476.9G            
├─sda1          part   488M /boot/efi  vfat				# 标准分区（part）
├─sda2          part     1G /boot      xfs				# 标准分区（part）
└─sda3          part 475.5G            LVM2_member	# LVM物理卷（PV）
  ├─rl_192-root lvm    150G /          xfs				# LVM逻辑卷（lv）
  ├─rl_192-swap lvm     16G [SWAP]     swap				# LVM逻辑卷（lv）
  └─rl_192-home lvm  309.5G /home      xfs				# LVM逻辑卷（lv）
```

</details>
