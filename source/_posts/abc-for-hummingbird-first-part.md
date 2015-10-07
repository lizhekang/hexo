title: 蜂鸟A31开发板入门——上
date: 2014-10-07 7:30:01
categories: 
- 开发板
tags: 
- 开发板
- linux
---

## 蜂鸟A31开发板入门——上

### 硬件：

- 蜂鸟开发板（A31）
- 串口线、12V/3A电源适配器和mini usb线（不是micro usb）。买板的时候店家只会送一条串口线，电源适配器和mini usb线都需要自己买，挺坑爹的。

### 软件环境：

官方提供的文档说需要两台PC，一台Linux一台Windows，其实用一台就行，不是用双系统，在Linux下也有镜像烧录软件，这个以后再说，首先说说SDK的编译环境。

#### 配置编译服务器

a. 安装 Ubuntu12.04/12.10(64 bit)，不能使用 32bit 的。
b. 更新系统：apt-get update
c. 安装编译需要的包：

```
apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev libc6-dev lib32ncurses5-dev ia32-libs  x11proto-core-dev libx11-dev lib32z1-dev libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown libxml2-utils uboot-mkimage
```

d. 修改 gcc 版本为 4.4

```
apt-get install gcc-4.4
apt-get install g++-4.4
rm /usr/bin/gcc
ln -s /usr/bin/gcc-4.4 /usr/bin/gcc 运行 `gcc -v` 来确认结果
```

e. 安装JDK

```
apt-get -y install python-software-properties
add-apt-repository -y ppa:webupd8team/java
echo debconf shared/accepted-oracle-license-v1-1 select true | \
debconf-set-selections
echo debconf shared/accepted-oracle-license-v1-1 seen true | \
debconf-set-selections
apt-get update && apt-get -y install oracle-java6-installer 测试是否成功 java -version
```

至此，编译服务器已经搭建完成，可以用来编译android镜像了。