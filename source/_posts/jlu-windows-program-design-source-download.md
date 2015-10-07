title: 吉林大学Windows程序设计代码下载
date: 2014-11-11 11:05:16
categories: 
- 学习资料
tags:
- 源代码
- 吉林大学
---

## 吉林大学Windows程序设计代码下载

### 题目

#### 编写一个滚动显示文本的程序。

提示：参照sysmets1.c和sysmets3.c程序，增加处理WM_TIMER消息。

#### 编写一个滚动显示正弦曲线的程序。

提示：参照SINEWAVE.C程序，增加处理WM_TIMER消息。

先求出一个周期的所有的点，用Polyline()画出由点连成的线。

每次处理WM_TIMER消息时，擦除原有的线，画出移动后的线。

#### 编写一个扫雷程序。

窗口风格：

```
WS_OVERLAPPEDWINDOW = 
WS_OVERLAPPED|WS_CAPTION |WS_SYSMENU|WS_THICKFRAME
|WS_MINIMIZEBOX |WS_MAXIMIZEBOX;

WS_OVERLAPPED   Creates an overlapped window. An overlapped window usually has a caption and a border.
WS_THICKFRAME   Creates a window with a thick frame that can be used to size the window.
窗口大小：可在CreateWindow()函数中指定，窗口各元素的尺寸：
窗口边框宽度：GetSystemMetrics(SM_CXBORDER);
窗口边框高度：GetSystemMetrics(SM_CYBORDER);
窗口标题条高度：GetSystemMetrics(SM_CYCAPTION);
窗口菜单条高度：GetSystemMetrics(SM_CYMENU);
```

用画线函数使用不同的笔和刷子即可完成。

#### 编制一个无模态标点符号输入对话框。

首先用“ \附件\系统工具\字符映射表”程序查出每个特殊字符的编码，如“。”的编码是0xA1A4。

在对话框的客户区的网格内输出字符

```
char bData[2];
int wCode;
wCode=0xA1A4;
bData[0]=HIBYTE(wCode);
bData[1]=LOBYTE(wCode);
TextOut(hDC, x, y, bData, 2);
```

把鼠标点击的字符发向目标窗口

```
wCode= 0xA1A4;
PostMessage(hwnd,WM_CHAR, (WPARAM)HIBYTE(wCode), 1L);
PostMessage(hwnd,WM_CHAR,(WPARAM)LOBYTE(wCode), 1L);
```

#### 编制一个功能如在目录“winline”下的winline程序

可以参考我的实现的版本。

### 源代码下载

本地下载地址：{% asset_link homework.zip homework.zip %}