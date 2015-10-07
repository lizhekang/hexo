title: 利用shadowsocks科学上网
date: 2014-09-30 12:18:25
categories: 
- 软件
- 工具
tags:
- shadowsocks
- 科学上网
- 工具
---

## 利用shadowsocks科学上网

### 什么是科学上网

自古以来天朝就有长城，就像天朝向世界发出的第一封所说的”Across the Great Wall we can reach every corner in the world.(越过长城，走向世界)”，这就是科学上网的意思。

### 科学上网的原理

其实科学上网并不难，原理就是有一个在墙内可以访问的节点A，节点A可以访问墙内不能访问的网站，然后我们通过节点A转发墙内不能访问的网站的请求来达到科学上网的目的。

### 科学上网的方法

科学上网的关键点就在于如何找到节点A，要获得节点A有很多方法，网上有各种免费或者收费的VPN，自己有墙外可以访问的服务器也是可以的，但是我比较不建议用免费的VPN，因为用免费VPN对你个人信息安全没有保障，小的可能泄露你访问网站的内容，大的可能会导致你账号密码可能被窃取，所以如果自己有服务器是最好的情况。我就有一台在新加坡的VPS，我在主机上面搭了Shadowsocks作为我科学上网的A节点。

### Shadowsocks是什么

Shadowsocks实质上也是一种socks5代理服务，有点类似于ssh代理，有人误以为socks5代理只能做浏览器代理，不能做全局代理，这是不对的，socks5也能VPN一样做全局代理。

### 在服务器上部署Shadowsocks

#### 在Debian / Ubuntu下安装命令

```
apt-get install python-pip python-m2crypto pip install shadowsocks
```

#### 在CentOS下安装命令

```
yum install m2crypto python-setuptools easy_install pip pip install shadowsocks
```

#### 按自己想法修改一下配置文件

```
sudo vim /etc/shadowsocks.json 
```

{% codeblock lang:json %}
{
    “server”: ”my_server_ip”,
    “server_port”: 8388,
    “local_address”: “127.0.0.1”,
    “local_port”: 1080,
    “password”: ”mypassword”,
    “timeout”: 300,
    “method”: ”aes-256-cfb”,
    “fast_open”: false,
    “workers”: 1
}
{% endcodeblock %}

#### 运行shadowsocks服务端

```
sudo ssserver -c /etc/shadowsocks.json
```

#### 配置客户端

服务器配置结束后，在本机运行Shadowsocks GUI，简单配置一下，服务器ip（vps的ip地址），服务器上Shdoowsocks端口号。设置后点击Save，大功告成。

{% asset_img 100114_2357_Shadowsocks1.png Pic for Shadowsocks GUI %}

### 下载GUI
本地下载地址：{% asset_link shadowsocks.zip Shadowsocks GUI %}