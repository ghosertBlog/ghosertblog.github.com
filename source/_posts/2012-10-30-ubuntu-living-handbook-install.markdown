---
layout: post
title: "Ubuntu桌面生存指南 (3) --- 构建Ubuntu系统基础设施"
date: 2012-10-30 13:03
comments: true
categories: [Ubuntu， Linux， Tools]
---


Ubuntu系统的基础设施
--------------------------
这一节中，我们将讨论从头开始搭建一个基础设施完善的 Ubuntu 系统。主要包括基于 Ubuntu 系统的安装，分区方案，Wubi分区移植到物理分区，显卡驱动安装，翻墙相关的一些技巧和同步软件（Dropbox）等基础性软件的安装方法。另外也会讨论关于系统升级的一些策略。在夯实了这些基础之后，我们也为未来的系统备份，恢复做好了准备，提高了系统的图形处理性能，解决了在不同机器，不同系统间的文件同步问题，同时突破墙的封锁，进一步方便我们日常的工作学习。

<!--more-->

从U盘安装
--------------------------

上节我们提到过， Ubuntu 的发行安装方式众多，我们仍然推荐从硬盘开始安装，比起光盘，相信很多同学也了解U盘的优势：易于携带，保存，复制。这里不再敷陈，我们就从制作启动U盘说起。

**0. 下载 Ubuntu ISO 文件**

访问 Ubuntu 的 [官方下载][1] 页面，选择相应版本。一般而言它的版本有桌面版，服务器版，32位，64位，LTS，非LTS之分。所谓 LTS（Long Term Support）指的是长时间支持版本，并不是每一个新版本的 Ubuntu 都是 LTS 版本，譬如，最新的 12.10 版本就不是 LTS 版本，12.04 就是 LTS 版本，12.04 之前的 LTS 版本要追溯到2010年4月发布的 10.04，同时主版本号代表发布的年份，次版本号代表发布的月份，例如：12.04表示2012年4月发布。一般来说，推荐下载最近的 LTS 版本会得到更好的官方支援。这里我们推荐安装 12.04 的64位桌面版（命名方式：ubuntu-12.04.1-desktop-amd64.iso），官方支持达到了5年之久，基本上已经超过了用户当前硬件的使用寿命期限，也就是说你在换下一台PC之前无需更换操作系统。如果官方站点的下载速度较慢，你也可以搜索国内的一些镜像网站加速下载过程。譬如 [网易镜像][2]

**1. 从 Windows 制作启动U盘**

在 Windows 下访问 [Universal USB Installer][3] 的主页，这个Ubuntu官方推荐的绿色小工具就是帮助用户在手头没有 Ubuntu 的情况下，使用 Windows 来制作启动U盘。这个页面不仅包括了工具的下载链接，同时也包含了详细的操作步骤，同学们准备好1G容量以上的U盘和刚才下载到的ISO文件，按部就班操作即可。制作U盘的时候注意相应的选项，按我个人的经验它制作出的启动U盘质量相当高，甚至超越了Ubuntu下自带的工具。

![Universal-USB-Installer]

**2. 从 Ubuntu 制作启动U盘**

如果你手头有一台安装完毕的 Ubuntu 系统，你也可以通过启动 Startup Disk Creator 这个系统自带的工具制作启动U盘。注意如果你是跨版本的制作相应的启动盘可能会存在问题，笔者曾经在Ubuntu 10.04下使用这个工具制作基于12.04 ISO的启动盘，结果无法启动电脑，花了一个晚上才搞清楚原来是跨版本引发的问题，最后切换到 Windows 下的 [Universal USB Installer][3] 才解决问题。

![startup-disk-creator]



[1]: http://www.ubuntu.com/download
[2]: http://mirrors.163.com/ubuntu-releases/precise/
[3]: http://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/
[Universal-USB-Installer]: /images/ubuntu_living_handbook/Universal-USB-Installer.png
[startup-disk-creator]: /images/ubuntu_living_handbook/startup-disk-creator.png
