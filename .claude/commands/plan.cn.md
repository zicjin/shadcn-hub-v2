---
description: 使用计划模板执行实施规划工作流以生成设计工件。
---

您可以直接通过代理或作为命令参数提供用户输入 - 在继续提示之前，您**必须**考虑它（如果非空）。

用户输入：

$ARGUMENTS

鉴于作为参数提供的实施细节，请执行此操作：

1. 从仓库根目录运行 `.specify/scripts/bash/setup-plan.sh --json` 并解析 FEATURE_SPEC、IMPL_PLAN、SPECS_DIR、BRANCH 的 JSON。所有将来的文件路径都必须是绝对路径。
2. 阅读并分析功能规范以了解：
   - 功能需求和用户故事
   - 功能性和非功能性需求
   - 成功标准和验收标准
   - 提到的任何技术约束或依赖关系

3. 阅读位于 `.specify/memory/constitution.md` 的章程以了解章程要求。

4. 执行实施计划模板：
   - 加载 `.specify/templates/plan-template.md`（已复制到 IMPL_PLAN 路径）
   - 将输入路径设置为 FEATURE_SPEC
   - 运行执行流程（主）函数步骤 1-9
   - 模板是自包含且可执行的
   - 遵循指定的错误处理和门禁检查
   - 让模板指导在 $SPECS_DIR 中生成工件：
     * 阶段 0 生成 research.md
     * 阶段 1 生成 data-model.md、contracts/、quickstart.md
     * 阶段 2 生成 tasks.md
   - 将用户从参数提供的详细信息合并到技术上下文中：$ARGUMENTS
   - 在完成每个阶段时更新进度跟踪

5. 验证执行是否完成：
   - 检查进度跟踪是否显示所有阶段都已完成
   - 确保所有必需的工件都已生成
   - 确认执行中没有错误状态

6. 报告结果，包括分支名称、文件路径和生成的工件。

对所有文件操作使用带有仓库根目录的绝对路径，以避免路径问题。