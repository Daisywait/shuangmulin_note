- [x] 在本机拉取镜像 ✅ 2025-11-16
```
docker pull docker.1ms.run/nvidia/cuda:12.4.0-devel-ubuntu22.04
````
- [x] 导出为tar且scp到工作站 ✅ 2025-11-16
```bash
docker save -o cuda12.8-cudnn-devel.tar docker.1ms.run/nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
scp cuda12.4.1-cudnn-devel.tar linmiaoju@<工作站IP>:/data/linmiaoju/docker_images/
```
- [x] 工作站导入镜像 ✅ 2025-11-16
```
docker load -i /data/linmiaoju/docker_images/cuda12.4.1-cudnn-devel.tar
```
- [x] 启动容器并挂载目录 ✅ 2025-11-16
```bash
  jepa:                                 # ← 这里必须比 gitlab 再缩进 2 个空格
    container_name: jepa
    image: docker.1ms.run/nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
    runtime: nvidia
    tty: true
    stdin_open: true
    working_dir: /workspace/jepa
    volumes:
      - /home/linmiaoju/jepa:/workspace/jepa
      - /data/linmiaoju/jepa_data:/workspace/data
      - /data/linmiaoju/.cache/huggingface:/root/.cache/huggingface
      - /data/linmiaoju/.cache/pip:/root/.cache/pip
      - /data/linmiaoju/.cache/torch:/root/.cache/torch
      - /data/linmiaoju/.conda:/opt/conda  # 挂载整个conda目录
    environment:
      - PATH=/opt/conda/bin:$PATH
    command: bash
```
```bash
# 在 docker-compose.yml 所在目录
docker-compose up -d jepa
```
- [x] 本机电脑安装库、包 ✅ 2025-11-16
```bash
#到Ubuntu
#pip安装脚本和wheel文件
curl -O https://bootstrap.pypa.io/get-pip.py
pip download pip -d ./pip_wheels
#torch
pip download torch torchvision torchaudio \
  --index-url https://download.pytorch.org/whl/cu124 \
  -d wheels
  #transformer
pip download transformers datasets accelerate sentencepiece \
  -d wheels
  #其他
  pip3 download matplotlib scipy opencv-python tensorboard \
  --platform manylinux2014_x86_64 \
  --only-binary=:all: \
  --python-version 3.12

```
- [x] scp到工作站 ✅ 2025-11-17
```bash
 scp -r /mnt/d/wait_what/Downloads/wheels linmiaoju@172.31.179.162:/data/linmiaoju/.cache/pip/
 用XFTP
```
>需设置目录属主[[scp]]

- [x] 在容器里装一个miniconda将工作站的复制一份到容器内 ✅ 2025-11-17
`docker cp Miniconda3-py312_24.5.0-0-Linux-x86_64.sh jepa:/opt/conda`
- [x] `bash Miniconda3-py312_24.5.0-0-Linux-x86_64.sh -u -p /opt/conda`将miniconda安装到指定目录，在/opt/conda/下有文件的情况下更新 ✅ 2025-11-17
- [x] 在容器内安装库 ✅ 2025-11-18
```bash
#pip
 scp -r /mnt/d/wait_what/Downloads/pip_wheels/* linmiaoju@172.31.179.162:/data/linmiaoju/.cache/pip/#`/*` 会把 `pip_wheels` 里的所有文件复制到目标目录，而不是把整个文件夹复制过去。
 #在确保了python安装的第三方库会被安装到数据盘进行安装，进入wheel目录：/root/.cache/pip/wheels
	pip install --no-index --find-links=. torch-2.9.1-cp312-cp312-manylinux_2_28_x86_64.whl

 
```
>注意权限
>sudo chown -R linmiaoju:linmiaoju /data/linmiaoju/.cache/pip/
sudo chmod -R 755 /data/linmiaoju/.cache/pip/

- [x] 修改配置文件 ✅ 2025-11-18
```bash
mkdir -p /workspace/jepa/experiment_logs #添加日志文件目录

```
- [x] 本地下载测试的数据集 ✅ 2025-11-18
`wget https://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz`