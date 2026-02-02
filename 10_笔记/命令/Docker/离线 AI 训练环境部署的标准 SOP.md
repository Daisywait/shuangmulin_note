---
类型: 命令
标签:
  - Docker
  - 离线部署
  - AI训练
  - SOP
---

### 方案 A：手动离线安装依赖（逐条命令）
- 命令
```bash
# 1) 本机拉取 CUDA 镜像（可联网机器）。
docker pull docker.1ms.run/nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
```

```bash
# 2) 导出镜像为 tar（离线传输用）。
docker save -o cuda12.4.1-cudnn-devel.tar docker.1ms.run/nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
```

```bash
# 3) 传输 tar 到工作站数据盘。
scp cuda12.4.1-cudnn-devel.tar linmiaoju@<工作站IP>:/data/linmiaoju/docker_images/
```

```bash
# 4) 工作站导入镜像。
docker load -i /data/linmiaoju/docker_images/cuda12.4.1-cudnn-devel.tar
```

```bash
# 5) 启动容器（挂载代码与数据盘）。
docker run -it --gpus all \
  -v /home/linmiaoju/jepa:/workspace/jepa \
  -v /data/linmiaoju/jepa_data:/workspace/data \
  -v /data/linmiaoju/.cache/pip:/root/.cache/pip \
  -v /data/linmiaoju/.cache/torch:/root/.cache/torch \
  -v /data/linmiaoju/.cache/huggingface:/root/.cache/huggingface \
  docker.1ms.run/nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04 \
  bash
```

```bash
# 6) 本机下载离线 wheels（可联网机器）。
mkdir -p wheels pip_wheels
pip download pip -d ./pip_wheels
pip download torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124 -d wheels
pip download transformers datasets accelerate sentencepiece -d wheels
pip3 download matplotlib scipy opencv-python tensorboard --platform manylinux2014_x86_64 --only-binary=:all: --python-version 3.12
```

```bash
# 7) 传输 wheels 到工作站。
scp -r ./wheels linmiaoju@<工作站IP>:/data/linmiaoju/.cache/pip/
```

```bash
# 8) 容器内离线安装 PyTorch wheel。
pip install --no-index --find-links=/root/.cache/pip/wheels torch-2.9.1-cp312-cp312-manylinux_2_28_x86_64.whl
```

```bash
# 9) 修正 pip 缓存目录权限。
sudo chown -R linmiaoju:linmiaoju /data/linmiaoju/.cache/pip/
sudo chmod -R 755 /data/linmiaoju/.cache/pip/
```

```bash
# 10) 创建训练日志目录。
mkdir -p /workspace/jepa/experiment_logs
```

```bash
# 11) 下载测试数据集（如需）。
wget https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz
```

- 参数说明
```bash
# save/load 对象是镜像（image）而不是容器（container）
# --no-index: 禁用联网
# --find-links: 指定离线 wheel 目录
```
- 解释：离线环境需先传镜像与 wheels；容器与缓存需挂载到数据盘避免占满系统盘。

---

### 方案 B：Dockerfile 离线构建镜像（docker build + docker run）

#### 1) 准备离线 wheels（可联网机器）
```bash
mkdir -p wheels
pip download torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124 -d wheels
pip download transformers datasets accelerate sentencepiece -d wheels
pip3 download matplotlib scipy opencv-python tensorboard --platform manylinux2014_x86_64 --only-binary=:all: --python-version 3.12
```

#### 2) 准备 Dockerfile（放在项目目录）
```Dockerfile
FROM docker.1ms.run/nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04

WORKDIR /workspace

# 复制离线 wheels（与 Dockerfile 同级的 wheels 目录）
COPY wheels /opt/wheels

# 安装基础依赖与 pip（如果镜像未自带）
RUN apt-get update && apt-get install -y python3 python3-pip && rm -rf /var/lib/apt/lists/*

# 离线安装 Python 依赖
RUN pip install --no-index --find-links=/opt/wheels torch torchvision torchaudio \
    transformers datasets accelerate sentencepiece matplotlib scipy opencv-python tensorboard

CMD ["bash"]
```

#### 3) 在工作站构建镜像（离线）
```bash
docker build -t jepa-offline:cuda124 .
```

#### 4) 运行容器
```bash
docker run -it --gpus all \
  -v /home/linmiaoju/jepa:/workspace/jepa \
  -v /data/linmiaoju/jepa_data:/workspace/data \
  jepa-offline:cuda124 \
  bash
```

- 参数说明
```bash
# Dockerfile COPY 需要 wheels 目录与 Dockerfile 同级
# docker build 必须在包含 Dockerfile 的目录执行
```
- 解释：离线构建通过 COPY wheels 完成依赖安装，避免在线下载。




