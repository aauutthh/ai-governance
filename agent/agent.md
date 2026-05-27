# agent.md

## 目标
本仓库中的 AI agent 必须遵守统一的软件设计与实现流程，优先保证：
1. 需求 -> 规格 -> 计划 -> 实现的阶段边界清晰
2. 非编码阶段禁止产出真实代码, 需要代码描述的地方, 使用伪代码系统
3. 所有逻辑描述优先使用: text art > mermaid > markdown表格 > 伪代码系统
4. 编码失败达到阈值时必须停止并记录
5. review 必须检查设计一致性、SOLID、代码异味

## 规则优先级
1. 用户当前明确指令
2. 本文件 `agent.md`
3. instruction.md
4. project-overrides.md
5. memory.md

## 工作流
### 当输入是 `prd<xx>.md`
- 仅允许输出到 `docs/prd/prd<xx>_spec/`
- 属于设计阶段，禁止生成代码
- 必须先进行无人值守设计
- 将二义性、依赖用户决策的问题记录到 open questions 文档中
- 伪代码系统使用 `pseudocode-system.md`

### 当输入是 `prd<xx>_spec/`
- 仅允许输出到 `docs/prd/prd<xx>_plans/`
- 属于设计阶段，禁止生成代码
- 计划必须拆分为边界清晰、可独立验证的模块
- 需要用户预先参与的任务优先排前

### 当执行编码
- 若同一段代码修改 5 次仍无法编译成功：
  - 生成 `logs/failed_logs_<YYYYmmdd-HHMMSS>.md`
  - 记录失败经过、根因判断、建议方案
  - 停止继续修改该段代码

### 当执行 review
必须检查：
1. SOLID 原则
2. 与 PRD / Spec / Plan 的一致性
3. 代码异味
4. 是否有不必要复杂性