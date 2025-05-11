---
title: 开源去水印工具IOPaint的部署
date: 2024-09-04 15:59:05
categories: Linux
tags:
  - 去水印
  - IOPaint
  - AI

id: "IOPaint-deploy"
# cover: "https://i0.wp.com/uxiaohan.github.io/v2/2024/09/1725437035.webp"
recommend: true
---

如果您的 `iopaint` 是在虚拟环境（如 `myenv`）中安装和运行的，以下是 **配置开机自启和后台运行** 的完整方案（基于 `systemd`，推荐生产环境使用）：

---

### **步骤 1：确认虚拟环境路径**
在 `myenv` 目录下运行：
```bash
# 查看虚拟环境的绝对路径（关键！）
pwd
# 示例输出：/home/username/iopaint/myenv
```

---

### **步骤 2：创建 Systemd 服务文件**
```bash
sudo vim /etc/systemd/system/iopaint.service
```
写入以下内容（根据实际情况修改）：
```ini
[Unit]
Description=IOpaint Web Service
After=network.target

[Service]
User=ray  # 如 ubuntu
WorkingDirectory=/home/ray/IOPaint  # iopaint 项目目录
Environment="PATH=/home/ray/IOPaint/myenv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"  # 关键：包含虚拟环境路径
ExecStart=/home/ray/IOPaint/myenv/bin/python -m iopaint start --model=migan --device=cpu --port=8080 --host=0.0.0.0
Restart=always
RestartSec=5s

[Install]
WantedBy=multi-user.target
```


Downloading: "https://github.com/Sanster/models/releases/download/migan/migan_traced.pt" to /ray/.cache/torch/hub/checkpoints/migan_traced.pt



#### 关键参数说明：
- `Environment="PATH=..."`：确保包含虚拟环境的 `bin` 目录
- `ExecStart`：必须使用 **虚拟环境中的绝对路径** 调用 `iopaint`
- `WorkingDirectory`：设置为 `iopaint` 的项目目录（可选但推荐）

---

### **步骤 3：启用并启动服务**
```bash
# 重新加载 systemd
sudo systemctl daemon-reload

# 启动服务
sudo systemctl start iopaint

# 设置开机自启
sudo systemctl enable iopaint
```

---

### **步骤 4：验证服务状态**
```bash
# 查看运行状态
sudo systemctl status iopaint

# 查看实时日志
journalctl -u iopaint -f
```

---

### **常见问题解决**

#### 1. 如果报错 `ModuleNotFoundError`：
- 原因：Systemd 未正确加载虚拟环境
- 解决方案：
  在 `[Service]` 部分添加：
  ```ini
  Environment="PYTHONPATH=/home/username/iopaint/myenv/lib/python3.8/site-packages"
  ```
  （根据实际 Python 版本修改路径）

#### 2. 如果服务启动但无法访问：
- 检查端口监听：
  ```bash
  sudo ss -tulnp | grep 8080
  ```
- 检查防火墙：
  ```bash
  sudo ufw allow 8080/tcp
  ```

#### 3. 更新服务配置后：
```bash
sudo systemctl daemon-reload
sudo systemctl restart iopaint
```

---

### **方案优势**
1. **完全兼容虚拟环境**：所有依赖项隔离
2. **崩溃自动重启**：通过 `Restart=always` 保障服务稳定
3. **集中管理日志**：通过 `journalctl` 查看完整输出
4. **按需扩展**：后续可轻松添加环境变量、资源限制等

> 如果开发阶段需要频繁重启服务，可以配合 `tmux` 使用（先通过 `systemctl stop iopaint` 停止服务后再在 `tmux` 中手动调试）。