title: 简单粗暴的命令行助手
date: 2015-04-26 13:22:08
tags:
---

## 简单粗暴的命令行助手

最近在github上面又发现了一个有趣的项目，项目名叫[thefuck](https://github.com/nvbn/thefuck)。

### 她很有趣

{% asset_img Screenshot-from-2015-04-26-11-50-17.png Pic for thefuck 1 %}

当你不小心打错了一个命令，例如把ifconfig写成ifcnofig，终端会无法执行命令，然后，特别简单粗暴的方法校正之前打错的命令的方法就是输入一个fuck命令，哈哈，结果就出来了。

当然，带上参数也是可以的。

{% asset_img Screenshot-from-2015-04-26-11-55-37.png Pic for thefuck 2 %}

### 原理

其实这个软件的原理也不复杂，从配置文件可以看出来fuck这个程序拿到了历史记录里最后一条命令，就是上一次打错的命令，在根据一些逻辑判断校正命令。

大家如果有兴趣的话可以上github上面留意这个项目，[nvbn/thefuck](https://github.com/nvbn/thefuck)。