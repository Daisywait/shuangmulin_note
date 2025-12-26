#### ROS2 Launch文件的特殊结构

##### 1. 入口函数是 `generate_launch_description()`

```python

def generate_launch_description():
    # 所有启动配置在这里定义
    return LaunchDescription([...])
```
这是ROS2 launch文件的固定入口点，ROS2的launch系统会自动调用这个函数。


get_package_share_directory函数：
这个函数返回指定ROS2包的 **`share` 目录**的完整路径。


## 总结对比

| 特性        | `arguments` (spawner) | `parameters` (joint_state_publisher) |
| --------- | --------------------- | ------------------------------------ |
| **传递方式**  | 命令行参数                 | ROS2参数系统                             |
| **运行时修改** | 不可修改                  | 可以动态修改                               |
| **适用场景**  | 一次性工具                 | 持续服务节点                               |
| **代码获取**  | `sys.argv[1]`         | `get_parameter()`                    |
| **管理方式**  | 简单直接                  | 统一配置管理                               |