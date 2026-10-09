---
tags: Instruction
---
# WSL Ubuntu-24.04 安装引导
## 对于Window用户
> 请确保你的Window为最新版

## 步骤1：启用WSL功能
打开PowerShell（以管理员身份运行）：

按Win + X，然后选择Windows PowerShell（管理员）。

启用WSL和虚拟机平台：

``` =powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
重启计算机。

## 步骤2：安装Ubuntu 24.04
> 安装其它版本只需更改 ```wsl --install <Distro>``` 中 <Distro> 对应得发行版。 可通过命令 ``` wsl --list --online ``` 查询。==安装其它版本请确保你了解其特性。==

安装Ubuntu 24.04请使用
``` =powershell
wsl --install -d Ubuntu-24.04 
```

按提示完成安装。

## 步骤3：更新apt
win+R打开运行框，输入wsl回车以打开wsl界面
``` =shell
sudo apt update
sudo apt upgrade
```
当你安装包时出现问题请先尝试更新apt。
    
## 步骤4: 安装C++相关必要的包。
``` 
sudo apt install build-essential cmake g++ gdb
```
~~对于MacOS的用户
**Best Answer:使用虚拟机运行Ubuntu**~~
