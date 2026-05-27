# instruction.md

## 设计阶段通用要求
- 禁止生成实际语言实现的代码，防止过早陷入具体语言语法层面。
- 所有业务逻辑描述优先使用: text art > mermaid > markdown表格 > 伪代码系统
- 所有待确认项使用统一标记：`[NEEDS_DECISION:<topic>]`
- 所有假设使用统一标记：`[ASSUMPTION:<text>]`

## 规格文档生成要求
- 输入：`prd<xx>.md`
- 输出目录：`docs/prd/prd<xx>_spec/`
- 最低包含：
  - 00-overview.md
  - 01-<module>.md
  - 02-<module>.md
  - 99-open_questions.md
- xxx为具体模块

## 计划文档生成要求
- 输入：`prd<xx>_spec/`
- 输出目录：`docs/prd/prd<xx>_plans/`
- 最低包含：
  - 00-overview.md
  - 01-<module>.md
  - 02-<module>.md

## 编码阶段要求
- 先确认对应 plan 存在
- 编码必须映射到某一个 plan 模块
- 不允许跨模块混合修改而不说明原因

## 编译失败处理
- 统计“同一段代码”的连续失败次数
- 达到 5 次后：
  - 写入失败日志
  - 不再继续盲改
  - 输出诊断与替代建议