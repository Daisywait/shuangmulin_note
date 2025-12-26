明白了，Anne，我们先把注意力放回 **Docker 镜像已经在本地 pull 完** 的情况。

---

### 1️⃣ 镜像下载完成后做什么

假设你在本地电脑成功拉取了：

```bash
docker pull docker.1ms.run/nvidia/cuda:12.4.0-devel-ubuntu22.04
```

接下来要做的主要步骤：

---

### 2️⃣ 导出镜像为 tar 文件（离线搬运）

因为工作站无法连外网，你需要把镜像从本地电脑搬到工作站：

```bash
docker save -o cuda12.8-cudnn-devel.tar docker.1ms.run/nvidia/cuda:12.8.0-cudnn-devel-ubuntu22.04
```

- `-o cuda12.8-cudnn-devel.tar` → 生成一个可拷贝的文件
    
- 这个 tar 文件大小可能几 GB，需要拷贝到工作站的数据盘或用移动存储
    

---

### 3️⃣ 拷贝到工作站

假设你工作站的数据盘路径是 `/data/linmiaoju/docker_images/`：

```bash
scp cuda12.8-cudnn-devel.tar linmiaoju@<工作站IP>:/data/linmiaoju/docker_images/
```

或者用移动硬盘直接拷贝到 `/data/linmiaoju/docker_images/`

---

### 4️⃣ 在工作站导入镜像

工作站上执行：

```bash
docker load -i /data/linmiaoju/docker_images/cuda12.8-cudnn-devel.tar
```

- 成功后执行 `docker images` 可以看到镜像已经在工作站上了
    
- **注意**：普通用户是否能访问 Docker，要看管理员是否允许
    

---

### 5️⃣ 启动容器并挂载目录

挂载代码和数据盘，让工作站运行 JEPA：

```bash
docker run -it --gpus all \
    -v /home/linmiaoju/jepa:/workspace/jepa \
    -v /data/linmiaoju/jepa_data:/workspace/data \
    docker.1ms.run/nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04 \
    bash
```

- `/workspace/jepa` → 容器内代码目录
    
- `/workspace/jepa_data` → 容器内数据目录
    

---

💡 总结：

1. 本地 pull 镜像 → docker save 导出 tar
    
2. 拷贝 tar 到工作站 → docker load 导入
    
3. 挂载代码和数据盘 → 进入容器跑 JEPA
    

---

Anne，如果你愿意，我可以直接帮你写一条 **完整命令，从本地镜像导出到工作站运行 JEPA 的全流程**，直接可用，不用每步都敲命令。

你希望我写吗？




### Gitlab
网址：172.31.179.162:8289
```bash

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
![[Pasted image 20251115150550.png]]
### 查看容器日志

`docker logs -f gitlab`

- `-f` 表示实时跟踪日志
    
- 你会看到 GitLab 初始化的各种信息，例如数据库迁移、服务启动等

挂载：
-v /data/gitlab/config:/etc/gitlab   # GitLab 配置文件
-v /data/gitlab/logs:/var/log/gitlab # GitLab 日志
-v /data/gitlab/data:/var/opt/gitlab # GitLab 数据库和仓库


### 修改 `external_url`（可选，但推荐）

如果你打算通过宿主机 IP + 映射端口访问，修改 GitLab 配置：

`docker exec -it gitlab bash vi /etc/gitlab/gitlab.rb`

把这一行修改：

`external_url 'http://172.31.179.162:8929'`

[[vi编辑器]]

### 1️⃣ 基本概念

`external_url` 是 GitLab 的配置项，用来告诉 GitLab **外部用户访问它时的完整 URL**。

- 它不仅是一个访问地址，也会影响 GitLab 生成的各种链接。
    
- 在 GitLab 配置文件 `/etc/gitlab/gitlab.rb` 里设置，例如：
    

`external_url 'http://172.31.179.162:8929'`

---

### 2️⃣ 它控制的内容

1. **网页访问 URL**
    
    - 你访问浏览器里的 GitLab 页面时用的地址就是 `external_url`。
        
2. **仓库克隆 URL**
    
    - Git 仓库的 HTTPS 克隆地址就是基于 `external_url` 自动生成的。
        
    - 例子：如果 `external_url` 是 `http://172.31.179.162:8929`，仓库克隆地址会是：
        
        `http://172.31.179.162:8929/username/project.git`
GitLab 首次启动需要较长时间（通常 2-10 分钟）进行初始化。！！！！！



“写 IP 连宿主机，写 gitlab 连容器！”！！！！