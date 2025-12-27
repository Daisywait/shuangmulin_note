>含 GitLab 容器管理

## 一、查看容器状态

```bash
# 查看所有容器（包括已停止的）
docker ps -a

# 仅查看正在运行的容器
docker ps
```

> 输出字段说明：
> - `CONTAINER ID`：容器唯一 ID  
> - `NAMES`：容器名称（如 `gitlab`）  
> - `STATUS`：运行状态（Up / Exited）

---

## 二、启动 / 停止 / 删除容器

### 启动已存在的容器
```bash
docker start gitlab          # 按名称启动
docker start <CONTAINER_ID>  # 按 ID 启动
```

### 停止运行中的容器
```bash
docker stop gitlab
```

### 删除容器（必须先停止）
```bash
docker rm gitlab
# 或强制删除（即使在运行）
docker rm -f gitlab
```

> ⚠️ 注意：**只有删除了使用某镜像的所有容器后，才能删除该镜像。**

---

## 三、进入容器内部调试

```bash
# 以交互方式进入容器
docker exec -it gitlab bash
```

### 示例：检查共享内存（/dev/shm）
```bash
docker exec gitlab df -h /dev/shm
```

> 💡 GitLab 推荐 `/dev/shm` 至少 256MB，否则可能启动失败。

---

## 四、镜像管理

### 查看本地镜像
```bash
docker images
```

### 删除镜像
```bash
# 先确保没有容器在使用该镜像！
docker rmi <IMAGE_ID>
# 例如：
docker rmi 7d79b4fee201
```

> ❌ 如果提示 “image is being used by container”，请先删除对应容器（如 `1bf2cdab1f0e`）。

---
### 根据Dockerfile构建镜像
```
docker build -t jepa:latest .#.表示当前目录为构建目录
```


## 五、查看日志与调试

```bash
# 实时查看容器日志
docker logs -f gitlab

# 查看最后 100 行日志
docker logs --tail 100 gitlab
```

> ✅ 用于排查 GitLab 是否成功启动、是否监听端口等。

---

## 六、GPU 状态检查（如使用 NVIDIA）

```bash
nvidia-smi
```

> 显示 GPU 使用情况、驱动版本、进程占用等信息。  
> 若未安装驱动或容器未启用 GPU，此命令可能报错。

---

## 七、典型 GitLab 容器操作流程

```bash
# 1. 停止并删除旧容器
docker stop gitlab
docker rm gitlab

# 2. （可选）删除旧镜像
docker rmi <old_image_id>

# 3. 重新运行新容器（略，需 docker run 命令）

# 4. 启动后检查
docker ps                    # 确认运行中
docker logs -f gitlab        # 查看启动日志
docker exec gitlab df -h /dev/shm  # 检查共享内存
```

---

✅ **小贴士**：
- 容器名（如 `gitlab`）是你 `docker run --name gitlab ...` 时指定的。
- 使用 `docker ps -a` 总是第一步，避免误删或重复创建。
- 删除镜像前务必确认无容器依赖！
