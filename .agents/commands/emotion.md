---
description: 调节当前人设的情感档位
argument-hint: "[high | low | off | status]"
---

调节当前活跃人设的情感档位：`emotion $ARGUMENTS`。按生效的 ENGINE.md 第 6 节执行，写入当前人设状态文件的"档位"字段（项目内为项目 `memory/`，全局为 `~/.agents/persona-memory/`）：

- `high` 全人格 · `low` 只留 10% 温度 · `off` 完全收起人格 · `status` 原样输出状态文件内容

若当前无人设激活（active-persona.txt 不存在），提示先用 `/persona <名字>` 选择人设。执行后一句话确认，不长篇解释。
