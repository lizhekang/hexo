title: 在极路由上使用drcom
date: 2015-03-24 13:12:17
categories:
- DIY
tags:
- drcom
- hiwifi
- 路由器
- 源代码
---

## 在极路由上使用drcom

相信不用我说大家都知道在路由器上面装drcom目的都是为了什么，所以绝对干货。

最近刚刚入手了一台极2（现在感觉自己真的很二=_=），原以为生态圈会有很多应用，结果市场上的插件屈指可数，还好底层就是openwrt，而且官方提供方法ssh到路由器，不然连drcom都装不上，还怎么在学校用呢？

### drcom逆向项目

首先提供一个大神逆向出来的吉大drcom客户端C语言代码，项目在GitHub上[https://github.com/ly0/jlu-drcom-client](jlu-drcom-client)。然后参考HiWifi的官方文档，配置好交叉编译环境，然后通过ssh把客户端推到路由器上面。其实理论上到这里就应该结束了。但是在实际使用的时候发现C语言版本的客户端有Bug——下线之后不会重连，搞到我每次都要上去kill进程，蛋疼死了。。。不要问我为什么不用Python版本，因为路由器flash不够大，装不上。

### shell脚本

后来我就写了下面这个shell脚本，配合crontab使用，终于不掉线了。

```
#!/bin/bash
#
#检测网络链接
network(){
    #超时时间
    timeout=1
    #目标网站
    target=www.baidu.com
    #获取响应状态码
    ret_code=`curl -I -s --connect-timeout $timeout $target -w %{http_code} | tail -n1`
        #echo $ret_code
    if [ "x$ret_code" = "x200" ]; then
        echo "network ok"
        return 1
        #网络畅通
    else
        echo "network not ok"
        return 0
        #网络不畅通
    fi
}
network
result=$?
 
if [ "$result" = "1" ]; then
    echo "finish"
else
    echo "kill drcom"
    `pgrep drcom | xargs kill -9`
    echo "start drcom"
    result=`./drcom`
    if [ "$result" = "[drcom-challenge]: challenge success!\n[drcom-login]: login success!\n\n[drcom-keep-alive]: drcom running in daemon!" ]; then
        echo "success"
    else
        echo "error"
    fi
    echo "finish"
fi
```