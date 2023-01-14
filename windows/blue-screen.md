---
description: 发布于 2020-02-25
categories:
- windows
date: 2020-02-25
slug: blue-screen
title: Win10 蓝屏终止代码 WHEA UNCORRECTABLE ERROR 的解决方法
---

# Win10蓝屏终止代码：WHEA\_UNCORRECTABLE\_ERROR的解决方法

最近蓝屏频率又上升了

查询资料 发现可能是**Intel C-State**导致的问题

需要**进主板BIOS关闭Intel C-State功能**

另外有人关闭**Cpu Power Management**也可能有效，据说**Intel C-State**是它的组成部分

我的主板型号：微星 MPG Z390 GAMING PRO CARBON

以下来自：[http://blog.sina.com.cn/s/blog\_906d892d0102vn26.html](http://blog.sina.com.cn/s/blog\_906d892d0102vn26.html)

Intel C-State Tech是intel的节约能耗，基于Intel组件基础上的一项深度节能技术。这一技术在BIOS中可以设置使用或是不使用。可以通过BIOS的升级选择是否在BIOS中是否有这一选项。

此技术有独立的控制标准，具体的控制由BIOS来定。操作系统运行到耗能高，或CPU的使用频率高等时候此项功能可对能耗和CPU核心进行适当的调节，以达到节约能耗的目的。

网络上有一部分人们认为，BIOS中把这一项打开之后电脑列容易出现死机的情况。这得根据装机的配件来定，所以这可能的一定的理由。因为，迅速的改变耗能对于各种配件的要求程度不一样。当某一配件不能很快达到Intel C-State Tech所要求的状态时就有可能出现死机的情况。

因为这一技术是基于Intel组件之上，所以可能于其它的配件产生冲突。从而可能用着用着就会死机,要么突然蓝屏,要么突然黑屏,要么就是画面定格死在那里,键鼠没响应,但是关闭后则一切正常。

其实Intel C-State Tech是一项深度节能的技术,Intel E0Stepping的CPU在开启C-State后,除了CPU自行省电以外,北桥与内存本身都会被CPU要求进入省电模式。不过该功能对内存的要求也非常高,此时如果内存本身的体质较差,就会出现死机的故障,如果遇到开启C-State死机的故障,只有在BIOS中将该选项设置为关闭状态,才能保证系统的稳定性。当然,如果是内存质量较好,开启该功能也不会对系统稳定性产生影响。
