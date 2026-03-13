---
permalink: /blog/isaaclab-install/
share: false
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

# IsaacLab 安装指南

## IsaacLab 简介

## 前置条件与 Isaac Sim 安装

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

以下以 Ubuntu 20.04 为例，介绍如何使用 Docker 方式安装和部署 Isaac Sim。

#### container setup

1. 安装 Docker

```bash
# Docker installation using the convenience script
curl -fsSL https://get.docker.com -o get-docker.sh
# sudo -E sh get-docker.sh 使用代理运行安装脚本
sudo sh get-docker.sh

# Post-install steps for Docker
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker

# Verify Docker
docker run hello-world
```

2. 安装 NVIDIA Container Toolkit 以支持在 Docker 容器中使用 GPU

```bash
# Configure the repository
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
    && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
    && \
    sudo apt-get update

# Install the NVIDIA Container Toolkit packages
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker

# Configure the container runtime
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

# Verify NVIDIA Container Toolkit
docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi
```

#### container deployment

1. 拉取 Isaac Sim Docker 镜像

```bash
# 拉取最新的 Isaac Sim Docker 镜像
docker pull nvcr.io/nvidia/isaac-sim:5.0.0
```

2. 使用交互式 bash 运行 Isaac Sim Docker 容器

```bash
docker run --name isaac-sim --entrypoint bash -it --runtime=nvidia --gpus all -e "ACCEPT_EULA=Y" --rm --network=host \
    -e "PRIVACY_CONSENT=Y" \
    -v ~/docker/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw \
    -v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw \
    -v ~/docker/isaac-sim/cache/pip:/root/.cache/pip:rw \
    -v ~/docker/isaac-sim/cache/glcache:/root/.cache/nvidia/GLCache:rw \
    -v ~/docker/isaac-sim/cache/computecache:/root/.nv/ComputeCache:rw \
    -v ~/docker/isaac-sim/logs:/root/.nvidia-omniverse/logs:rw \
    -v ~/docker/isaac-sim/data:/root/.local/share/ov/data:rw \
    -v ~/docker/isaac-sim/documents:/root/Documents:rw \
    nvcr.io/nvidia/isaac-sim:5.0.0
```

命令解析：
- `--name isaac-sim`：指定容器名称为 `isaac-sim`。
- `--entrypoint bash`：将容器的入口点设置为 `bash`
- `-it`：以交互模式运行容器，并分配一个伪终端。
- `--runtime=nvidia --gpus all`：启用 NVIDIA GPU 支持。
- `-e "ACCEPT_EULA=Y"`：接受最终用户许可协议。
- `--rm`：容器停止后自动删除容器。
- `--network=host`：使用主机的网络设置。
- `-v ~/docker/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw`：将主机的缓存目录挂载到容器中，以便缓存数据的持久化。
- 其他 `-v` 选项类似，用于挂载不同的缓存和数据目录。
- `nvcr.io/nvidia/isaac-sim:5.0.0`：指定要使用的 Docker 镜像。

3. 在容器中启动 Isaac Sim 的直播模式

```bash
./runheadless.sh -v
```

4. 在本地通过 ` Isaac Sim WebRTC Streaming Client` 访问容器中运行的 Isaac Sim
    - 下载 `Isaac Sim WebRTC Streaming Client`：https://docs.isaacsim.omniverse.nvidia.com/latest/installation/download.html#isaac-sim-latest-release
    - 启动 `Isaac Sim WebRTC Streaming Client`，在 `Server` 中输入 `ip`，点击 `Connect` 即可连接到容器中的 Isaac Sim。其中 `ip` 是运行容器的主机的 IP 地址
    - 连接成功后，即可在本地使用 Isaac Sim
![Isaac Sim WebRTC Streaming Client](/assets/images/isaac-sim-webrtc.png)


## Isaac Lab 安装

### 本地安装

可参考上一部分中的教程

**如果使用二进制文件安装 Isaac Sim**，注意配置环境变量

```bash
# Isaac Sim root directory
export ISAACSIM_PATH="${HOME}/isaac-sim"
# Isaac Sim python executable
export ISAACSIM_PYTHON_EXE="${ISAACSIM_PATH}/python.sh"
```

检查仿真器是否可以正确运行

```bash
# note: you can pass the argument "--help" to see all arguments possible.
${ISAACSIM_PATH}/isaac-sim.sh

# checks that python path is set correctly
${ISAACSIM_PYTHON_EXE} -c "print('Isaac Sim configuration is now complete.')"
# checks that Isaac Sim can be launched from python
${ISAACSIM_PYTHON_EXE} ${ISAACSIM_PATH}/standalone_examples/api/isaacsim.core.api/add_cubes.py
```

**克隆 Isaac Lab 仓库**

```bash
git clone git@github.com:isaac-sim/IsaacLab.git
# or
git clone https://github.com/isaac-sim/IsaacLab.git
```

**如果使用二进制文件安装 Isaac Sim**，需要创建符号链接；如果使用 Pip 安装 Isaac Sim，则跳过此步骤

```bash
# enter the cloned repository
cd IsaacLab
# create a symbolic link
ln -s <path_to_isaac_sim> _isaac_sim
# For example: ln -s ${HOME}/isaacsim _isaac_sim
```

使用辅助脚本构建 conda 环境

```bash
# Option 1: Default name for conda environment is 'env_isaaclab'
./isaaclab.sh --conda  # or "./isaaclab.sh -c"
# Option 2: Custom name for conda environment
./isaaclab.sh --conda my_env  # or "./isaaclab.sh -c my_env"

# Activate the conda environment
conda activate env_isaaclab  # or "conda activate my_env"
```

安装依赖

```bash
# these dependency are needed by robomimic which is not available on Windows
sudo apt install cmake build-essential

./isaaclab.sh --install # or "./isaaclab.sh -i"
# only part
./isaaclab.sh --install rl_games  # or "./isaaclab.sh -i rl_games"
```

验证 Isaac Lab 是否安装成功

```bash
# Option 1: Using the isaaclab.sh executable
# note: this works for both the bundled python and the virtual environment
./isaaclab.sh -p scripts/tutorials/00_sim/create_empty.py

# Option 2: Using python in your virtual environment
python scripts/tutorials/00_sim/create_empty.py
```

### Docker 安装与部署