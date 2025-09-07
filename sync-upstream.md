---
allowed-tools: Bash(git remote), Bash(git fetch), Bash(git merge), Bash(git rebase), Bash(git push)
description: Sync with upstream repository and resolve conflicts
---

## 同步状态
- 远程仓库：!`git remote -v`
- 本地分支：!`git branch --show-current`
- 上游差异：!`git log HEAD..origin/main --oneline | head -5`

上游同步流程：

1. **获取更新**
   - 拉取上游最新变更
   - 分析更新内容
   - 评估冲突风险

2. **同步策略**
   - 选择合并或变基
   - 处理可能的冲突
   - 保持提交历史整洁

3. **后续操作**
   - 推送到远程分支
   - 更新依赖分支
   - 通知团队成员

上游分支：$ARGUMENTS
