```bash
# 管理员身份打开PowerShell

# 1. 安装并启用WSL功能，完成后按提示重启电脑
wsl --install

# 2. 电脑重启后，确保WSL内核是最新版
wsl --update

# 3. 将 WSL 2 设置为默认版本
wsl --set-default-version 2

# 方法1（推荐）
# 从官网下载 wsl 文件
wsl --install -d Ubuntu-24.04 --location D:\wsl\ubuntu --name ubuntu --version 2
wsl --install --from-file \path\to\xxx.wsl --location D:\wsl\NAME --name NAME --version 2

# 导出虚拟机做备份
wsl --export MyXXXLinux D:\MyXXXLinux-backup.tar

# 导入虚拟机
wsl --import MyXXXLinux D:\WSL\MyXXXLinux D:\MyXXXLinux-backup.tar --version 2

# 删除虚拟机（发行版就是指虚拟机）
wsl --unregister MyRockyLinux

# -d参数指定发行版名字，也就是虚拟机名字
wsl -d MyRockyLinux

# 查看发行版
wsl --list --online

# 查看本机已安装的虚拟机
wsl -l -v
```

VMware与WSL2无法共存，下面是切换方法，注意：以管理员身份在PowerShell中执行，然后重启

```bash
# VMware 正常运行，WSL 2 将无法使用
bcdedit /set hypervisorlaunchtype off

# WSL 2 正常运行，VMware 启动会报错
bcdedit /set hypervisorlaunchtype auto
```
