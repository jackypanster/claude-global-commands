---
allowed-tools: Bash(git status), Bash(git log), Bash(git diff), Bash(npm test), Bash(npm run lint)
description: Prepare branch for pull request with comprehensive checks
---

## 分支状态检查
- 当前分支：!`git branch --show-current`
- 与主分支差异：!`git log origin/main..HEAD --oneline`
- 工作区状态：!`git status`

PR 准备检查清单：

1. **代码质量检查**
   - 运行所有测试
   - 执行代码检查 (lint)
   - 检查代码覆盖率

2. **提交历史整理**
   - 分析提交信息质量
   - 建议是否需要 squash
   - 检查提交原子性

3. **变更分析**
   - 总结主要功能变更
   - 识别潜在的破坏性变更
   - 生成变更日志

4. **PR 描述模板**
   - 生成 PR 标题和描述
   - 列出测试步骤
   - 标注相关 issue

目标分支：$ARGUMENTS
