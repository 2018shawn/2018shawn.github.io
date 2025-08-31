---
title: "Ubuntu上从snap安装的VSCode无法输入中文问题"
date: 2025-08-31 11:33:00 +0800
top: false
cover: false
password:
toc: true
mathjax: true
summary:
tags:
categories: Ubuntu VSCode Snap
---
### 问题描述：
Ubuntu的snap商店下载安装的VS Code切换为中文输入法后依旧无法输入中文。

### 问题分析：
Snap应用运用在严格的沙箱中，访问接口受限制。

### 解决方法：
1.卸载在snap上安装的VS Code
2.
方法一：通过下载VS Code.deb文件进行安装：

方法二：通过包管理工具apt添加VSCode外部存储库，交给apt来管理，执行如下步骤的命令行：

下载密钥并将密钥添加到自己的密钥环目录中，这里让我们在 /etc/apt/keyrings 中添加 Microsoft 库的密钥：
```bash
curl -sS https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /etc/apt/keyrings/microsoft.gpg
```

在源列表文件 /etc/apt/sources.list.d/microsoft.list 中写入源库信息及签名信息。
```bash
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/vscode stable main" | sudo tee /etc/apt/sources.list.d/microsoft.list
```

更新软件包索引后安装：
```bash
sudo apt update
sudo apt install code
```

参考链接：
[Ubuntu22.04 使用 apt 安装VSCode](https://zhuanlan.zhihu.com/p/695350898)
