---
description: 发布于 2022.02.18
---

# 关于【Windows 资源保护找到了损坏文件，但其中有一些文件无法修复】的解决方法

[**参考**](https://www.cnblogs.com/karmapeng/p/10241731.html)

**问题描述**

```
PS C:\Users\Qwer-laptop> sfc /scannow

开始系统扫描。此过程将需要一些时间。

开始系统扫描的验证阶段。
验证 100% 已完成。

Windows 资源保护找到了损坏文件，但其中有一些文件无法修复。
对于联机修复，位于 windir\Logs\CBS\CBS.log 的 CBS 日志文件中
有详细信息。例如 C:\Windows\Logs\CBS\CBS.log。对于脱机修复，
/OFFLOGFILE 标记提供的日志文件中有详细信息。
```

**解决方法** 依次输入`DISM.exe /Online /Cleanup-image /Scanhealth`和`DISM.exe /Online /Cleanup-image /Restorehealth`，然后再`sfc /scannow`即可。在我的环境下，似乎可以不重启电脑。

```
PS C:\Users\Qwer-laptop> DISM.exe /Online /Cleanup-image /Scanhealth

部署映像服务和管理工具
版本: 10.0.22557.1

映像版本: 10.0.22557.1

[==========================100.0%==========================] 可以修复组件存储。
操作成功完成。
```

```
PS C:\Users\Qwer-laptop> DISM.exe /Online /Cleanup-image /Restorehealth

部署映像服务和管理工具
版本: 10.0.22557.1

映像版本: 10.0.22557.1

[==========================100.0%==========================] 还原操作已成功完成。
操作成功完成。
```

```
PS C:\Users\Qwer-laptop> sfc /scannow

开始系统扫描。此过程将需要一些时间。

开始系统扫描的验证阶段。
验证 100% 已完成。

Windows 资源保护找到了损坏文件并成功修复了它们。
对于联机修复，位于 windir\Logs\CBS\CBS.log 的 CBS 日志文件中
有详细信息。例如 C:\Windows\Logs\CBS\CBS.log。对于脱机修复，
/OFFLOGFILE 标记提供的日志文件中有详细信息。
```
