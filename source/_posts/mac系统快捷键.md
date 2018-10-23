---
title: mac vscode快捷键
date: 2018-10-17 16:08:17
tags: mac
categories: mac
---

* 关闭窗口 / 标签 `command + W`
* 浏览器 / 终端 新建标签 `command + T`
* mini窗口最小化 `command + M`
* 窗口最大化和还原 `command + ctrol + F`
<!-- more -->
* hide隐藏最上层窗口 `command + H`
* 隐藏最上层以外的窗口 `command + option + H`
* 复制 / 粘贴 / 全选 / 撤销 `command + C / V / A / Z`
* 浏览器刷新页面 `command + r`
* 自定义快捷键打开app[https://www.jianshu.com/p/abeb1123178d](https://www.jianshu.com/p/abeb1123178d)
* 自定义打开 iTerm2 `command + .`
* 自定义打开 chrome `command + shift + .`
* 自定义打开 vscode `command + '`
* 自定义打开 wechat `command + shift + '`

>关闭第三方程序验证
```bash
$ sudo spctl --master-disable
$ defaults write com.apple.LaunchServices LSQuarantine -bool false
```