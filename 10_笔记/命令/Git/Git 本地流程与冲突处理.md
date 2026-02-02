---
类型: 命令
标签:
  - Git本地
---

- 命令
```bash
# 进入项目目录并初始化仓库
cd /path/to/your/project

git init
```

```bash
# 生成 .gitignore（示例）
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

```bash
# 首次提交（空项目先建 README）
echo "# myproject" > README.md

git add .

git commit -m "Initial commit"
```

```bash
# 将默认分支改为 main
git branch -M main
```

```bash
# 分支偏离时推荐的拉取方式（保持线性历史）
git pull --rebase
```

```bash
# 分支偏离时使用合并方式拉取
git pull --no-rebase
```

```bash
# 解决 rebase 冲突后继续
git add <CONFLICT_FILE>

git rebase --continue
```

```bash
# 跳过当前冲突提交（谨慎）
git rebase --skip
```

```bash
# 放弃本次 rebase
git rebase --abort
```

```bash
# 接受远程版本（冲突解决）
git checkout --theirs <FILE>
```

```bash
# 接受本地版本（冲突解决）
git checkout --ours <FILE>
```

- 参数说明
```bash
# -M: 重命名分支（即使已存在）
# <CONFLICT_FILE>: 冲突文件
# <FILE>: 冲突文件
# --rebase: 变基拉取
# --no-rebase: 合并拉取
```

- 解释：分支偏离需要明确选择 rebase 或 merge；冲突解决应优先确认文件取舍。

- 链接
  - [[10_Notes/常用终端命令/Git/Git 远程协作与撤销（通用）]]




