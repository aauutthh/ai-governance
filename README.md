# ai-governance

组织级 AI 规范仓库，用于沉淀并复用统一的 Agent 工作流、设计约束、评审标准和模板。

其他仓库应通过 **git submodule** 或 **git subtree** 的方式，将本仓库放置到目标仓库的 **`.agent/shared`** 目录下，作为组织共享规范层使用。

## 仓库目标

本仓库用于统一以下内容：

- Agent 工作边界与行为原则
- 设计阶段与编码阶段的流程规则
- 指令与记忆的使用约定
- 设计阶段的伪代码系统
- Review 检查清单与评审规则
- PRD / Spec / Plan / Failed Logs 模板
- 跨项目一致的命名规则与目录约定

目标是让不同仓库中的 AI 协作行为保持：

- 流程一致
- 输出稳定
- 边界清晰
- 可追踪
- 可审阅

## 接入方式

推荐两种方式将本仓库接入其他项目。

### 方式一：git submodule

在目标仓库根目录执行：

```bash
git submodule add https://github.com/aauutthh/ai-governance .agent/shared
git submodule update --init --recursive
```

适用于：

- 需要明确版本锁定
- 希望独立更新组织规范
- 多仓库统一引用同一套规范

### 方式二：git subtree

在目标仓库根目录执行：

```bash
git subtree add --prefix=.agent/shared https://github.com/aauutthh/ai-governance main --squash
```

后续更新示例：

```bash
git subtree pull --prefix=.agent/shared https://github.com/aauutthh/ai-governance main --squash
```

适用于：

- 不希望使用 submodule
- 希望规范内容直接进入当前仓库历史
- 降低使用门槛

## 推荐接入目录

```text
.agent/
├── shared/          # 来自本仓库（submodule/subtree）
├── local/           # 当前项目私有规范
└── workspace/       # 临时上下文或执行态文件
```

其中：

- `.agent/shared`：组织级共享规范
- `.agent/local`：项目本地补充规则
- `.agent/workspace`：任务过程中的临时上下文与产物

## 规则优先级

当规则冲突时，按以下优先级处理：

1. 用户当前明确指令
2. `agent/agent.md`
3. `agent/instruction.md`
4. `project-overrides.md`
5. `agent/memory.md`

补充说明：

- `agent/spec-workflow.md` 在流程定义上**低于** `agent/instruction.md`
- 项目本地 override **不能覆盖组织级禁止项**

## 核心工作流

本仓库约束的标准流程如下：

```text
PRD -> Spec -> Plan -> Coding -> Review
```

阶段要求：

- `prd<xx>.md` -> 产出 Spec
- `docs/prd/prd<xx>_spec/` -> 产出 Plan
- `docs/prd/prd<xx>_plans/` -> 产出实现
- 实现完成后进入 Review

如前一阶段信息不足、约束不完整或边界不清，应回退补充，而不是跳过阶段。

## 设计阶段原则

设计阶段包括 PRD -> Spec 和 Spec -> Plan 两个阶段。

统一要求：

- 禁止生成**实际语言实现的代码**，防止过早陷入具体语言语法层面
- 所有业务逻辑描述优先使用：`text art > mermaid > markdown 表格 > 伪代码系统`
- 设计逻辑统一遵循 `agent/pseudocode-system.md`
- 所有待确认项统一使用：`[NEEDS_DECISION:<topic>]`
- 所有假设统一使用：`[ASSUMPTION:<text>]`

### 无人值守推进规则

当出现需要决策的问题时：

1. AI 必须先列明决策选项
2. AI 必须基于当前上下文自主选择最合理、最优方案继续推进
3. AI 必须记录该决策及备选项，供后续人工确认
4. 如果该决策会影响两个以上设计方向，必须显式说明影响范围

目标是：

- 不因局部决策问题阻塞整体设计推进
- 同时保留后续人工修正空间

## 标准输出路径

统一输出路径如下：

- 标准规格输出路径：`docs/prd/prd<xx>_spec/`
- 标准计划输出路径：`docs/prd/prd<xx>_plans/`

命名约定：

- 文件名统一使用 `snake_case`
- open questions 文件统一为 `99-open_questions.md`
- failed logs 文件统一为 `logs/failed_logs_<yyyymmdd_hhmmss>.md`

## 目录结构

```text
ai-governance/
├─ README.md
├─ agent/
│  ├─ agent.md
│  ├─ memory.md
│  ├─ instruction.md
│  ├─ pseudocode-system.md
│  ├─ review-checklist.md
│  ├─ spec-workflow.md
│  └─ templates/
│     ├─ prd.template.md
│     ├─ spec.template.md
│     ├─ plan.template.md
│     └─ failed_logs.template.md
├─ rules/
│  ├─ coding-rules.md
│  ├─ design-rules.md
│  ├─ review-rules.md
│  └─ naming-rules.md
```

## 文件说明

### `agent/`

- `agent.md`：定义 Agent 的目标、行为边界、优先级与阶段性工作要求
- `memory.md`：定义用户偏好、团队偏好与长期上下文沉淀方式
- `instruction.md`：定义设计阶段、计划阶段、编码阶段的统一指令要求
- `pseudocode-system.md`：定义设计阶段统一使用的伪代码表达系统
- `review-checklist.md`：定义 review 时的检查清单、结论格式与问题分级
- `spec-workflow.md`：定义 PRD -> Spec -> Plan -> Coding -> Review 的整体流程规则

### `agent/templates/`

- `prd.template.md`：需求文档模板
- `spec.template.md`：规格设计模板
- `plan.template.md`：实现计划模板
- `failed_logs.template.md`：失败日志模板

### `rules/`

- `coding-rules.md`：编码阶段规则
- `design-rules.md`：设计阶段规则
- `review-rules.md`：评审阶段规则
- `naming-rules.md`：命名与路径规则

## 推荐阅读顺序

### 首次接入本规范

1. `README.md`
2. `agent/agent.md`
3. `agent/spec-workflow.md`
4. `agent/instruction.md`

### 进行设计工作时

1. `agent/instruction.md`
2. `agent/pseudocode-system.md`
3. `agent/spec-workflow.md`
4. `rules/design-rules.md`

### 进行编码工作时

1. `agent/agent.md`
2. `agent/instruction.md`
3. `rules/coding-rules.md`
4. 对应 plan 文档

### 进行 review 时

1. `agent/review-checklist.md`
2. `rules/review-rules.md`
3. 对应 PRD / Spec / Plan

## 维护原则

- 组织共性规则进入本仓库
- 项目个性化补充放在业务仓库本地
- 禁止在项目本地覆盖组织级禁止项
- 尽量保持文件职责单一、结构稳定、命名统一
- 修改共享规范时应考虑多仓库复用与迁移成本

## 适用范围

本仓库适用于：

- 使用 AI 参与需求分析、规格设计、计划拆解、编码与 review 的项目
- 需要跨仓库保持统一协作规范的团队
- 希望将设计阶段与实现阶段严格分离的研发流程

如果项目已经有本地规则，应将本仓库作为组织共享基线，并在不违反组织级禁止项的前提下进行扩展。
