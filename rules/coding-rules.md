# 编码阶段(由计划文档产出代码)

TRIGGER:
- 用户明确要求实现代码
- 且存在对应的 spec / plans

REQUIRED:
- 修改必须对应某个计划模块
- 如果同一段代码 5 次修改仍无法编译成功：
  - 写 failed_logs_<timestamp>.md
  - 记录尝试历史、失败原因、建议
  - 停止继续修改