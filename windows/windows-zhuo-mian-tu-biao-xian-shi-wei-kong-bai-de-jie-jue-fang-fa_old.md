---
description: 发布于 2020.09.26
---

# Windows 桌面图标显示为空白的解决方法

#### 问题展示

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-052803.png)

#### 解决思路及方法概括

查看隐藏文件 - 删除图标缓存 - 重启 `explorer`

#### 解决方案

① 打开 `此电脑` ，在 `菜单栏` - `查看` - `隐藏的项目` 打勾

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-052804.png)

② 进入 `C:\Users(用户)\你的用户名\AppData\Local` ，删除 `IconCache.db`

③ 打开 `任务管理器`（Ctrl + Shift + Esc），结束 `explorer.exe` 进程

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-052805.png)

④ 在 `任务管理器` 中的 `文件` 里选择 `运行新任务` ，输入 `explorer` 并确定

![](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-52806.png)

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-052802.png)

**注意**：是 `explorer` 而不是 `explore`

解决后图标恢复正常

![在这里插入图片描述](http://nme-200t.oss-cn-hangzhou.aliyuncs.com/notes/2022-10-05-52803.png)

最后可以重新取消查看`隐藏的项目`
