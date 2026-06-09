# Ubuntu Desktop

![](./img/u.desktop.1.png)

![](./img/u.desktop.2.png)

![](./img/u.desktop.3.png)

![](./img/u.desktop.4.png)

![](./img/u.desktop.5.png)

![](./img/u.desktop.6.png)

![](./img/u.desktop.7.png)

![](./img/u.desktop.8.png)

![](./img/u.desktop.9.png)

![](./img/u.desktop.10.png)

![](./img/u.desktop.11.png)

![](./img/u.desktop.12.png)

![](./img/u.desktop.13.png)

![](./img/u.desktop.14.png)

![](./img/u.desktop.15.png)

![](./img/u.desktop.16.png)

![](./img/u.desktop.17.png)

![](./img/u.desktop.18.png)

![](./img/u.desktop.19.png)

# Ubuntu Server

![](./img/u.server.1.png)

![](./img/u.server.2.png)

![](./img/u.server.3.png)

![](./img/u.server.4.png)

![](./img/u.server.5.png)

![](./img/u.server.6.png)

![](./img/u.server.7.png)

![](./img/u.server.8.png)

![](./img/u.server.9.png)

![](./img/u.server.10.png)

![](./img/u.server.11.png)

![](./img/u.server.12.png)

![](./img/u.server.13.png)

![](./img/u.server.14.png)

![](./img/u.server.15.png)

![](./img/u.server.16.png)

![](./img/u.server.17.png)

![](./img/u.server.18.png)

![](./img/u.server.19.png)

![](./img/u.server.20.png)

![](./img/u.server.21.png)

![](./img/u.server.22.png)

![](./img/u.server.23.png)

# Ubuntu Disk Partition

Desktop 512GB

|挂载目录|容量|类型|备注|
| ----- | -------- | ----- | ----------------------------- |
| /boot | 1G       | FAT32 |
| /     | 150G     | ext4  | 系统 、APT 软件               |
| /var  | 150G     | ext4  | Docker、数据库、KVM/QEMU      |
| /home | 剩下空间 | ext4  | 代码、文档、个人文件          |
| swap  | 不需要   |       | 用 swap 文件代替              |
| /usr  | 不需要   | ext4  | 存放 apt 安装的软件和应用程序 |

VMware Ubuntu Server (minimal) 500GB

|挂载目录|容量      |类型|
| ----- | ---- | ----- |
| /boot | 1G   | FAT32 |
| /     | 300G | ext4  |
| /home | 200G | ext4  |

# Debian Disk Partition

| 挂载目录 | 容量大小 | 类型 | 分区类型 | 备注             |
| -------- | -------- | ---- | -------- | ---------------- |
| /        | 10G      | ext4 | Primary  |                  |
| /boot    | 100M     | ext4 | Primary  | 可以与根分区合并 |
| /usr     | 5G       | ext4 | Logical  | 可以与根分区合并 |
| /var     | 2G       | ext4 | Logical  | 可以与根分区合并 |
| /tmp     | 5G       | ext4 | Logical  | 可以与根分区合并 |
| swap     | 2G       | swap | Logical  |                  |
| /home    | 尽量大些 | ext4 | Logical  |                  |

| 挂载目录 | 容量大小 | 类型 | 分区类型 | 备注             |
| -------- | -------- | ---- | -------- | ---------------- |
| /        | 30G      | ext4 | Primary  |                  |
| /boot    | 512M     | ext4 | Primary  | 可以与根分区合并 |
| /usr     | 20G      | ext4 | Logical  | 可以与根分区合并 |
| /var     | 10G      | ext4 | Logical  | 可以与根分区合并 |
| /tmp     | 5G       | ext4 | Logical  | 可以与根分区合并 |
| swap     | 2G       | swap | Logical  |                  |
| /home    | 尽量大些 | ext4 | Logical  |                  |

| 挂载目录 | 容量大小 | 类型 | 分区类型 | 备注 |
| -------- | -------- | ---- | -------- | ---- |
| /        | 65G      | ext4 | Primary  |      |
| swap     | 2G       | swap | Logical  |      |
| /home    | 尽量大些 | ext4 | Logical  |      |
