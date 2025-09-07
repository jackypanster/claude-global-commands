---
allowed-tools: Bash(npm test), Bash(npm run test:*), Bash(npm run lint), Bash(npm run fix:*)
description: Run tests and automatically fix failures
---

运行项目测试，如果发现失败则自动尝试修复。请按以下步骤执行：

1. 首先运行完整的测试套件
2. 分析任何失败的测试
3. 识别失败的根本原因
4. 实施修复方案
5. 重新运行测试确认修复成功
6. 如果需要，运行代码格式化和 lint 修复

如果有特定的测试参数，请使用：$ARGUMENTS
