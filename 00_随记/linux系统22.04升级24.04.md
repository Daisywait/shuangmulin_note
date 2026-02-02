# Ubuntu 22.04 升级到 24.04 LTS（Desktop）

> 仅适用于 Ubuntu Desktop（LTS 升级）。建议先做备份/快照，升级过程不可逆。

## 1. 升级前准备

1) 备份 / 快照（强烈建议）

- 重要数据：同步到外置硬盘 / NAS / 云盘  
- 桌面环境可用 Timeshift 创建系统快照（GUI 操作更直观）  

确认备份可用后再进行升级。

备份清单（按需选择）：
- 用户数据：`/home`（文档、下载、桌面、项目）  
- 配置文件：`/etc`（系统服务、网络、ssh、软件源）  
- 应用数据：`/var`（数据库、缓存、容器数据等）  
- 自定义脚本/服务：`/usr/local`  

外接硬盘备份（示例）：
1) 插入移动硬盘后查看设备  
```bash
lsblk
```
2) 创建挂载点并挂载（示例设备为 `/dev/sdb1`）  
```bash
sudo mkdir -p /mnt/backup
sudo mount /dev/sdb1 /mnt/backup
```
3) 备份数据（示例：备份整个 `/home`）  
```bash
sudo rsync -aHAX --info=progress2 /home/ /mnt/backup/home/
```
4) 完成后安全卸载  
```bash
sudo umount /mnt/backup
```

2) 确认当前版本  
```bash
lsb_release -a
```

3) 先把系统更新到最新  
```bash
sudo apt update
sudo apt upgrade
```
必要时重启：  
```bash
sudo systemctl reboot
```

4) 可选：清理无用包（降低冲突）  
```bash
sudo apt autoremove
sudo apt clean
```

## 2. 图形界面升级（推荐）

1) 打开 **Software Updater**  
2) 如果提示有新版本，点击 **Upgrade**  
3) 按提示完成升级  
4) 升级完成后重启系统

## 3. 升级过程注意事项

- 升级过程中会提示是否替换配置文件：  
  - 关键配置建议选择保留旧配置，升级后再手动合并。
- 升级完成后建议再次更新系统包。

## 4. 升级后检查

1) 确认版本  
```bash
lsb_release -a
```

2) 检查第三方源（PPA）是否被禁用  
```bash
ls -la /etc/apt/sources.list.d/
```
需要的 PPA 需手动重新启用并确认 24.04 版本支持。

3) 再次更新  
```bash
sudo apt update
sudo apt upgrade
```

## 参考

- Ubuntu 官方升级文档（Desktop）
- Ubuntu 社区升级说明（NobleUpgrades）
