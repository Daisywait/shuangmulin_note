---
类型: 命令
标签:
  - Docker
---

- 命令
```bash
# 查看所有容器（包含已停止的）
docker ps -a
```

```bash
# 只查看运行中的容器
docker ps
```

```bash
# 启动容器（按名称）
docker start gitlab
```

```bash
# 启动容器（按 ID）
docker start <CONTAINER_ID>
```

```bash
# 停止运行中的容器
docker stop gitlab
```

```bash
# 删除容器（需先停止）
docker rm gitlab
```

```bash
# 强制删除容器（即使在运行）
docker rm -f gitlab
```

```bash
# 进入容器交互式终端
docker exec -it gitlab bash
```

```bash
# 查看容器共享内存大小
docker exec gitlab df -h /dev/shm
```

```bash
# 查看本地镜像
docker images
```

```bash
# 删除镜像（确保没有容器占用）
docker rmi <IMAGE_ID>
```

```bash
# 基于 Dockerfile 构建镜像
docker build -t jepa:latest .
```

```bash
# 实时查看容器日志
docker logs -f gitlab
```

```bash
# 查看最近 100 行日志
docker logs --tail 100 gitlab
```

- 参数说明
```bash
# -a: 显示全部（包含已停止）
# -f: 持续跟踪日志输出
# --tail N: 只看最近 N 行
# -t: 镜像标签
# <CONTAINER_ID>: 容器 ID
# <IMAGE_ID>: 镜像 ID
```
- 解释：容器启动失败时优先看日志；删除镜像前需先删除使用它的容器；/dev/shm 太小会导致部分服务报错。

- 链接
  - [[shuangmulin/10_笔记/命令/Docker/GitLab 部署]]
  - [[离线 AI 训练环境部署的标准 SOP]]
  - [[共享内存（dev_shm 与 --shm-size）]]





