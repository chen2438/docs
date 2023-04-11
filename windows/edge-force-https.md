---
description: 转载于 2023-04-11
categories:
- windows
date: 2019-04-13
slug: edge-force-https
title: Edge、Chrome 强制跳转 https 网页解决办法
---

原文链接：https://juejin.cn/post/7113754454440017951

1.在浏览器输入：edge输入：edge://net-internals/#hsts，谷歌输入：chrome://net-internals/#hsts
2.左侧菜单点击 “domain security policy”
3.在最下方“Delete domain security policies” 输入要删除自动跳转的域名
4.点击delete

![image-20230411225134671](https://media.opennet.top/i/2023/04/11/643573f88d6d7.png)
