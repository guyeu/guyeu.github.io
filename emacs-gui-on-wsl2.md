# 在Windows 10的Linux子系统中安装部署图形界面的emacs的配置记录

> 总得来说这是个很无聊的事，记录一下避免自己忘记。

## Requirements

- 支持虚拟化的Windows电脑；
- 用于Windows的包管理器[scoop](https://scoop.sh/)；
- 足够的网速用来下载；
- 足够的时间用来挥霍；

## Enable & Install WSL2

> 以下翻译自[官方文档](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
> 所有的命令都需要在管理员权限的Powershell中执行。

1. 启用WSL支持
   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```
2. 启用虚拟机
   ```powershell
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
3. 从[这里](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)下载Linux内核更新包，需要以管理员权限安装
4. 设置WSL 2作为默认版本
   ```powershell
   wsl --set-default-version 2
   ```
   
现在可以在[Microsoft Store](https://aka.ms/wslstore)安装里面的发行版了。

## Install Arch Linux

## Initialize Arch Linux

### User & Group

### Mirror & Repository

### Shell

## X410

## Enjoy
