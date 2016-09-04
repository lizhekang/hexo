title: Win10的"正式版"bash使用体验
date: 2016-08-27 22:54:16
categories:
- 工具
- 软件
tags:
- Win10
- 使用体验
---

## Win10的"正式版"bash使用体验

### 0x7c00

早在今年 Build 2016 上，微软就发布了 Bash on Ubuntu on Windows，但是当时只有Win10 Preview 版本才能体验。抱着尝鲜的心态在虚拟机里面安装了 Win10 带 Bash 的 Preview 版本，结果发现各种Bug，没有宣传时候说的“原生的软件包可以直接运行于 Windows 之上”那么美好，所以就一直没有用起来。

终于等来了 Win10 周年纪念版的正式推送，正式体验了一把“正式版” bash。

### 改变

既是惊喜也是意料之中吧，Win10周年纪念版正式推送的Bash for Windows的Bug少了，zsh等软件包不能安装和正常使用的问题都没有了。基本上在ubuntu常用的软件都能正常在源里面安装和使用。

### 使用体验

安装启用 Bash on Ubuntu on Windows 的教程 Google 一下到处都是，在这里就不赘述了。

#### 关于Linux

{% asset_img win10-bash-version.png Ubuntu 版本 %}

不难发现其实 Bash on Ubuntu on Windows（下面简称BUW） 就是一个跑在 Windows 上的 Ubuntu 14.04 系统，是通过 Windows Subsystem for Linux（WSL）实现的，当用户启动开发者模式并启动WSL功能，就能享受运行在 Windows 上的 Ubuntu 了。

#### 关于本地文件

BUW 启动的时候会将 Windows 相应的硬盘分区挂载到 /mnt 目录下，非系统目录可以在当前用户权限下操作，但是对于 Windows 等目录就算在 root 或者 sudo 下还是没有权限做删除、重命名等操作。

#### 关于服务

其实在 Windows 上面使用 Linux （以前是虚拟机）一个很大的需求就是需要一些 Linux 上的服务，例如：Apache、Nginx还有Mysql等，一批在 Linux 上安装设置很方便的工具和服务，然而在 Windows 上却享受不到这种便捷。

亲测只要设置1024以上的端口，Bash 中起的服务，在 Windows 上可以通过本地端口访问。

{% asset_img windows-terminal.png Windows 自带Terminal默认展示效果 %}

{% asset_img cmder-terminal.png Cmder的默认展示效果 %}

这也就意味着，我们可以通过BUW使用一整套 Linux 的服务和辅助工具，帮助我们在 Windows 上面进行开发。

#### 升级体验

Windows 自带的 Terminal 默认状态下在展示上面有各种Bug（字符），另外功能也很弱（没有多Tab支持之类的），所以对于一个命令行重度依赖者来说在用 Bash 之前找一个顺手的 Terminal 是非常重要的。

根据个人习惯吧，我选择了 Cmder 作为我的 Terminal。

{% asset_img windows-terminal.png Windows 自带Terminal默认展示效果 %}

{% asset_img cmder-terminal.png Cmder的默认展示效果 %}


### 总结

总的一句，BUW 基本上满足了我对 Linux 终端的需求，基本上可以摆脱双系统的宿命了，希望 Windows 在以后的版本迭代更新 BUW 的体验给原生自带的 Terminal 免得还要装一个 Cmder 做适配。