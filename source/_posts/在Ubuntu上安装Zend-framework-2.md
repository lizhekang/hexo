title: 在Ubuntu上安装Zend framework 2
date: 2014-10-25 10:07:49
categories: 
- 工具
- 软件
tags: 
- ubuntu
- linux
- php
- framework
---

## 在Ubuntu上安装Zend framework 2

### 前提

假设已经在Ubuntu服务器上搭建好Apache+php环境

### 步骤

a. 下载并解压ZF2快速环境搭建工具（ZendSkeletonApplication）

```
cd 项目目录 wget https://github.com/zendframework/ZendSkeletonApplication/archive/master.zip unzip ZendSkeletonApplication-master.zip
```

b. 完成配置

```
cd ZendSkeletonApplication php composer.phar self-update php composer.phar install
```

c. 修改Apache配置文件

```
cd /etc/apache2/sites-available sudo vim 000-default.conf
```

在文件末尾添加以下信息

```
<VirtualHost *:80>
ServerName ServerName.com                                            ;服务器ip或域名
DocumentRoot /path/to/ZendSkeletonApplication/public     ;public目录的绝对路径
SetEnv APPLICATION_ENV “development”
<Directory /path/to/ZendSkeletonApplication/public>           ;同上
DirectoryIndex index.php
AllowOverride All
Order allow,deny
Allow from all
</Directory>
</VirtualHost>
```

d. 重启Apache服务

```
sudo service apache2 restart
```

e. 访问你设定的ServerName就能看到ZendFramework的初始页面了。