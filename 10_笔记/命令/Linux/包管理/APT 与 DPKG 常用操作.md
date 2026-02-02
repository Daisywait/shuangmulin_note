---
类型: 命令
标签:
  - 包管理
---

- 命令
```bash
# 卸载软件包但保留配置
sudo apt remove <PACKAGE>
```

```bash
# 彻底卸载软件包（含配置）
sudo apt purge <PACKAGE>
```

```bash
# 列出已安装的软件包
dpkg --list
```

```bash
# 按关键字过滤已安装的软件包
dpkg --list | grep <KEYWORD>
```

- 参数说明
```bash
# apt remove: 卸载但保留配置
# apt purge: 卸载并删除配置
# <PACKAGE>: 软件包名
# <KEYWORD>: 关键字
```
- 解释：快速定位与管理已安装的软件包。



