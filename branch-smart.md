---
allowed-tools: Bash(git branch), Bash(git checkout), Bash(git switch), Bash(git merge), Bash(git rebase)
description: Smart branch operations and management
argument-hint: [action] [branch-name]
---

## 当前分支状态
- 当前分支：!`git branch --show-current`
- 所有分支：!`git branch -a`
- 分支关系：!`git log --oneline --graph -10`

智能分支操作：

根据操作类型 "$1" 和分支名 "$2"：
- **create**: 创建新功能分支
- **switch**: 切换到指定分支
- **merge**: 合并指定分支到当前分支
- **delete**: 删除本地/远程分支
- **sync**: 同步远程分支状态

操作：$1
分支：$2
