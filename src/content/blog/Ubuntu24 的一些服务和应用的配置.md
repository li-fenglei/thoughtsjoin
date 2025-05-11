---
title: "Ubuntu24 的一些服务和应用的配置"
categories: Linux
tags: ["Ubuntu"]
id: "Ubuntu-service-and-app-config"
date: 2025-03-02 18:18:18
cover: "https://i0.wp.com/uxiaohan.github.io/v2/2025/04/1743737394560.webp"
recommend: true
---

## 设置静态ip
```
cd /etc/netplan/

network:
  version: 2
  ethernets:
    eno1:                   # 替换为你的网卡名称
      dhcp4: no              # 禁用DHCP
      addresses:
        - 192.168.1.10/24   # 静态IP/子网掩码（CIDR格式）
      routes:
        - to: default
          via: 192.168.1.1    # 网关地址
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]  # DNS服务器


sudo netplan apply
```
## ollama
参照：https://github.com/ollama/ollama/blob/main/docs/linux.md



## open-webui

在 Ubuntu 上通过虚拟环境安装 Open WebUI 并设置为开机自启，可以按照以下详细步骤操作：

---

### **1. 创建专用目录和虚拟环境**
建议在 `/opt` 或用户主目录下创建专用目录，便于管理：

```bash
# 创建目录（以 /opt 为例，需 sudo 权限）
sudo mkdir -p /opt/open-webui
sudo chown -R $USER:$USER /opt/open-webui  # 将所有权赋予当前用户
cd /opt/open-webui

# 创建 Python 虚拟环境
python3 -m venv venv
source venv/bin/activate  # 激活虚拟环境
```

---

### **2. 安装 Open WebUI 和依赖**
在虚拟环境中安装：
```bash
pip install --upgrade pip
pip install open-webui

# 如果从源码安装（可选）
git clone https://github.com/open-webui/open-webui.git
cd open-webui
pip install -r requirements.txt
```

---

### **3. 配置 Ollama 后端**
确保 Ollama 已安装并运行：
```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama pull llama3  # 下载默认模型
sudo systemctl enable --now ollama  # 设置 Ollama 开机自启
```

---

### **4. 测试运行**
手动启动 Open WebUI 测试：
```bash
cd /opt/open-webui
source venv/bin/activate
open-webui --port 8080  # 或根据实际命令调整
```
访问 `http://localhost:8080` 确认服务正常。

---

### **5. 设置开机自启（systemd 服务）**
创建 systemd 服务文件：
```bash
sudo vim /etc/systemd/system/open-webui.service
```
写入以下内容（根据路径调整）：
```ini
[Unit]
Description=Open WebUI
After=network.target ollama.service  # 确保在 Ollama 后启动

[Service]
User=$USER  # 替换为你的用户名
WorkingDirectory=/opt/open-webui
ExecStart=/opt/open-webui/venv/bin/open-webui --port 8080  # 确保命令正确
Restart=always
Environment="PATH=/opt/open-webui/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

[Install]
WantedBy=multi-user.target
```

**关键参数说明**：
- `User`: 运行服务的用户（需有目录权限）。
- `ExecStart`: 使用虚拟环境中的绝对路径启动命令。
- `Environment`: 确保包含虚拟环境的 `bin` 目录。

---

### **6. 启用并启动服务**
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now open-webui  # 启用并立即启动
sudo systemctl status open-webui       # 检查状态
```

---

### **7. 日志和故障排查**
- 查看日志：
  ```bash
  journalctl -u open-webui -f  # 实时日志
  ```
- 如果服务失败：
  1. 检查命令路径：`which open-webui`（需在虚拟环境中确认）。
  2. 确保端口未被占用（如 `8080`）。
  3. 检查 Ollama 是否运行：`systemctl status ollama`。

---

### **8. 更新或卸载**
- **更新**：重新拉取代码或 `pip install --upgrade open-webui`。
- **卸载**：
  ```bash
  sudo systemctl stop open-webui
  sudo systemctl disable open-webui
  sudo rm /etc/systemd/system/open-webui.service
  sudo rm -rf /opt/open-webui  # 彻底删除目录
  ```

---

### **注意事项**
1. **虚拟环境必要性**：避免全局 Python 污染，尤其系统有多个 Python 项目时。
2. **资源占用**：Open WebUI 和 Ollama 需要足够内存（建议 8GB+）。
3. **反向代理（可选）**：若需通过域名访问，配置 Nginx/Apache 反向代理到 `8080` 端口。

通过以上步骤，Open WebUI 会在系统启动时自动运行，并通过虚拟环境隔离依赖。