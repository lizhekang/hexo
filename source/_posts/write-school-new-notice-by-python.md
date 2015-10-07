title: 编写python校内通知提醒
date: 2015-5-23 13:32:04
categories:
- DIY
tags:
- python
- 源代码
---

## 编写python校内通知提醒

### MLGB

因为暑假要去实习，实习时间又会影响实习阶段的考核，所以想早点去报道，sb学院又迟迟不发考试安排，教务处又不能给明确答复（╯‵□′）╯︵┴─┴，真的烦死人了，让我都不知道该买几号的机票。难道让我天天守着校内通知的网站等通知，由于实在是太赖，怕自己忘记看了，所以就花了一个下午研究了一下mongodb和python的stmp模块，写了一个获取校内考试通告，然后自动发邮件通知我的小脚本。

### 思路

其实， 写这个脚本其实就做了一个小型的爬虫，最麻烦的一步是提取自己想要的有用信息这一步。在这里我用到了BeautifulSoup4这个库来解析网页的DOM元素，主要是获取校内通知的标题，寻找相关的目标字符串，如：2012级、考试等等。

然后，为了实现保存已发送的信息记录，这里用到了一个比较轻量级的数据库mongodb，相比mysql是轻太多了，选用mongodb主要出于脚本需要运行在树莓派上面，树莓派本来性能就一般般。


### 效果

{% asset_img QQ截图20150523224103-1024x558.png Pic for 效果图1 %}

</br>

{% asset_img 9214333552491734895-614x1024.jpg Pic for 效果图2 %}

### 源代码

```
#coding=utf-8
from urllib.request import urlopen
from urllib.parse import quote
from bs4 import BeautifulSoup
import smtplib
from email.mime.text import MIMEText
from email.header import Header
import pymongo
# 使用线程优化效率
import time
import threading
import sys
import re
 
# 邮件发送参数
sender = 'sender email addr'
receiver = 'receiver email addr'
subject = '考试日程提醒'
smtpserver = 'smtp.163.com'
username = 'your username'
password = 'your password' 
 
# 查找目标
url_base = 'http://ccst.jlu.edu.cn/'
target_text = '2012级'
sleep_time_normal = 15*60   # 设置15分钟检查一次
sleep_time_high = 5         # 设置5秒钟一次
global sleep_time
 
# 判断一个字符是否是汉字
def is_chinese(uchar):
    reChinese = re.compile('[\u4e00-\u9fa5]')
    if reChinese.search(uchar) != None:
        return True
    else:
        return False
 
# 处理url中的中文字符问题
def turn_chinese_to_base64(string):
    result = '';
    for i in range(len(string)):
        if(is_chinese(string[i])):
            # 把字符转为gbk
            result += quote(string[i].encode('gbk'))
        else:
            result += string[i]
    return result
 
def getHtml(url):
    url = turn_chinese_to_base64(url)
    url = url_base + url
    try:
        page = urlopen(url, timeout = 10)
    except:
        html = None
    else:
        html = page.read()
        html = html.decode('gbk')
    return html
 
def sendEmail(smtp, title, text, notes):
    global sleep_time
    msg = MIMEText(text,'html','utf-8')
    msg['Subject'] = Header(subject + ' - ' + title, 'utf-8')
    try:
        smtp.sendmail(sender, receiver, msg.as_string())
        notes.update({'title':title}, {'title':title, 'sended': 1})
        print("%s had been sended" %(title))
    except:
        print("[ERROR] error in sending %s" %(title))
        sleep_time = sleep_time_high
 
def checkNotSendedEmail(smtp, notes):
    print("检查尚未发送成功的邮件")
    unSended = notes.find({'sended': 0})
    for item in unSended:
        # 为了保证发送的成功率，在这不新开线程
        sendEmail(smtp, item['title'], item['html'], notes)
 
def newTask():
 
    # 登录邮箱系统
    smtp = smtplib.SMTP()
    smtp.connect('smtp.163.com')
    smtp.login(username, password)
    # 初始化开始地址
    target = '?mod=info&act=search&kw=考试&x=0&y=1'
    # 同时开启4个线程
    thread_list = list()
 
    # 循环执行标记
    flag = True
 
    # 初始化数据库
    db = pymongo.MongoClient()
    notes = db.ccst_db.notes
 
    while(flag):
        # 初始化
        tid = None
        html = getHtml(target)
        flag = False
        # 检查是否获取到html元素
        if html == None:
            break
 
        soup = BeautifulSoup(html)
 
        # 查找目标结果集合
        target_items = soup.find(class_='list').ul.find_all('a')
        # 将结果转化为字符串
        result = ''
        for target_item in target_items:
            if target_text in target_item.text:
                if notes.find_one({"title": target_item.text}) == None:
 
                    # 设置标志位，因为网页有变动
                    flag = True
 
                    # 打开通知具体页面
                    target_url = target_item.attrs['href']
                    target_html = getHtml(target_url)
                    #print(target_html)
                    target_soup = BeautifulSoup(target_html)
                    result = str(target_soup.find(class_='list').div)
                    # 如果没有当前记录，保存到数据库
                    post = {"title": target_item.text,
                            "url": target_item.attrs['href'],
                            "html": result,
                            "sended": 0}
                    notes.insert(post)
 
                    # 等待上一次线程执行结束
                    if tid != None:
                        tid.join()
 
                    # 新建发送电子邮件的线程
                    tid = threading.Thread(target=sendEmail, args=(smtp, target_item.text, result,
 
notes))
                    tid.setDaemon(True)
                    tid.start()
 
        if flag:
            print("网页有新消息")
        else:
            print("网页无新消息")
 
        if soup.find(class_="pagestring").find('a',text="下一页") == None:
            break
        else:
            target = soup.find(class_="pagestring").find('a',text="下一页").attrs['href']
 
    # 确保最后线程执行完成
    if tid != None:
        tid.join()
    # 检查发送失败的邮件
    checkNotSendedEmail(smtp, notes)
    # 退出邮箱系统
    smtp.quit()
# 主线程
if __name__=='__main__':
    global sleep_time
    try:
        while True:
            # 重置系统挂起时间
            sleep_time = sleep_time_normal
            newTask()
            time.sleep(sleep_time)
    except KeyboardInterrupt:
        print('程序结束')
        sys.exit(0)
```

当然，最后为了保证这个脚本能够稳定运行，配置一下supervisor就好了。

我的项目在[Git@OSC](http://git.oschina.net/dalex/checkNotify)上。