---
类型: 命令
标签:
  - Git远程
---

- 命令
```bash
# 绑定远程（SSH）
git remote add origin git@<host>:<group>/<repo>.git
```

```bash
# 绑定远程（HTTPS）
git remote add origin https://<host>/<group>/<repo>.git
```

```bash
# 首次推送
git push -u origin main
```

```bash
# 查看远程地址
git remote -v
```

```bash
# 重设远程 origin
git remote remove origin

git remote add origin <new-url>
```

```bash
# 删除远程分支
git push origin --delete "branch-name"
```

```bash
# 清理远程过期引用
git remote prune origin
```

```bash
# 强制推送（慎用）
git push -f origin main
```

```bash
# 安全撤销已推送提交（生成一次反向提交）
git revert <COMMIT_ID>

git push
```

- 参数说明
```bash
# -u: 建立本地分支与远程分支跟踪关系
# -f: 强制推送
# <new-url>: 新的远程仓库地址
# <COMMIT_ID>: 要撤销的提交 ID
# <host>: 远程主机地址（GitHub/GitLab/自建）
# <group>/<repo>: 组织/仓库名
```

- 解释：revert 是最安全的“撤销已推送提交”方式；HTTPS 推送通常需要 Token/PAT。

- 链接
  - [[GitLab SSH 配置（客户端）]]
  -[[shuangmulin/10_笔记/命令/Docker/GitLab 部署]]]
  - [[Git 本地流程与冲突处理]]



