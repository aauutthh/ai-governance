# ai-governance

组织级 AI 规范仓库。

本仓库用于集中维护可被多个仓库复用的 AI Agent / Copilot / 自动化协作规范。其他项目应通过 **git submodule** 或 **git subtree** 的方式，将本仓库放置到目标仓库的 **`.agent/shared`** 目录下。

## 用途

该仓库用于沉淀组织范围内共享的：

- Agent 行为规范
- 指令编写规范
- 记忆管理规范
- 伪代码系统规范
- 模板文件

## 接入方式

推荐两种方式将本仓库接入其他项目：

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

## 目录结构

```text
.
├── README.md
├── agent.md
├── instruction.md
├── memory.md
├── pseudocode-system.md
└── templates/
```

## 文件说明

- `agent.md`：定义 Agent 的角色、边界、行为方式与协作原则
- `instruction.md`：定义指令设计、书写风格、约束与优先级规则
- `memory.md`：定义上下文、记忆、状态沉淀与使用原则
- `pseudocode-system.md`：定义伪代码表达、任务拆解与流程建模方式
- `templates/`：放置可复用模板，如任务模板、规范模板、提示词模板等

## 建议

在接入仓库后，可由各业务仓库在 `.agent/` 下维护本地扩展，例如：

```text
.agent/
├── shared/          # 来自本仓库（submodule/subtree）
├── local/           # 当前项目私有规范
└── workspace/       # 临时上下文或执行态文件
```

其中：
- `.agent/shared` 存放组织统一规范
- `.agent/local` 存放项目特定规则
- `.agent/workspace` 存放临时执行上下文

## 维护原则

- 组织共性内容进入本仓库
- 项目个性化内容保留在业务仓库
- 尽量保持文件职责单一、命名稳定、目录简洁
- 变更共享规范时，应考虑向后兼容和迁移成本
