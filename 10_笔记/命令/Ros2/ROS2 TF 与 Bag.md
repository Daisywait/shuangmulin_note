---
类型: 命令
标签:
  - ROS2
  - TF和bag
---

- 命令
```bash
# 查看 TF 树（需要 tf2_tools）
ros2 run tf2_tools view_frames
```

```bash
# 录制 bag
ros2 bag record /topic1 /topic2
```

```bash
# 播放 bag
ros2 bag play <bag_dir>
```

- 参数说明
```bash
# <bag_dir>: 录制目录
```

- 解释：TF 可视化与 bag 录制/回放的常用命令。


