---
类型: 命令
标签:
  - 节点话题
  - ROS2
---

- 命令
```bash
# 运行节点
ros2 run <pkg> <node>
```

```bash
# 启动 launch 文件
ros2 launch <pkg> <file.launch.py>
```

```bash
# 查看节点列表
ros2 node list
```

```bash
# 查看话题列表
ros2 topic list
```

```bash
# 查看话题内容
ros2 topic echo /topic_name
```

```bash
# 查看话题发布频率
ros2 topic hz /topic_name
```

```bash
# 查看服务列表
ros2 service list
```

```bash
# 调用服务（示例：std_srvs/Empty）
ros2 service call /service_name std_srvs/srv/Empty {}
```

```bash
# 查看参数列表
ros2 param list
```

```bash
# 获取参数值
ros2 param get /node_name param_name
#ros2 param get /moveit_servo command_in_type
```

```bash
# 设置参数值
ros2 param set /node_name param_name value
```

- 参数说明
```bash
# <pkg>: 包名
# <node>: 节点名
# <file.launch.py>: 启动文件
# /topic_name: 话题名
# /service_name: 服务名
# /node_name: 节点名
```

- 解释：节点/话题/服务/参数的日常查看与调试命令。


