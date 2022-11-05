---
layout: posts
title: 树莓派zero2w教程
date: 2022-01-15 11:23:00
updated: 2022-08-28 11:23:53
tags: 
categories: 
 - 笔记
---
# 前言

最近买了一个树莓派zero2w，回来后一搜百度和谷歌，关于zero2w的教程少之又少。但好在我参考了zerow的教程，最后还是成了。

# 准备

1. 树莓派zero2w主板一块
2. 读卡器
3. SD卡（16GB以上）
4. 一根数据线
5. 一台电脑，win10/win11都行
6. [树莓派官方烧写工具](https://downloads.raspberrypi.org/imager/imager_latest.exe)（这个比较方便）
7. [OTG驱动](https://pan.baidu.com/s/1xHAYLeb-tZIcSced471-yw) 提取码：7vtr
8. ssh连接工具（putty、xshell）
9. linux命令行知识

# 开工

##  烧录镜像

把SD卡用读卡器插到电脑，打开烧写工具。

选择镜像：CHOOSE OS  –>  Raspberry Pi OS (other)  –>  Raspberry Pi OS Lite(32-bit)

选择储存设备：CHOOSE STORAGE –> （需要烧录的SD卡）

**完了后不要急着上电！！！**

**完了后不要急着上电！！！**

**完了后不要急着上电！！！**

##  初始设置

其实就是使用USB Gadget驱动将USB-OTG模拟为有线网卡，之前需要的设置比较繁琐，好在新版的Raspbian内核不需要额外安装补丁，可以直接启用，另外虚拟出来的和有线网卡基本一样，不像串口那样只能打开一个终端。方法如下：

```
# 修改boot分区里的config.txt文件，找到“[cm4]”，在其上方加入以下内容
dtoverlay=dwc2
# 修改boot分区里的cmdline.txt文件，在rootwait后面增加如下内容，注意每个参数之间空格分开，且都是在同一行
modules-load=dwc2,g_ether
```

在boot分区根目录创建一个文本文件，然后重命名为ssh，**注意去掉.txt后缀**，此时即可以 开启ssh登录（新版Raspbian的改动）。

##  上电

usb数据线一头连接电脑，一头连接电脑。如果显示一个未知设备说明连接成功了。

右键桌面上的“此电脑”，选择“管理” –> “设备管理器” —>右键新的未知设备 –>  “更新驱动程序” –> “浏览我的电脑以查找驱动程序”—> 选择解压出来的驱动

##  连接系统

大约一分钟后可以用ssh连接工具来连接树莓派zero2w。服务器(host)填`raspberrypi.local` ，端口`22`，用户名`pi`，密码`raspberry`

> 注意：进入系统后必须先修改密码，否则过段时间会无法登录。输入 `sudo passwd pi`,接下来输入密码，第二次确认。

##  连接wifi

之前我按照网上一大堆类似修改文件的方法，最后行不通。后来才发现其实树莓派自带了一个管理工具。输入`sudo raspi-config`,选择国家—> “System Options”—>选第一个 —> 输入WiFi名称密码—> 重启

###  配置静态IP

输入`sudo nano /etc/dhcpcd.conf`，在最后一行加上

```
interface wlan0
static ip_address=192.168.1.28/24 # 192.168.1.28改为你要设置的静态IP
static routers=192.168.1.1 # 192.168.1.1改为你家的网关地址
static domain_name_servers=192.168.1.1 223.5.5.5 # 192.168.1.1也改为你的网关地址
```

重启

好了，就写这么多，原文地址为：https://blog.liaouser.top/2022/01/25/64feb591.html
# 参考

> 一根数据线玩转树莓派zerow：https://www.cnblogs.com/sjqlwy/p/zero_otg.html
>
> Raspberry Pi Docum

文章作者: liaouser
文章链接: https://blog.liaouser.top/2022/01/25/64feb591.html
版权声明: 本博客所有文章除特别声明外，均采用 CC BY-NC-SA 4.0 许可协议。转载请注明来自 liaouser&博客！