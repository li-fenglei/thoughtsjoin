---
title: 把本地已有的项目推送到新创建的GitHub仓库
date: 2024-09-22 20:20:13
categories: Linux
tags:
  - github

id: "local-repo-2-github"
# cover: "https://i0.wp.com/uxiaohan.github.io/v2/2024/09/1727007937.webp"
recommend: true
---

若要把本地已有的项目推送到新创建的 GitHub 仓库（仓库里只有一个 `license` 文件），可按以下步骤操作：

### 1. 配置 Git 用户名和邮箱
在操作之前，要保证 Git 配置了你的用户名和邮箱，这些信息会关联到你在 GitHub 上的提交记录。
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### 2. 初始化本地项目为 Git 仓库（如果还未初始化）
进入本地项目的根目录，执行以下命令来初始化 Git 仓库：
```bash
cd /path/to/your/local/project
git init
```

### 3. 添加远程仓库地址
从 GitHub 仓库页面复制仓库的 URL（可选择 HTTPS 或 SSH 方式），然后在本地项目中添加该远程仓库地址：
- **使用 HTTPS 方式**：
```bash
git remote add origin https://github.com/yourusername/your-repository.git
```
- **使用 SSH 方式**：
```bash
git remote add origin git@github.com:yourusername/your-repository.git
```

### 4. 拉取远程仓库的 `license` 文件
由于远程仓库已有一个 `license` 文件，为避免推送时出现冲突，需要先拉取该文件并合并到本地仓库：
```bash
git pull origin main --allow-unrelated-histories
```
这里的 `--allow-unrelated-histories` 选项用于合并两个没有共同历史的仓库。

### 5. 添加本地项目文件到暂存区
将本地项目中的所有文件添加到 Git 的暂存区：
```bash
git add .
```

### 6. 提交本地项目文件
为暂存区的文件创建一个提交，并添加提交说明：
```bash
git commit -m "Initial commit of local project"
```

### 7. 推送本地项目到远程仓库
把本地的提交推送到 GitHub 仓库的 `main` 分支（如果远程仓库默认分支是 `master`，则相应修改）：
```bash
git push -u origin main
```
`-u` 选项会设置本地分支跟踪远程分支，以后使用 `git push` 时就无需再指定远程仓库和分支。

### 8. 验证推送结果
打开 GitHub 仓库页面，检查是否已成功推送本地项目文件。

通过以上步骤，你就能将本地已有的项目推送到新创建的 GitHub 仓库。 