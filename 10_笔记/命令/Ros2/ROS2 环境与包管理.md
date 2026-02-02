---
类型: 命令
标签:
  - ROS2
  - 包管理
  - 系统版本
---

- 命令
```bash
# 查看当前 ROS 版本
printenv ROS_DISTRO
```

```bash
# 载入 ROS2 环境（示例：Humble）
source /opt/ros/humble/setup.bash
```

```bash
# 创建工作空间并初始化
mkdir -p ~/ros2_ws/src

cd ~/ros2_ws

colcon build
```

```bash
# 载入工作空间环境
source ~/ros2_ws/install/setup.bash
```

```bash
# 创建 ROS2 包（示例：C++）
cd ~/ros2_ws/src

ros2 pkg create my_pkg --build-type ament_cmake
```

```bash
# 列出所有包
ros2 pkg list
```

- 参数说明
```bash
# --build-type: 构建类型（ament_cmake / ament_python）
```

- 解释：环境初始化与工作空间/包管理的常用命令。
