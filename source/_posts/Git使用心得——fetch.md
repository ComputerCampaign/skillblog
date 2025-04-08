---
title: Git使用心得——fetch
date: 2025-04-08 14:27:32
tags: git
index_img: /img/git/git_logo.jpeg
---

# Git使用心得——fetch

### git fetch

git fetch origin 是 Git 版本控制系统中一个常用命令，用于从远程仓库（默认命名为 origin）同步最新改动到本地，但不会自动合并或修改你的本地代码。

***

### 具体作用

*   1 拉取远程更新
    将远程仓库（如 GitHub/GitLab）的所有分支、标签和提交记录下载到本地，存储在本地的 .git 目录中（例如 origin/main 分支）。

*   2 不修改本地工作区
    你的当前分支代码和文件内容保持不变，仅更新 Git 的本地仓库元数据。

***

### 典型使用场景：

*   查看他人提交
    想先检查远程仓库是否有新内容，再决定是否合并到本地分支。

    git fetch origin
    git diff main origin/main  # 查看本地 main 和远程 origin/main 的差异

*   手动合并更新
    安全地将远程更新合并到本地（需后续操作）：

    git fetch origin
    git merge origin/main      # 将远程 origin/main 合并到当前分支
    或
    git rebase origin/main     # 以变基方式合并

***

### 与 git pull 的区别

*   git pull = git fetch + git merge
    git pull 会直接拉取并合并代码，可能引发冲突；而 git fetch 更安全可控。

***

### 示例流程：

    # 1. 拉取远程最新状态（仅同步元数据）
    git fetch origin

    # 2. 查看远程分支更新（比如 origin/main）
    git checkout origin/main

    # 3. 比较差异后决定合并
    git checkout main
    git merge origin/main

通过 git fetch，你可以灵活管理远程和本地代码的同步，避免直接操作带来的风险。
