---
permalink: /blog/copilot-sign/
share: false
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

# 远程服务器 GitHub Copilot 无法登录解决方法

## 背景

当远程服务器由于 443 端口被防火墙阻断，或校园网等网络环境限制，导致无法直接访问 GitHub 服务时，GitHub Copilot 插件会因为无法连接到 GitHub 而无法登录。

## 解决方法

一种方法是在远程服务器上使用代理服务器，可以参考[这篇文章](./2025-09-07-proxy.md)配置 SSH 隧道代理。

另一种方法是直接使用本地已经登录验证过的 GitHub Copilot 插件，在远程服务器端的 Vscode 窗口只是设置一个 UI 来显示和编辑代码。

这里着重介绍第二种方法。

### 设置 UI

1. 在远程服务器的 Vscode 窗口中，打开 settings.json 文件
    - 控制台命令：`Ctrl + Shift + P`，输入 `Preferences: Open Settings (JSON)` 打开 settings.json 文件。
2. 添加以下配置：
    ```json
        "remote.extensionKind": {
        "GitHub.copilot": ["ui"],
        "GitHub.copilot-chat": ["ui"],
    },
    ```
3. reload Vscode 窗口