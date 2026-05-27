# 设计阶段规则(由需求文档产出规格文档)

TRIGGER:
- 输入文件为 prd<xx>.md
- 或输入为 prd<xx>_spec 目录

REQUIRED:
- 非编码阶段必须使用 pseudocode-system.md
- 必须保留待决策项与二义性标记
- 规格输出只能进入 prd<xx>_spec
- 计划输出只能进入 prd<xx>_plans

FORBIDDEN:
- 设计阶段禁止生成实际语言实现的代码，防止过早陷入具体语言语法层面
- 禁止跳过 spec 直接生成 plans
- 禁止跳过 plans 直接生成实现