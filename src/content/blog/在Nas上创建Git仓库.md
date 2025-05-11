---
title: 在Nas上创建Git仓库
categories: Linux
tags:
  - Nas
  - Git
id: nas-git
# cover: https://i0.wp.com/uxiaohan.github.io/v2/2024/12/1733449691.png
date: 2024-12-06 09:37:11
---

在NAS上通过SSH登录后，初始化一个Git仓库并将本地已有项目上传到该仓库的具体步骤如下：

---

### **前提条件**
- 你已经在NAS上安装了Git。
- 你已经在本地电脑上安装了Git。
- 你已经在本地电脑上创建了一个Git项目。
- 设置全局默认分支为 main：`git config --global init.defaultBranch main`

### **1. 通过SSH登录到NAS**
1. 打开终端（或使用SSH客户端如PuTTY）。
2. 使用以下命令连接到NAS：
   ```bash
   ssh <用户名>@<NAS的IP地址>
   ```
   例如：
   ```bash
   ssh admin@192.168.1.100
   ```
3. 输入密码登录。

---

### **2. 初始化Git仓库**
1. 在NAS上选择一个目录作为Git仓库的存储位置。例如：
   ```bash
   cd /volume1/git/
   ```
2. 创建一个新的Git仓库：
   ```bash
   mkdir my-repo.git
   cd my-repo.git
   git init --bare
   ```
   - `--bare`选项表示创建一个裸仓库（没有工作目录），适合作为远程仓库。

---

### **3. 在本地已有项目中初始化Git**
1. 打开本地项目的根目录。
2. 初始化Git仓库：
   ```bash
   cd /path/to/your/local/project
   git init
   ```
3. 添加所有文件到暂存区：
   ```bash
   git add .
   ```
4. 提交更改：
   ```bash
   git commit -m "Initial commit"
   ```

---

### **4. 将本地仓库连接到NAS上的远程仓库**
1. 在本地项目中，添加NAS上的Git仓库作为远程仓库：
   ```bash
   git remote add origin ssh://<用户名>@<NAS的IP地址>/volume1/git/my-repo.git
   ```
   例如：
   ```bash
   git remote add origin ssh://admin@192.168.1.100/volume1/git/my-repo.git
   ```
2. 验证远程仓库是否添加成功：
   ```bash
   git remote -v
   ```
   输出示例：
   ```
   origin  ssh://admin@192.168.1.100/volume1/git/my-repo.git (fetch)
   origin  ssh://admin@192.168.1.100/volume1/git/my-repo.git (push)
   ```

---

### **5. 推送本地项目到NAS上的远程仓库**
1. 将本地代码推送到远程仓库：
   ```bash
   git push -u origin main
   ```
   - 如果本地分支名称不是`main`，请替换为实际分支名称（如`master`）。
   - `-u`选项将本地分支与远程分支关联，后续可以直接使用`git push`。

2. 如果遇到错误提示`main`分支不存在，可以尝试以下命令：
   ```bash
   git branch -M main
   git push -u origin main
   ```

---

### **6. 验证上传结果**
1. 在NAS上，进入Git仓库目录，检查文件是否已上传：
   ```bash
   cd /volume1/git/my-repo.git
   git log
   ```
2. 如果能看到提交记录，说明上传成功。

---

### **7. 克隆远程仓库（可选）**
如果需要从NAS上的远程仓库克隆项目到其他机器，可以使用以下命令：
```bash
git clone ssh://<用户名>@<NAS的IP地址>/volume1/git/my-repo.git
```
例如：
```bash
git clone ssh://admin@192.168.1.100/volume1/git/my-repo.git
```

---

### **总结**
通过以上步骤，你可以在NAS上初始化一个Git仓库，并将本地已有项目上传到该仓库。具体步骤如下：
1. 通过SSH登录到NAS。
2. 在NAS上初始化一个裸仓库。
3. 在本地项目中初始化Git并提交代码。
4. 将本地仓库连接到NAS上的远程仓库。
5. 推送本地代码到远程仓库。
6. 验证上传结果。

如果有任何问题，请检查SSH连接、权限设置和Git配置。