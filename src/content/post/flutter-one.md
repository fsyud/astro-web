---
title: "Build flutter desktop music program-01"
description: "flutter notes"
publishDate: " 05 Dec 2023"
tags: ["todo"]
draft: true
---

[项目地址 flutter-music](https://github.com/fsyud/music-flutter)

在 Windows 操作系统上安装和配置 Flutter 开发环境

> 本文基于window环境开发

## 系统配置要求

为了安装和运行 Flutter，你的开发环境必须至少满足以下要求：

- 操作系统：Windows 10 或更高的版本（基于 x86-64 的 64 位操作系统）。
- 磁盘空间：除安装 IDE 和一些工具之外还应有至少 2.5 GB 的空间。
- 工具：要让 Flutter 在你的开发环境中正常使用，依赖于以下的工具：
  > [Windows PowerShell 5.0](https://learn.microsoft.com/en-us/powershell/scripting/windows-powershell/overview?view=powershell-7.4) 或者更高的版本（Windows 10 中已预装）
  > [Git for Windows 2.x](https://git-scm.com/download/win)，并且勾选从 Windows 命令提示符使用 Git 选项。
  > 如果 Windows 版的 Git 已经安装过了，那么请确保能从命令提示符或者 PowerShell 中直接执行 git 命令。

## 获取 Flutter SDK

**重点提醒: 国内的网络环境下可能需要对 Flutter 工具进行一些额外配置，请参考文档 在中国网络环境下使用 Flutter。**

1.点击下方的安装包，获取 stable 发行通道的 Flutter SDK 最新版本：
[flutter_windows_3.16.5-stable](https://storage.flutter-io.cn/flutter_infra_release/releases/stable/windows/flutter_windows_3.16.5-stable.zip)

    要查看其他发行通道和以往的版本，请参阅 SDK 版本列表 页面。

2.将压缩包解压，然后把其中的 flutter 目录整个放在你想放置 Flutter SDK 的路径中（例如 %USERPROFILE%\flutter 或者 D:\dev\flutter）。

> 请注意:

**请勿将 Flutter 有特殊字符或空格的路径下。**

> 请注意:

**请勿将 Flutter 安装在需要高权限的文件夹内，例如 C:\Program Files\。**

待续...
