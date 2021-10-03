---
title: WSL2 and Windows Terminal
date: 2021/10/03 13:28
---

> Windows 是最好的 Linux 发行版。

WSL2 发布以后就一直有人这么评论。最近发现 WSL2 和 Windows Terminal 配合起来无比好用。

## 效果

- 按 Win 键，然后输入 Windows，回车，就能在自己用户文件夹下打开 PowerShell
- 按 Win 键，然后输入 Ubuntu，回车，就能在 Linux 用户文件夹下打开 Ubuntu 命令行
- 按 Win 键，然后输入 wt，回车，就能在自己用户文件夹下打开 Ubuntu 命令行
- 任意文件夹内，右键，选择 在 Windows 终端 中打开，就能在此处打开 PowerShell
- 任意文件夹内，右键，选择 在 Windows 终端 预览版 中打开，就能在此处打开 Ubuntu 命令行

## 操作

1. 准备两个 Windows Terminal（Windows Terminal 和 Windows Terminal Preview）
2. 设置默认终端应用程序为 Windows Terminal Preview（在 Windows Terminal 的设置里面可以找到这个选项）
3. 在 Windows Terminal Preview 的设置里面，将默认配置文件改成 WSL2（比如 Ubuntu）

