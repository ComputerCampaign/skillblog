---
title: Git使用心得——配置本地用户名
date: 2025-04-08 14:30:38
tags: git
index_img: /img/git/git_logo.jpeg
---

# Git使用心得——配置本地用户名

## 问题背景

当你在两台电脑上使用**同一个GitHub账号**但**不同的SSH密钥**时，可能会遇到以下问题：

*   1 提交记录显示为不同提交者

*   2 贡献统计不归属到目标账号

*   3 GitHub Actions等自动化功能鉴权失败

## 核心原理

### 1. SSH密钥与邮箱的分工

| 组件      | 作用域    | 功能描述       |
| ------- | ------ | ---------- |
| SSH密钥   | 仓库操作权限 | 代码推送/拉取的鉴权 |
| Git邮箱配置 | 提交记录归属 | 决定提交者身份显示  |

### 2. GitHub账号关联规则

*   邮箱匹配：提交记录中的邮箱必须与GitHub账号的**已验证邮箱**一致
*   密钥绑定：SSH密钥需添加到目标账号的`Settings → SSH and GPG keys`

## 完整配置方案

### 步骤1：统一邮箱配置

在两台电脑上配置**完全相同的邮箱**（必须与GitHub账号已验证邮箱一致）

```bash
# 全局配置（推荐）
git config --global user.email "your_verified_email@example.com"
git config --global user.name "YourGitHubUsername"

# 验证配置
git config --global --get user.email
```

### 步骤2：独立SSH密钥配置

为每台电脑生成独立的SSH密钥：

```bash
# 电脑A
ssh-keygen -t ed25519 -C "your_verified_email@example.com" -f ~/.ssh/github_pcA

# 电脑B 
ssh-keygen -t ed25519 -C "your_verified_email@example.com" -f ~/.ssh/github_pcB
```

### 步骤3：配置SSH代理

编辑 \~/.ssh/config 文件：

```bash
# 电脑A配置
Host github.com-pcA
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_pcA
    IdentitiesOnly yes

# 电脑B配置  
Host github.com-pcB
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_pcB
    IdentitiesOnly yes
```

### 步骤4：修改仓库远程地址

```bash
# 替换远程地址格式
git remote set-url origin git@github.com-pcA:username/repo.git
# 或
git remote set-url origin git@github.com-pcB:username/repo.git
```

## 验证配置

### 1. SSH连接测试

```bash
# 电脑A
ssh -T git@github.com-pcA
# 应显示：Hi YourUsername! You've successfully authenticated...

# 电脑B
ssh -T git@github.com-pcB 
# 应显示：Hi YourUsername! You've successfully authenticated...
```

### 2. 提交记录验证

```bash
# 创建测试提交
echo "test" >> test.txt
git add . && git commit -m "commit test"
git push

# 查看GitHub提交记录，应显示统一用户名和邮箱
```

