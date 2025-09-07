---
allowed-tools: Bash(git log), Bash(git tag), Bash(git diff), Bash(npm version), Bash(npm run build)
description: Prepare for release with version bump and changelog
argument-hint: [version-type]
---

## 发布状态
- 当前版本：!`git describe --tags --abbrev=0`
- 自上次发布的提交：!`git log $(git describe --tags --abbrev=0)..HEAD --oneline`
- 包版本：!`node -p "require('./package.json').version"`

发布准备流程：

1. **版本分析**
   - 分析变更类型 (major/minor/patch)
   - 建议版本号增长策略
   - 检查破坏性变更

2. **变更日志生成**
   - 提取功能变更
   - 整理 bug 修复
   - 列出重要更新

3. **发布检查**
   - 构建测试
   - 依赖安全检查
   - 文档更新提醒

版本类型：$ARGUMENTS (major/minor/patch/prerelease)
