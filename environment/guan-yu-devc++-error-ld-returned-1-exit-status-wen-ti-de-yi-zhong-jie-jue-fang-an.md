---
description: 发布于 2021.10.04
---

# 关于 Dev\_C++ 【\[Error] ld returned 1 exit status】问题的一种解决方案

**首先请检查程序的exe文件有没有被关闭，若没有，关闭后即可正常编译。若仍然报错，请往下看。**

**本方法可能不能解决所有的此现象问题。**

**问题症状：**

1、编译cpp时出现`[Error] ld returned 1 exit status`

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-50757.png)

2、**之前**在运行这个cpp的exe文件时，命令行黑框光标闪烁，几秒后程序自动停止运行，返回值不为`0`。

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-050753.png)

**解决方法：**

1、这是代码有问题，请检查代码以彻底解决这个问题。**着重检查数组越界的问题。**

2、右键 Windows “开始”，进入`Windows终端（管理员）`

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-050756.png)

3、输入`taskkill -f -im xxx.exe(你的cpp所生成的exe名字)`，回车。

在可能的连续几次“拒绝访问”后，该进程会被终止。

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-050755.png)

4、随后可以正常编译。**但若代码没有修正，还是会复现此问题。**

**原因推测：**

程序生成的 exe 可执行文件 崩溃/被系统关闭，显示为已经关闭，但实际还在运行，需要手动关闭。此时如果删除exe文件，会提示被它自己占用。
