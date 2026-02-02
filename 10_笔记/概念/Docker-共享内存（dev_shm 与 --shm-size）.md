---
分类: 概念
标签: [Docker, 共享内存]
---

## 一句话
在 Docker 中通过 `--shm-size` 调整容器 `/dev/shm`（共享内存）大小，避免依赖共享内存的进程因空间过小而报错。

## 关键机制
- `/dev/shm` 是基于内存的临时文件系统（tmpfs），读写快但不持久化。
- Docker 容器默认 `/dev/shm` 只有 64MB，很多服务（如 GitLab）会不够用。
- 共享内存用于进程间快速通信、临时对象缓存、数据库排序/临时表等场景。

## 适用场景
- GitLab、PostgreSQL、Gitaly、Sidekiq 等需要较大共享内存的服务。
- 出现 `Cannot allocate memory`、`pack-objects failed` 等与内存相关的报错。

## 误区
- 只加大容器内存，不设置 `--shm-size`，共享内存仍可能过小。
- 以为 `/dev/shm` 持久化，容器重启后数据会丢失。

## 例子
```bash
# 启动 GitLab 容器并把 /dev/shm 设置为 2GB
docker run --detach \
  --hostname gitlab.local \
  --name gitlab \
  --shm-size=2g \
  -p 8929:80 -p 2224:22 \
  -v /data/gitlab/config:/etc/gitlab \
  -v /data/gitlab/logs:/var/log/gitlab \
  -v /data/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```




