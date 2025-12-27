> 适用于日常开发：初始化项目 → 提交代码 → 推送到内网 GitLab
## 1. 初始化本地仓库

```bash
cd /path/to/your/project
git init
```

> ✅ 验证是否为 Git 仓库：`git status`

---

## 2. 创建 `.gitignore`（推荐）

```bash
cat > .gitignore << EOF
# 依赖
node_modules/
venv/
__pycache__/
*.pyc

# 构建输出
dist/
build/

# 环境变量
.env
.env.local

# IDE
.idea/
.vscode/

# 日志
*.log
EOF
```

---

## 3. 首次提交

```bash
# 空项目需先创建 README
echo "# myproject" > README.md

git add .
git commit -m "Initial commit"
```

---

## 4. 设置现代分支名

```bash
git branch -M main    # 将默认分支改为 main
```

---

## 5. 连接远程 GitLab 仓库

### ✅ 前提：
- 项目已在 GitLab 创建（如 `root/jepa-ai-research`）
- 你有 **Developer+ 权限**

### 方式一：SSH（推荐，免密）
```bash
git remote add origin git@172.31.179.162:root/jepa-ai-research.git
```

### 方式二：HTTPS（需 Token）
```bash
git remote add origin http://172.31.179.162:8929/root/jepa-ai-research.git
```
> 🔑 推送时密码 = **Personal Access Token (PAT)**

---

## 6. 推送代码

```bash
git push -u origin main
```

---

## 7. Git 配置层级

| 层级 | 命令 | 配置文件 |
|------|------|----------|
| local | `git config --local` | `.git/config` |
| global | `git config --global` | `~/.gitconfig` |
| system | `git config --system` | `/etc/gitconfig` |

> 📌 优先级：**local > global > system**

---

## 8. 常见问题解决

| 问题 | 解决方法 |
|------|--------|
| `Permission denied (publickey)` | 检查 SSH Key 是否添加到 GitLab |
| `Repository not found` | 检查项目路径、端口、权限 |
| `failed to push some refs` | 先 `git pull --rebase origin main` |
| 无法推送 | 确保使用 **PAT** 而非账户密码（HTTPS）|

---

## 9. 实用命令补充

```bash
# 查看远程地址
git remote -v

# 重设 origin
git remote remove origin
git remote add origin <new-url>

# 删除远程分支
git push origin --delete "branch-name"

# 清理本地过期引用
git remote prune origin

# 强制推送（慎用！）
git push -f origin main
```

---

✅ **适用场景**：日常开发、团队协作、CI/CD 集成前的代码管理。

---

你可以将这两份笔记分别保存为：

- `gitlab-admin-guide.md`
- `git-developer-workflow.md`

需要我为你导出纯 Markdown 文本以便复制粘贴吗？😊