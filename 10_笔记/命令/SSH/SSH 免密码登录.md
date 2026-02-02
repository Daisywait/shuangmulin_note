---
类型: 命令
标签:
  - SSH
---

- 命令
```bash
# 生成 SSH 密钥（Linux 侧）。
ssh-keygen -t ed25519 -C "your-email@lab.com"
```

```bash
# 手动追加公钥到远程
cat ~/.ssh/id_ed25519.pub 
```

- 参数说明
```bash
# -t ed25519: 密钥类型
# -C: 备注邮箱
```
- 解释：使用公钥认证避免重复输入密码。


