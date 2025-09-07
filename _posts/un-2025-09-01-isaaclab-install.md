---
permalink: /blog/isaaclab-install/
share: false
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

# IsaacLab 安装指南

## IsaacLab 简介

## 前置条件

IsaacLab 依赖 Isaac Sim，因此需要先安装 Isaac Sim。

Isaac Sim 对系统和硬件有一定要求，具体请参考[官方文档](https://docs.isaacsim.omniverse.nvidia.com/latest/installation/requirements.html)

### Isaac Sim 本地安装

使用 Isaac Lab 有多种安装 Isaac Sim 的方式：

- Pip 安装：这种方式较为方便，但只能在一个 conda/python 环境中使用，如果需要多个环境使用，需要重复安装。
- 二进制文件 安装： 这种方式可以在多个 conda/python 环境中使用，但需要手动配置环境变量，且安装和配置较为复杂。

#### Pip 安装

参考[Isacc Sim Pip Install](https://docs.robotsfan.com/isaaclab/source/setup/installation/pip_installation.html)

#### 二进制文件 安装

参考[Isacc Sim Quick Install](https://docs.isaacsim.omniverse.nvidia.com/latest/installation/quick-install.html)
与[使用Isaac Sim二进制安装](https://docs.isaacsim.omniverse.nvidia.com/latest/installation)

### Isaac Sim Docker 安装

由于 Isaac Sim 的安装和配置较为复杂，如果需要多平台支持，建议使用 Docker 方式安装 Isaac Sim。
并且有些系统（如 Ubuntu 20.04）可能无法直接安装 Isaac Sim，可以通过 Docker 方式安装。