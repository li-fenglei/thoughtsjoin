---
title: MacOS安装配置Anaconda
categories: Share
tags:
  - Anaconda
id: Mac-Anaconda
# cover: "https://i0.wp.com/uxiaohan.github.io/v2/2025/02/1739241633608.webp"
date: 2024-11-21 15:32:51
recommend: true
---

1. anaconda 安装
2. 验证安装是否成功
```
conda --version
```
3. 更改镜像源
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```
4. 查看镜像源
```
conda config --get channels
```
5. 创建和管理虚拟环境
```
# 创建虚拟环境
conda create -n myenv python=3.12
# 查看已创建的虚拟环境
conda env list
# 激活和退出虚拟环境
conda activate myenv
conda deactivate
# 删除虚拟环境
conda remove -n myenv --all
```