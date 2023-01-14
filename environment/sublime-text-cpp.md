---
description: 发布于 2022-01-21, 修订于 2022-01-22
categories:
- environment
date: 2022-01-21
slug: sublime-text-cpp
title: Sublime Text 搭建 C++ 环境
---

# Sublime Text 搭建 C++ 环境

### 一、下载`MinGW`文件

1、下载`mingw-get-setup`：

网址：`https://sourceforge.net/projects/mingw/`

由于这是境外网站，请自行解决连接问题（下载的文件仅84.5KB）

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caef3cbc2.png)

2、双击运行，一直`continue`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caf0ee7e3.png)

3、安装完成后显示`MinGW Installation Manager`的页面

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caf25fc9a.png)

4、单击`mingw32-base`和`mingw-gcc-g++`左边的方框，选择`Mark for installation`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caf33e566.png)

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caf413447.png)

5、单击菜单栏左上角`Installation`，选择`Apply Changes`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caf4c9d56.png)

6、在弹出来的界面单击`Apply`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caf5e9550.png)

7、等待下载完成（图示为正在下载）

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caf6e294b.png)

8、下载完成，`Close`退出

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6caf85a924.png)

### 二、添加环境变量（以Win11为例）

1、右键`此电脑`，选择`属性`。

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cafa1a01a.png)

2、在相关链接里选择`高级系统设置`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cafb9f9ca.png)

3、在弹出的窗口单击`环境变量`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cafd701c6.png)

4、单击选中`系统变量`里的`Path`，然后单击`编辑`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cafeb921e.png)

5、在弹出的窗口单击`新建`，并输入你安装`MingGW`的地址，后面加一个`/bin`（二进制文件）

我是`C:\MinGW\bin`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb0015142.png)

5.5 连续点击`确定`以保存刚才的操作

6、右击`Win徽标键`，选择`Windows 终端（管理员）`，输入`gcc -v`

如果有以下样式反馈说明环境变量配置成功

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb015b212.png)

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb030d173.png)

### 三、配置Sublime Text

1、下载并安装`Sublime`

官网：`https://www.sublimetext.com/`

不演示安装过程，中途勾选`add to explorer context menu`，意思是右键菜单会出现“Open with Sublime Text”（用Sublime打开）的选项

2、找到你刚才设置的`Sublime Text`安装目录，打开`sublime_text.exe`（sublime本体）

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb0460dcc.png)

你会发现软件是英文的，如果需要汉化，可自行搜索

3、单击菜单栏的`Tools`，选择`Build System` - `New Build System`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb057014c.png)

4、用以下代码替换`untitled.sublime-build`文件中的所有内容

```javascript
{
	"cmd": ["g++", "-Wall", "${file}","-std=c++11", "-fexec-charset=gbk", "-o","${file_path}/${file_base_name}"],
	"file_regex": "^(..[^:]*):([0-9]+):?([0-9]+)?:?(.*)$",
	"working_dir": "${file_path}",
	"selector": "source.c, source.c++",
	"shell": true,
	"encoding":"cp936",
	"variants":
	[
		{
			"name": "Compile Only",
			"cmd": ["cmd","/C","g++", "-Wall", "${file}","-std=c++11", "-fexec-charset=gbk", "-o","${file_path}/${file_base_name}"],
		},
		{
			"name": "Run Only",
			"cmd": ["start","cmd","/c", "${file_base_name} & echo. & pause"],
		},
		{
			"name": "Compile & Run",
			"cmd": ["cmd","/C","g++", "-Wall", "${file}","-std=c++11", "-fexec-charset=gbk", "-o","${file_path}/${file_base_name}", "&&","start","cmd","/c", "${file_base_name} & echo. & pause"],
		}
	]
}
```

5、`Ctrl + S`保存，命名文件为`CPP.sublime-build`（建议）

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb06d62e0.png)

6、在`Tools`-`Build System`中选择`CPP`（你刚才设置的文件名）

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb08624f8.png)

7、用`sublime`打开一个`.cpp`文件，选择`Tools` - `Build With...`

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb09880af.png)

8、此时有两个选项，第一个选项表示`编译`，第二个选项表示`编译并在CMD运行`。

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb0a5a4e0.png)

9、运行结果如图。

![在这里插入图片描述](https://media.opennet.top/i/2023/01/05/63b6cb0c2e018.png)
