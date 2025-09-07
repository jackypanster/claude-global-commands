---
allowed-tools: Bash(git fsck), Bash(git gc), Bash(git remote), Bash(git branch), Bash(git log)
description: Comprehensive Git repository health check and cleanup
---

## 仓库状态
- 远程仓库：!`git remote -v`
- 分支概况：!`git branch -a | wc -l` 个分支
- 仓库大小：!`du -sh .git`

Git 仓库健康检查：

1. **仓库完整性**
   - 检查对象完整性
   - 验证引用一致性
   - 扫描悬空对象

2. **性能优化**
   - 垃圾回收建议
   - 大文件检测
   - 包文件优化

3. **分支管理**
   - 识别已合并分支
   - 清理过期分支建议
   - 远程分支同步状态

4. **安全检查**
   - 敏感信息扫描
   - 提交签名状态
   - 权限配置检查

检查范围：$ARGUMENTS
