---
description: 转载于 2023-04-22
categories:
- debian
date: 2023-04-22
slug: d-jdk1.8
title: Debian 安装 JDK 1.8
---

# Debian 安装 JDK 1.8

参考 https://www.cnblogs.com/xuweiqibky/p/15695408.html

**第一步、下载安装包**

下载Linux环境下的jdk8，请去（[Java Downloads | Oracle](https://www.oracle.com/java/technologies/downloads/#java8)）中下载jdk的安装文件；

x86_64 架构 CPU 下载 **x64 Compressed Archive**

Arm 架构 CPU 下载  **ARM64 Compressed Archive**

**第二步、解压安装包**

将我们在windows上下载好的JDK安装包用wincp上传到debian，进行解压

新建/etc/java文件夹,解压至当前目录

解压命令进行解压

```
cd /etc/java
tar -zxvf jdk-8u311-linux-x64.tar.gz
```

**第三步、修改环境变量**

至此，我们最后需要修改环境变量，通过命令

```
vi /etc/profile
```

在文件末尾添加

```bash
export JAVA_HOME=/etc/java/jdk1.8.0_311/
export JRE_HOME=/etc/java/jdk1.8.0_311/jre
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JRE_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
```

然后，保存并退出

之后，通过命令 `source /etc/profile` 或重启电脑profile文件配置立即生效

**第四步、测试是否安装成功**

使用java -version，出现版本为java version "1.8.0_311"
