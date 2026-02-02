---
类型: 概念
标签: [ROS2, 话题, Action]
---


- 一句话：速度指令走 topic，夹爪控制走 action。
- 关键：Publisher 发布 TwistStamped，ActionClient 发送 GripperCommand。
- 误区：把 action 当成普通 topic 发布。
- 例子：VR 位移 → /moveit_servo/delta_twist_cmds；夹爪 → gripper_cmd。
- 链接：






