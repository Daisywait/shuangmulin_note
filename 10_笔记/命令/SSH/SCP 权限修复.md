---
类型: 命令
标签:
  - 文件权限
---

- 命令
```bash
# 查看目录属主与权限。
ls -ld /data/linmiaoju/.cache /data/linmiaoju/.cache/pip
```

```bash
# 修正目录属主。
sudo chown -R linmiaoju:linmiaoju /data/linmiaoju/.cache/pip
```

```bash
# 修正目录权限（如需）。
sudo chmod -R 755 /data/linmiaoju/.cache/pip
```

- 参数说明
```bash
# chown -R: 递归修改属主
# chmod -R: 递归修改权限
```
- 解释：SCP/手动拷贝后目录属主不一致导致权限问题。


