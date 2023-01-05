---
description: 转载于 2022.10.26
---

# ubuntu 查看实时网速

[参考链接](https://huataihuang.gitbooks.io/cloud-atlas/content/network/packet\_analysis/utilities/tcptrack.html)

### TCPtrack

#### 安装`tcptrack`

```bash
sudo apt install tcptrack
```

#### 使用`tcptrack`

立即开始使用

```bash
sudo tcptrack -i eth0
```

`tcptrack`使用非常简单，只要具有`root`权限执行，带上`-i <interface>`参数就可以在指定接口上开始抓包分析，实际使用语法

```bash
Usage: tcptrack [-dfhvp] [-r <seconds>] -i <interface> [<filter expression>] [-T <pcap file]
```

* `-r`

默认情况下，如果网络连接关闭2秒以后，`tcptrack`就会移除显示，不过可以通过`-r`参数来修改，例如调整成5秒以后再移除

```bash
tcptrack -i eth0 -r 5
```

* `-d`

默认情况下，`tcptrack`会尝试跟踪启动时已经存在的连接。要避免跟踪启动时已经存在的连接，可以使用`-d`参数

```bash
tcptrack -i eth0 -r 5 -d
```

* `-p`

默认情况下，`tcptrack`会将接口设置成混杂模式（promiscuous），要避免混杂模式，则使用`-p`

* 交互命令 - 启用`tcptrack`之后，会进入交互模式，此时可以按以下键切换模式
  * `p` - 暂停/恢复 显示
  * `s` - 切换使用3种排序模式：非排序（默认），按照速率排序（sorted by rate），按照字节排序（sorted by bytes）。其中按照速率排序比较实用。
  * `q` - 退出程序

![TCPTrack sort by rate](https://nme-file.oss-cn-hangzhou.aliyuncs.com/img/202210262011616.png)

* `-f` - 表示`fast average speed`算法。TCPTrack将通过使用运行平均来计算连接的平均速度。TCPTrack将消耗更多的内存和CPUshijian，但是平均值可以更接近实时并且比每秒一次的更新更及时，并且在沉重的网络负载下更为精确。每秒采样次数将在快速平均模式下重新计算，默认是每秒采样10次进行平均。

> 实践发现在大流量的服务器上，需要使用`-f`参数，观察似乎更接近实际流量。

* `-T` - 从文件读取，此时将尽可能快速展示已经通过tcpdump抓包的文件中的性能分析，例如：

```bash
tcptrack -T network.pcap port  > network.txt
cat network.txt
```

* 常用的过滤规则方法

```bash
tcptrack -i eth0 'ip dst 192.168.25.34 and port (80 or 443 or 21)'
tcptrack -i eth0 'port (443 or 80)'
tcptrack -i eth0 'dst port 80'
tcptrack -i eth0 'dst port 22'
tcptrack -i eth0 'src or dst 87.xx.xx.18'
```
