---
类型: 概念
标签: [ROS2, Launch]
---



- 一句话：ROS2 launch 文件入口为 generate_launch_description() 并返回 LaunchDescription。
- 关键：launch 系统通过入口函数构建启动描述。
- 误区：把 launch 当普通脚本入口运行会失效。
- 例子：使用 generate_launch_description() 组织节点与参数。







