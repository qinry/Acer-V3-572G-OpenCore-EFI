# Acer V3 572G OpenCore EFI

### 一 概述

本机机型为**Acer V3 572G 5247**，macOS为**BigSur 11.3.2**，OpenCore**0.6.9-RELEASE**。仿冒机型**MacBookPro12,1**。

### 二 安装前须知

目前最重要的一些硬件能够正常使用，有核芯显卡、声卡、蓝牙、以太网卡、I2C触控板、PS2键盘、USB（USB的定制请用搜索引擎）。除了原装的无线网卡，即WiFi，无法支持外，只能使用有线网络，或者购买合适的无线网卡更换原本的网卡，也可使用USB无线网卡来解决。亮度能够使用快捷键调节。开机八苹果花屏暂时被解决，暂时未解决的问题但不影响正常使用：睡眠会重启。无法保证该EFI能让你一定装上黑苹果。

无法保证开机花屏也能在你的机子得到解决，如果你不幸发生了花屏，可以使用快捷键fn+F4假睡眠关掉屏幕之后，过了1秒后回车唤醒，或许解决你开机花屏成功进入系统，等进入系统后，建议去尝试了解如何注入EDID来解决花屏问题，注入的EDID是要修改过的，EDID表示为128字节大小的十六进制数据时，找到第127位和第128位的数，如果两位是64改成20，如果是20改成64。

### 三 硬件信息

|   名称   |                             描述                             |
| :------: | :----------------------------------------------------------: |
|   CPU    |        Intel(R) Core(TM)i5-5200U @2.2GHZ, BroadWell-U        |
|   GPU    | Intel HD5500,设备ID 0x16168086/NVIDA GeForce 840M(不支持,禁用) |
|  芯片组  |                         Intel 9系列                          |
|   声卡   |                        Realtek ALC283                        |
| 以太网卡 |                  Realtek RTL8111/8168/8411                   |
| 无线网卡 |           Broadcom BCM43228 802.11a/b/g/n(不支持)            |
|   主板   |                         宏碁 EA50_HB                         |
|   BIOS   |         BIOS v1.32, insydeh20 setup utiility rev 5.0         |

### 四 BIOS设置

Acer开机按`F2`进入BIOS。本人的BIOS隐藏高级选项的配置，再者，`CFG-LOCK`默认未解锁，故DVMT Pre-Allocated此选项无法设置为64MB，本机默认为128MB(高于96MB)，`config.plist`的`DeviceProperties-`>`PciRoot(0x0)/Pci(0x2,0x0)`，必须加DVMT-prealloc 32MB的补丁，否则造成安装和启动过程中内核发生崩溃,如下：

``` l
framebuffer-patch-enable|Data|01000000
framebuffer-stolenmem|Data|00003001
framebuffer-fbmem|Data|00009000
```

大多数的设置都被隐藏了，要设置的很少，但却很重要。

开启项：

* Main->Boot Manager
* Boot->UEFI

禁用项：

* Main->WakeOnLAN

* Boot->Sercure Boot

### 五 推荐工具

* U盘：安装器的载体，一个32GB以上的U盘，我用的是64GB，用来制作离线安装器。如果U盘吃紧，比如只有4GB可以制作在线安装器，见下一点

* 镜像：macOS镜像文件可以下载[黑果小兵的部落阁](https://blog.daliansky.net/)的镜像用来制作离线安装器，或者参照[OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos)的制作macOS在线的安装器。
* 镜像制作工具：etcher(用来制作离线安装器)或rufus(用来制作在线安装器，使用时需要连接网线)
* 分区工具：DiskGenius，EFI分区建议200MB到500MB，本人使用200MB。
* 配置编辑器：propertree或OCAuxiliaryTools(注意版本，要与OpenCore版本对应)，用来编辑配置文件config.plist。

### 六 项目目录介绍

* EFI：引导文件，包含BOOT和OC两个文件夹
* HIDPI:用于开启HIDPI的工具，使用里面脚本hidpi.sh，详细使用步骤见搜索，不注入EDID开启HIDPI，手动分辨率输入，1920x1080、3840x2160、1600x900、3200x1800。
* EFI/OC下
  * ACPI：存放aml文件，用于提供操作系统管理电源
  * Drivers:存放efi文件用来加载驱动固件，比如HfsPlus.efi、OpenRuntime.efi等
  * Kexts:存放内核扩展文件，比如Lilu.kext、WhateverGreen.kext、VirtualSMC.kext等等
  * Tools:存放辅助工具，比如OpenShell.efi
  * Resources:存放与GUI相关的资源
  * config.plist:OpenCore的配置文件
  * OpenCore.efi：引导加载程序

#### 七 参考

* HangzhouLoser.[新手挑战黑苹果第二弹-超详细的OpenCore黑苹果笔记本安装教程](https://www.bilibili.com/video/BV1kK4y1S74C/?spm_id_from=333.788.b_636f6d6d656e74.6).bilibili.

* dortania.[OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/).
* 黑果小兵的部落阁.[macOS BigSur 11.0安装中常见的问题及解决方法](https://blog.daliansky.net/Common-problems-and-solutions-in-macOS-BigSur-11.0-installation.html).
* 黑苹果屋.[OpenCor_Clover通用Intel英特尔核显驱动完整教程及英特尔集成显卡与核显仿冒ID速查表显卡驱动](http://imacos.top/2020/09/03/2216/).

