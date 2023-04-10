---
description: 转载于 2023-04-10
categories:
- windows
date: 2023-04-10
slug: win-install-flex
title: Windows 安装 Flex
---

# Windows 安装 Flex

原文链接：https://blog.csdn.net/m944256098a/article/details/104992880

原文转载于：http://xiezs.uicp.top/archives/2020032001

### 下载安装包

Flex 安装包下载地址: https://gnuwin32.sourceforge.net/packages/flex.htm

![image-20230410175502582](https://media.opennet.top/i/2023/04/10/6433dcf91b6eb.png)

单击 Setup，会跳转到 SourceForge 下载。

### 配置环境变量

![image-20230410175811790](https://media.opennet.top/i/2023/04/10/6433ddb762fdf.png)

![image-20230410175832229](https://media.opennet.top/i/2023/04/10/6433ddca5c9fd.png)

![image-20230410175905722](https://media.opennet.top/i/2023/04/10/6433ddebd22bb.png)

![image-20230410175935417](https://media.opennet.top/i/2023/04/10/6433de0991a64.png)

根据自己的安装位置配置环境变量。

### 测试 Flex

新建测试文件 `f.l`

```cpp
%%  
[+-]?[0-9]+    { printf("%s\n", yytext); }    /* Print integers */  
\n    {}    /* newline */  
.    {}    /* For others, do nothing */  
%%  
void main(){  
    yylex();  
}   
int yywrap(){  
    return 1;  
}
```

打开命令行（cmd或powershell）并切换到代码所在目录，运行如下命令

```bash
flex f.l
```

该命令运行完成后将生成C语言代码lex.yy.c文件，接下来编译运行这个文件

```bash
gcc lex.yy.c
```

运行完之后将生成一个a.exe文件，运行即可（打不开可以尝试以管理员身份运行），这是一个数字提取的程序

![image-20230410180257311](https://media.opennet.top/i/2023/04/10/6433ded349e76.png)