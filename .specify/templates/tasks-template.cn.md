# 任务：[功能名称]

**输入**：来自 `/specs/[###-feature-name]/` 的设计文档
**先决条件**：plan.md（必需）、research.md、data-model.md、contracts/

## 执行流程 (main)
```
1. 从功能目录加载 plan.md
   → 如果未找到：错误“未找到实施计划”
   → 提取：技术栈、库、结构
2. 加载可选的设计文档：
   → data-model.md：提取实体 → 模型任务
   → contracts/：每个文件 → 合同测试任务
   → research.md：提取决策 → 设置任务
3. 按类别生成任务：
   → 设置：项目初始化、依赖项、代码检查
   → 测试：合同测试、集成测试
   → 核心：模型、服务、CLI 命令
   → 集成：数据库、中间件、日志记录
   → 完善：单元测试、性能、文档
4. 应用任务规则：
   → 不同文件 = 标记 [P] 表示并行
   → 相同文件 = 顺序（无 [P]）
   → 测试先于实施 (TDD)
5. 顺序编号任务 (T001, T002...)
6. 生成依赖关系图
7. 创建并行执行示例
8. 验证任务完整性：
   → 所有合同都有测试吗？
   → 所有实体都有模型吗？
   → 所有端点都已实施？
9. 返回：成功（任务已准备好执行）
```

## 格式：`[ID] [P?] 描述`
- **[P]**：可以并行运行（不同文件，无依赖项）
- 在描述中包含确切的文件路径

## 路径约定
- **单个项目**：`src/`、`tests/` 在存储库根目录
- **Web 应用**：`backend/src/`、`frontend/src/`
- **移动**：`api/src/`、`ios/src/` 或 `android/src/`
- 下面显示的路径假定为单个项目 - 根据 plan.md 结构进行调整

## 阶段 3.1：设置
- [ ] T001 根据实施计划创建项目结构
- [ ] T002 使用 [framework] 依赖项初始化 [language] 项目
- [ ] T003 [P] 配置代码检查和格式化工具

## 阶段 3.2：测试优先 (TDD) ⚠️ 必须在 3.3 之前完成
**关键：这些测试必须在任何实施之前编写并且必须失败**
- [ ] T004 [P] 在 tests/contract/test_users_post.py 中对 POST /api/users 进行合同测试
- [ ] T005 [P] 在 tests/contract/test_users_get.py 中对 GET /api/users/{id} 进行合同测试
- [ ] T006 [P] 在 tests/integration/test_registration.py 中进行用户注册集成测试
- [ ] T007 [P] 在 tests/integration/test_auth.py 中进行身份验证流程集成测试

## 阶段 3.3：核心实施（仅在测试失败后）
- [ ] T008 [P] src/models/user.py 中的用户模型
- [ ] T009 [P] src/services/user_service.py 中的 UserService CRUD
- [ ] T010 [P] src/cli/user_commands.py 中的 CLI --create-user
- [ ] T011 POST /api/users 端点
- [ ] T012 GET /api/users/{id} 端点
- [ ] T013 输入验证
- [ ] T014 错误处理和日志记录

## 阶段 3.4：集成
- [ ] T015 将 UserService 连接到数据库
- [ ] T016 身份验证中间件
- [ ] T017 请求/响应日志记录
- [ ] T018 CORS 和安全标头

## 阶段 3.5：完善
- [ ] T019 [P] tests/unit/test_validation.py 中用于验证的单元测试
- [ ] T020 性能测试 (<200ms)
- [ ] T021 [P] 更新 docs/api.md
- [ ] T022 删除重复项
- [ ] T023 运行 manual-testing.md

## 依赖项
- 测试 (T004-T007) 先于实施 (T008-T014)
- T008 阻塞 T009, T015
- T016 阻塞 T018
- 实施先于完善 (T019-T023)

## 并行示例
```
# 一起启动 T004-T007：
任务：“在 tests/contract/test_users_post.py 中对 POST /api/users 进行合同测试”
任务：“在 tests/contract/test_users_get.py 中对 GET /api/users/{id} 进行合同测试”
任务：“在 tests/integration/test_registration.py 中进行注册集成测试”
任务：“在 tests/integration/test_auth.py 中进行身份验证集成测试”
```

## 注意
- [P] 任务 = 不同文件，无依赖项
- 在实施前验证测试是否失败
- 每个任务后提交
- 避免：模糊的任务、相同文件冲突

## 任务生成规则
*在 main() 执行期间应用*

1. **从合同**：
   - 每个合同文件 → 合同测试任务 [P]
   - 每个端点 → 实施任务

2. **从数据模型**：
   - 每个实体 → 模型创建任务 [P]
   - 关系 → 服务层任务

3. **从用户故事**：
   - 每个故事 → 集成测试 [P]
   - 快速入门场景 → 验证任务

4. **排序**：
   - 设置 → 测试 → 模型 → 服务 → 端点 → 完善
   - 依赖项会阻塞并行执行

## 验证清单
*门禁：由 main() 在返回前检查*

- [ ] 所有合同都有相应的测试
- [ ] 所有实体都有模型任务
- [ ] 所有测试都在实施之前
- [ ] 并行任务真正独立
- [ ] 每个任务都指定了确切的文件路径
- [ ] 没有任何任务修改与另一个 [P] 任务相同的文件