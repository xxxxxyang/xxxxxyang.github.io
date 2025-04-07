---
permalink: /blog/legged_gym/
share: false
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

legged_gym: 使用 issac gym 进行四足机器人仿真的核心配置目录

## 目录结构

```bash
legged_gym\ 
    │  setup.py
    │
    ├─legged_gym\                                   核心目录
    │  │  __init__.py                               legged_gym 包初始化，主要是定义一些全局变量记录路径
    │  │
    │  ├─envs\                                      机器人、环境的及训练设置脚本目录，包括地形设置、强化学习算法PPO参数、奖励函数定义、读取机器人模型urdf设置等
    │  │  │  __init__.py
    │  │  │
    │  │  ├─anymal_c\                               以 anymal_c 机器人为例
    │  │  │  │  anymal.py                           仿真环境中模拟机器人的控制器驱动，例如通过 anydrive 神经网络驱动关节，或通过 PD 控制器驱动关节
    │  │  │  │
    │  │  │  ├─flat\
    │  │  │  │      anymal_c_flat_config.py         平坦地形参数、机器人配置和控制参数、奖励函数参数(~Cfg) 及相应的PPO算法配置(~CfgPPO) 分别继承自 LeggedRobotCfg 和 LeggedRobotCfgPPO
    │  │  │  │
    │  │  │  └─mixed_terrains\
    │  │  │          anymal_c_rough_config.py       崎岖地形参数(~Cfg) 及相应的PPO算法配置(~CfgPPO)
    │  │  │
    │  │  ├─base\                                   各个机器人配置与环境地形配置的基类
    │  │         base_config.py                     LeggedRobotCfg 类的基类 BaseCfg
    │  │         base_task.py                       LeggedRobot的 基类 BaseTask
    │  │         legged_robot.py                    **LeggedRobot 类的具体定义**
    │  │         legged_robot_config.py             **LeggedRobotCfg 类的具体定义**
    │  │
    │  ├─scripts\
    │  │      play.py                               查看训练效果脚本
    │  │      train.py                              训练脚本
    │  │
    │  ├─tests\
    │  │      test_env.py                           测试训练环境搭建
    │  │
    │  └─utils\
    │          helpers.py                           辅助方法 参数解析，配置更新等
    │          logger.py                            log 文件读取及存储
    │          math.py                              数学函数，处理四元数、角度及生成特定分布随机数等方法
    │          task_registry.py                     管理和注册不同的仿真环境(任务)
    │          terrain.py                           地形生成、地形管理、地形转换等方法，应该是被Cfg调用
    │          __init__.py
    │
    ├─licenses\                                     证书，忽略
    │  └─...
    │
    └─resources\                            机器人仿真相关的物理建模数据
        ├─actuator_nets\                    控制器驱动网络示例，驱动 anydrive 无刷电机的 lstm 神经网络
        │      anydrive_v3_lstm.pt
        │
        └─robots\                           机器人的 urdf 和 meshes 文件，URDF 文件：定义机器人的结构、物理属性和关节连接方式。Meshes 文件：提供机器人的详细几何形状，用于渲染和碰撞检测。
```


## 训练时的调用关系



## 运行时的调用关系

## 关键文件

### legged_gym/envs/base/

#### base_config.py

#### legged_robot_config.py

#### base_task.py

#### legged_robot.py

### legged_gym/utils/task_registry.py

### legged_gym/scripts/

#### train.py

#### play.py

## 深度图像处理

深度图像的帧率为 `dt x update_interval = 1/0.02x5 = 10(帧/s)`；每经过 `update_interval` 个 `step` 更新一次 `depth_buffer`

depth_buffer 的类型为：[envs, buffer_len, height, width] 的四维张量