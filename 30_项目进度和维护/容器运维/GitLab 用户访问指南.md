> **版本**：v1.0 | **更新日期**：2025-11-15 **管理员**：linmiaoju（root） **GitLab 地址**：http://172.31.179.162 **SSH 端口**：2224

---

## 1. 访问 GitLab 网页

1. 打开浏览器，输入：

    http://172.31.179.162:8929/
    
2. 使用 **管理员提供的用户名和初始密码** 登录

> 首次登录必须修改密码！

---

## 2. 获取账号（由管理员创建）

|字段|示例|
|---|---|
|用户名|wang-wu|
|初始密码|Lab@2025_Temp!|
|邮箱|wangwu@lab.com|

联系 **linmiaoju** 获取

---

## 3. 配置 SSH 访问（推荐，免密码）

### 步骤 A：生成 SSH 密钥（在你的电脑上）

bash

```bash
ssh-keygen -t rsa -b 4096 -C "your-email@lab.com"
```

- 回车一路默认（路径：~/.ssh/id_rsa）

### 步骤 B：复制公钥

bash

```
cat ~/.ssh/id_rsa.pub
```

**复制整行内容**

### 步骤 C：添加到 GitLab

1. 登录 GitLab → 头像 → **Preferences** → **SSH Keys**
2. 粘贴公钥 → Title 会自动识别填入 → **Add key**

---

## 4. 配置 SSH Config（必须！因为端口是 2224）

### 创建/编辑 ~/.ssh/config 文件：
```
Host gitlab
    HostName 172.31.179.162
    User git
    Port 2224
    IdentityFile ~/.ssh/id_rsa
```

### 设置权限（重要！）

bash

```
chmod 600 ~/.ssh/config
```

---

## 6. 克隆项目（使用别名 gitlab）

bash

```
git clone git@gitlab.local:root/jepa.git
```

> 成功输出：
> 
> text
> 
> ```
> Cloning into 'ijepa-4090'...
> remote: Enumerating objects: ...
> ```

---

