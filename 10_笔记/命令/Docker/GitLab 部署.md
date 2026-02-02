---
类型: 命令
标签:
  - 离线部署
  - Docker
  - Gitlab
---

- 命令
```bash
# 拉取 GitLab 镜像。
docker pull gitlab/gitlab-ce:latest
```

```bash
# 在服务器上用 Docker 启动 GitLab 容器。
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

```bash
# 查看 GitLab 启动日志（初始化可能较久）。
docker logs -f gitlab
```

```bash
# 设置 external_url（影响访问与克隆链接）。
docker exec -it gitlab bash -c "sed -i 's|^external_url.*|external_url \"http://172.31.179.162:8929\"|' /etc/gitlab/gitlab.rb"
```

```bash
# 重新加载 GitLab 配置。
docker exec -it gitlab gitlab-ctl reconfigure
```

```bash
# 打开 GitLab Web（示例端口 8929）。
http://172.31.179.162:8929/
```

```bash
# 获取初始 root 密码（容器内读取）。
docker exec -it gitlab cat /etc/gitlab/initial_root_password
```

- 参数说明
```bash
# -p 8929:80 Web 端口
# -p 2224:22 SSH 端口
# -v 挂载配置/日志/数据
# --shm-size 共享内存大小
# external_url: 对外访问与克隆地址的根 URL
```
- 解释：忘记挂载 /data 会导致数据丢失；只改端口不改 external_url 会导致链接不一致。


