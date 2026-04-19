下面是**完整中文版 README（已保留所有代码块 + 命令 + 路径）**，同时把说明全部翻译成中文，适合你直接拿去用或二次修改👇

---

# FORTRESS.SH :: Debian Linux 安全加固配置

**一键安全加固工具，实现企业级安全（DISA STIG + CIS），同时允许你在“安全性 vs 易用性”之间自由选择。适用于从桌面到服务器的不同场景。**

**版本 5.1**
修复：Docker 兼容、浏览器问题、完整配置文件
已测试：Debian 13、Ubuntu 24.04+

---

## ⚠️ 远程服务器用户重要警告

**必须先配置 SSH 密钥，否则你会被锁在服务器外。**

v5.1 虽然加入了防锁死机制，但你仍然需要提前配置好基于密钥的 SSH 登录。

---

## 📢 注意事项

该脚本负责：

* 系统/网络安全加固
* AppArmor（不是 SELinux）
* 审计日志
* 基础安全策略

不会处理：

* 用户组管理
* SELinux
* VFIO / IOMMU

---

## 🚀 快速开始

### 桌面用户

```bash
# 下载并赋权
wget https://raw.githubusercontent.com/captainzero93/security_harden_linux/main/fortress_improved.sh
chmod +x fortress_improved.sh

# 查看执行内容（推荐首次）
sudo ./fortress_improved.sh --explain --dry-run

# 使用默认配置执行
sudo ./fortress_improved.sh

# 按提示操作，完成后重启
```

---

### 服务器用户

```bash
# 第一步：配置 SSH 密钥（必须）
ssh-keygen -t ed25519
ssh-copy-id user@your-server

# 然后执行加固
sudo ./fortress_improved.sh -l high -n
```

---

### 出问题恢复

```bash
/root/fortress_backups_*/
```

---

## 📌 为什么默认 Linux 不安全

默认系统通常：

* 没有启用防火墙
* SSH 暴露公网
* 可无限尝试密码
* 无自动安全更新
* 内核防护弱
* 无入侵检测

👉 这是“易用性优先”的设计，而不是漏洞。

---

## 🔐 脚本功能

### 安全能力

* 启用防火墙（UFW）
* SSH 加固（防爆破）
* 登录失败限制
* 内核安全增强
* 软件包完整性检查
* 强密码策略
* 自动安全更新
* 审计日志
* 启动安全检查
* 删除无用软件

---

### 不影响功能

* 游戏 / Steam
* Discord / Zoom
* 音视频制作
* 开发工具
* Docker（已优化）
* 浏览器（已兼容）

---

## 🖥️ 桌面用户说明

默认使用 **moderate（中等）安全级别**：

👉 几乎无感知变化，但安全性大幅提升

---

## 🚨 远程服务器注意

脚本会：

👉 禁用 SSH 密码登录

必须提前执行：

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
ssh-copy-id username@your-server-ip

# 测试
ssh -i ~/.ssh/id_ed25519 username@your-server-ip
```

---

## 🔍 常见攻击防护

### 暴力破解

👉 解决：

* 禁用密码登录
* 限速连接
* 可选 fail2ban

---

### 内核漏洞

👉 解决：

* ASLR
* 内存保护
* 内核加固

---

### 软件被篡改

👉 解决：

* dpkg 校验
* 审计日志
* AppArmor

---

## 🆕 新功能（v5.1）

新增：

* Docker 自动检测
* 浏览器兼容
* 配置文件支持
* 健康检查脚本

---

## 📦 安装方式

```bash
wget https://raw.githubusercontent.com/captainzero93/security_harden_linux/main/fortress_improved.sh
chmod +x fortress_improved.sh

sudo ./fortress_improved.sh --explain --dry-run
```

---

## 📂 Git 克隆

```bash
git clone https://github.com/captainzero93/security_harden_linux.git
cd security_harden_linux

chmod +x fortress_improved.sh

# 查看代码
less fortress_improved.sh

# 运行
sudo ./fortress_improved.sh
```

---

## 🧰 辅助脚本

* fix_library_permissions.sh（修复库权限）
* PERM_diagnostic.sh（诊断）
* verify_fortress.sh（健康检查）

---

## 🧑‍💻 使用指南

### 基础用法

```bash
# 教学模式
sudo ./fortress_improved.sh --explain

# 预览模式
sudo ./fortress_improved.sh --dry-run

# 默认执行
sudo ./fortress_improved.sh

# 高安全
sudo ./fortress_improved.sh -l high

# 无交互（服务器）
sudo ./fortress_improved.sh -n
```

---

### 模块控制

```bash
# 指定模块
sudo ./fortress_improved.sh -e system_update,ssh_hardening,firewall

# 排除模块
sudo ./fortress_improved.sh -x fail2ban,usb_protection

# 查看模块
sudo ./fortress_improved.sh --list-modules
```

---

### 高级选项

```bash
sudo ./fortress_improved.sh --explain -d -v
sudo ./fortress_improved.sh -l paranoid
sudo ./fortress_improved.sh -c /path/to/config.conf
sudo ./fortress_improved.sh --version
```

---

## 🔍 执行后检查

```bash
sudo ./verify_fortress.sh
cat /root/fortress_report_*.html
tail -100 /var/log/fortress_hardening.log
sudo reboot
```

---

## 📊 监控命令

```bash
sudo ufw status verbose
sudo aa-status
sudo ausearch -m USER_LOGIN -ts recent
dpkg --verify
sudo fail2ban-client status
```

---

## 🔐 安全等级

* low：基础
* moderate：推荐
* high：服务器
* paranoid：极限

---

## 🧱 模块列表

核心：

* system_update
* firewall
* ssh_hardening
* automatic_updates
* sysctl
* audit
* apparmor

可选：

* fail2ban
* usb_protection
* packages

---

## 🔧 紧急恢复

### SSH 锁死

```bash
sudo cp /root/fortress_backups_*/etc/ssh/sshd_config /etc/ssh/sshd_config
sudo systemctl restart sshd
```

---

### 防火墙问题

```bash
sudo ufw allow 8080/tcp
sudo ufw disable
sudo ufw enable
```

---

### 启动失败

```bash
mount -o remount,rw /
cp /root/fortress_backups_*/etc/default/grub /etc/default/grub
update-grub
reboot
```

---

## 🧪 故障排查

### Docker 无网络

```bash
sudo sed -i 's/net.ipv4.ip_forward = 0/net.ipv4.ip_forward = 1/' /etc/sysctl.d/99-fortress.conf
sudo sysctl -p /etc/sysctl.d/99-fortress.conf
```

---

### 浏览器打不开

```bash
sudo sed -i 's/nodev,nosuid,noexec/nodev,nosuid/' /etc/fstab
sudo mount -o remount /dev/shm
```

---

### SSH 无法连接

```bash
sudo systemctl status sshd
sudo ufw allow ssh
sudo sshd -t
```

---

## ⚙️ 配置文件

生成：

```bash
sudo ./fortress_improved.sh --generate-config
```

示例：

```bash
SECURITY_LEVEL="moderate"
ALLOW_DOCKER_FORWARDING=true
ALLOW_BROWSER_SHAREDMEM=true

DISABLE_MODULES="fail2ban,usb_protection"
ENABLE_MODULES="system_update,ssh_hardening,firewall,apparmor"

SSH_PORT=22
FIREWALL_ALLOW_PORTS="80,443"
```

---

## 🤖 自动部署

```bash
#!/bin/bash

SERVERS="server1 server2 server3"

for server in $SERVERS; do
    scp fortress_improved.sh user@$server:/tmp/
    ssh user@$server "sudo /tmp/fortress_improved.sh -l high -n"
done
```

---

## 📦 Ansible

```yaml
- name: Deploy security hardening
  hosts: all
  tasks:
    - name: Copy script
      copy:
        src: fortress_improved.sh
        dest: /tmp/fortress_improved.sh
        mode: '0755'
    
    - name: Run hardening
      command: /tmp/fortress_improved.sh -l high -n
      become: yes
```

---

## ⚠️ 免责声明

本脚本：

* 不保证绝对安全
* 不替代安全审计
* 不防 0day
* 不保证合规

👉 使用风险自负

---

## 📌 一句话总结

👉 这是一个：

**适合普通人 + 服务器的一键 Linux 安全加固脚本（不折腾、不影响使用）**

---

如果你下一步想玩更骚一点的，我可以帮你👇

👉 给你改成**交互式菜单一键安全脚本（比 BlueSkyXN 更强）**
👉 或直接帮你做一套**服务器初始化 + 安全 + Docker + 面板 全自动部署脚本**
