目标：工作站不能连接外网 github，多人在不同本地工作站如何通过 git 同步代码协同开发，通过 gitlab？
待办事项：
- [x] 本地下载镜像：`docker pull gitlab/gitlab-ce` ✅ 2025-11-13
- [x] 保存为.tar文件：`docker save -o gitlab-ce-latest.tar gitlab/gitlab-ce:latest ✅ 2025-11-13
- [x] 搬到4090：`scp gitlab-ce-latest.tar linmiaoju@172.31.179.162:8289:/data/linmiaoju/docker_images/` ✅ 2025-11-13
- [x] 4090导入镜像：`docker load -i /data/linmiaoju/docker_images/gitlab-ce-latest.tar` ✅ 2025-11-13
- [x] 启动容器 ✅ 2025-11-13
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
>注意[[共享内存]]设置还有[[端口映射]]
>GitLab 首次启动需要较长时间（通常 2-10 分钟）进行初始化。！！！！！


- [x] root登录gitlab，网址：http://172.31.179.162:8929,用户名：root 密码获取：`docker exec -it gitlab cat /etc/gitlab/initial_root_password` ✅ 2025-11-15
- [x] SSH[[公私密钥生成]] ✅ 2025-11-15
>由于 docker run的时候执行的是 -p 2224:22 ，所以 SSH到gitlab容器的时候要用的是2224（外部），22是容器内部

- [x] 本地电脑下载jepa代码，scp到4090 ✅ 2025-11-15 
用Xftp8
- [x] 在gitlab Web页面创建仓库，不选择创建Readme.md ✅ 2025-11-15
- [x] 初始化工作站仓库、（检查分支名称，防止和gitlab的不一致）暂存、创建远程分支、推送到远程仓库 ✅ 2025-11-15
```bash
cd /home/linmiaoju/jepa 
git init 
git branch -M main #修改分支名称
git add . 
git commit -m "Initial commit: add I-JEPA source code" 
git remote add origin git@gitlab.local:root/jepa.git
git push -u origin main
```
- [x] 修复端口不绑定的问题，明天来试试——11.15 ✅ 2025-11-16
```bash
mv docker-compose-linux-x86_64.docker-compose-linux-x86_64 docker-compose
scp docker-compose "linmiaoju@172.31.179.162:/data/linmiaoju/gitlab/bin/docker-compose"
sudo chmod +x /data/linmiaoju/gitlab/bin/docker-compose sudo ln -sf /data/linmiaoju/gitlab/bin/docker-compose /usr/local/bin/docker-compose
 cd /data/linmiaoju/gitlab 
docker-compose up -d 
```
>docker-compose.yml在/data/linmiaoju/gitlab
- [x] 允许其他电脑访问： ✅ 2025-11-16
把这一行修改：
```bash
external_url 'http://172.31.179.162:8929'
```
在/data/linmiaoju/gitlab/docker-compose.yml

