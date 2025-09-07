---
allowed-tools: Bash(git status), Bash(git diff), Bash(git add), Bash(git commit), Bash(git log --oneline -5)
description: Analyze changes and create smart commit with proper message
---

## 当前状态
- Git 状态：!`git status --porcelain`
- 暂存区差异：!`git diff --cached`
- 工作区差异：!`git diff`
- 最近提交：!`git log --oneline -5`

分析代码变更并生成合适的提交信息：

1. 分析修改的文件和内容
2. 识别变更类型（feat/fix/docs/style/refactor/test/chore）
3. 生成符合 Conventional Commits 规范的消息
4. 检查是否有遗漏的文件需要添加
5. 执行提交

附加说明：$ARGUMENTS
