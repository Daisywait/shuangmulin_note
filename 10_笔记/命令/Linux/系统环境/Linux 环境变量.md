---
类型: 命令
标签: [Linux, 环境变量]
---

- 命令
```bash
# 设置环境变量（当前终端）
export VAR=/path/to/dir
```

```bash
# 查看常用环境变量
echo $HF_HOME

echo $PATH

echo $HOME

echo $USER
```

```bash
# 让配置永久生效
source ~/.bashrc
```

- 参数说明
```bash
# export: 设置当前终端环境变量
# source: 重新加载配置文件
```

- 解释：环境变量用于让系统定位命令与资源路径，常配合 ~/.bashrc 持久化。
