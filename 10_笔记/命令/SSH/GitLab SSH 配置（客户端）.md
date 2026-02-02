---
类型: 命令
标签:
  - SSH
  - Gitlab
---

- 命令
```bash
# 生成 SSH 密钥。
ssh-keygen -t rsa -b 4096 -C "your-email@lab.com"
```

```bash
# 查看并复制公钥内容（用于添加到 GitLab）。
cat ~/.ssh/id_rsa.pub
```

```bash
# 写入 SSH 配置，给 GitLab 设置别名与端口。
printf "Host gitlab\n  HostName 172.31.179.162\n  User git\n  Port 2224\n  IdentityFile ~/.ssh/id_rsa\n" >> ~/.ssh/config
```

```bash
# 收紧 SSH 配置文件权限。
chmod 600 ~/.ssh/config
```

- 参数说明
```bash
# -t rsa: 密钥类型
# -b 4096: 密钥长度
# -C: 备注邮箱
# Port 2224: GitLab 容器对外端口
```
- 解释：把 22 当作 GitLab 容器端口，实际对外是 2224。


