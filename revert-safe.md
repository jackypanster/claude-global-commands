---
allowed-tools: Bash(git log), Bash(git show), Bash(git revert), Bash(git reset), Bash(git stash)
description: Safe revert operations with impact analysis
argument-hint: [commit-hash|HEAD~n]
---

## 回滚目标分析
- 目标提交：!`git show --stat $ARGUMENTS`
- 影响文件：!`git show --name-only $ARGUMENTS`

安全回滚操作：

1. **影响分析**
   - 分析变更影响范围
   - 识别依赖关系
   - 评估回滚风险

2. **回滚策略**
   - 建议回滚方式 (revert vs reset)
   - 考虑历史保留
   - 团队协作影响

3. **验证计划**
   - 测试验证步骤
   - 功能回归检查
   - 部署注意事项

回滚目标：$ARGUMENTS
