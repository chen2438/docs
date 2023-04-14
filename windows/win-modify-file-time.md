---
description: 转载于 2023-04-13
categories:
- windows
date: 2023-04-13
slug: win-modify-file-time
title: Windows 批量修改文件时间
---

# Windows 批量修改文件时间

原文链接：https://blog.csdn.net/huangfujin321/article/details/107951410

在 `PowerShell` 里面执行

更改为当前时间：

```shell
Get-Childitem -path ‘D:\Tomcat7\webapps’ -Recurse | foreach-object { $_.LastWriteTime = Get-Date ; $_.CreationTime = Get-Date }
```

更改为指定时间：

```shell
Get-Childitem -path ‘D:\Tomcat7\webapps’ -Recurse | foreach-object { $_.LastWriteTime = Get-Date ; $_.CreationTime = ‘08/12/2020 10:18:36’ }
```

