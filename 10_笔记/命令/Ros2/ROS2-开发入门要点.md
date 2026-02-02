---
类型: 命令
标签: [ROS2, 入门, 权限]
---

- 一句话：ROS2 入门需要理解工作空间结构、环境变量与可执行权限。
- 关键：
  - `colcon build` 后生成 `build/ install/ log/`，可执行文件在 `install`。
  - `source install/setup.bash` 让 `ros2 run` 能找到可执行文件；可写入 `~/.bashrc`。
  - `chmod +x` 赋予执行权限；Shebang 可直接 `./file` 运行。
- 误区：
  - `.py` 后缀不决定可执行性，权限才是关键。
  - 以为源码位置就是可执行位置，实际在 `install`。
- 例子：
  - `source ~/ros2_ws/install/setup.bash` 或写入 `~/.bashrc`。
  - `chmod +x node.py` 后直接 `./node.py` 运行。
- 链接
  - [[ROS2 环境与包管理]]
