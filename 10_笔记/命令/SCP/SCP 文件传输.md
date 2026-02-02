---
类型: 命令
标签:
  - SSH
  - 文件传输
---

- 前置网络条件
  - 两台主机网络互通（能 ping 通或端口可达）。
  - 目标主机 SSH 端口可访问（默认 22，若改端口需显式指定）。

- 系统条件
  - 两端都安装并启用 SSH 服务（常见为 `sshd`）。
  - 若未安装/未启用 SSH：
    - 安装（Debian/Ubuntu）：`sudo apt update && sudo apt install -y openssh-server`
    - 启用并开机自启：`sudo systemctl enable --now ssh`
  - 传输用户对目标路径有读/写权限。

- 命令
```bash
# 从本机传文件到远程
scp /path/to/file user@host:/remote/path/
```

```bash
# 从远程拉取文件到本机
scp user@host:/remote/path/file /local/path/
```

```bash
# 递归传输目录
scp -r /local/dir user@host:/remote/dir/
```

```bash
# 指定 SSH 端口（注意大写）
scp -P 2224 /path/to/file user@host:/remote/path/
```

```bash
# 限速（单位 Kbit/s）
scp -l 10240 /path/to/file user@host:/remote/path/
```

```bash
# 使用指定私钥
scp -i ~/.ssh/id_rsa /path/to/file user@host:/remote/path/
```

- 参数说明
```bash
# -r: 递归传输目录
# -P: 指定 SSH 端口
# -l: 限速（Kbit/s）
# -i: 指定私钥文件
```

- 解释：scp 基于 SSH 传输，前提是网络可达且 SSH 服务可用。

- 链接
  - [[GitLab SSH 配置（客户端）]]



