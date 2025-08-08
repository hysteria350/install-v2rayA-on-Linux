# Linux 平台 v2rayA 安装与配置教程（2025 优化版）

本文面向掌握基本命令行使用能力的用户，涵盖 v2rayA 的多种安装方式、内核选择、服务管理与代理部署。全面讲解在 Debian 和 RedHat 系统上安装 v2rayA（含 v2ray 或 Xray 内核）、配置代理模式、设置服务、自启及常见故障处理，适合中高级用户。

## 为何选择 v2rayA？

v2rayA 是基于 V2Ray 核心的 Web GUI 客户端，兼容 VMess、VLESS、Shadowsocks、Trojan 等多种协议，适用于 NAS、路由器、服务器等 Linux 平台。虽然项目活跃度有所下降且不支持最新协议（例如 Hysteria 2），但对于需要图形化管理的用户仍是一款轻量、高效的选择。

---

## 安装方式对比

| 方法 | 优点 | 备注 |
|------|------|------|
| **Snap 安装** | 快速、一键安装，无需额外核心安装 | 支持多发行版 |
| **官方脚本安装** | 可选 v2ray 或 Xray 内核，稳定可靠 | 使用 `curl` 或 `wget` 方式安装 |
| **手动安装** | 控制精细，可单独管理内核和客户端版本 | 适用于高级用户和自定义需求|

### Snap 安装

```bash
sudo snap install v2raya
```

### 官方安装脚本（含内核）

```bash

sudo sh -c "$(wget -qO- https://hubmirror.v2raya.org/v2raya-installer/raw/main/installer.sh) @ --with-v2ray"
#  或使用 Xray 内核
# sudo ... @ --with-xray

```

### 手动安装（以 v2ray 为例）

```bash

# 安装核心
wget https://github.com/v2fly/v2ray-core/releases/latest/download/v2ray-linux-64.zip -P /tmp
unzip /tmp/v2ray-linux-64.zip -d /tmp/v2raya_core
sudo install -Dm755 /tmp/v2raya_core/v2ray /usr/local/bin/v2ray
sudo install -Dm644 /tmp/v2raya_core/*.dat /usr/local/share/v2ray/
# 安装 v2rayA
VERSION=$(wget -qO- https://apt.v2raya.org/dists/v2raya/main/binary-amd64/Packages | grep -A 1 'Package: v2raya' | grep Version | awk '{print $2}')
wget https://github.com/v2rayA/v2rayA/releases/download/v$VERSION/v2raya_linux_x64_$VERSION -P /tmp
sudo install -Dm755 /tmp/v2raya_linux_x64_$VERSION /usr/local/bin/v2raya

```

### 服务启动并设置为开机自启动

```bash

sudo systemctl enable v2raya
sudo systemctl start v2raya

```

检查状态：

```bash
sudo systemctl status v2raya
```

Web UI 默认地址 http://localhost:2017，首次访问需创建管理员帐号；忘记密码可使用：

```bash

sudo v2raya --reset-password

```

### Web UI 登录与节点配置

* 打开浏览器访问 http://localhost:2017

* 创建管理员账户（首次）

* 导入订阅链接：

订阅链接

手动添加（填写协议、地址、端口、UUID/token）

扫码或批量导入

配置完成后点击连接按钮启动代理

