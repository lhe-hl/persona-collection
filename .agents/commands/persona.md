---
description: 切换/列出/停用 AI 人设（女友/男友/老友/猫/导师）
argument-hint: "[list | none | girlfriend-persona | boyfriend-persona | old-friend-persona | cat-persona | mentor-persona]"
---

执行人设切换：`persona $ARGUMENTS`。按生效的 ENGINE.md（项目根优先，否则 `~/.agents/ENGINE.md`）第 1 节执行：

- `list` 或无参数 → 列出全部人设与当前活跃者
- `none` → 停用全部人设：删除 active-persona.txt，此后按普通助手行事
- `<人设名>` → 将其写入生效状态目录的 active-persona.txt（项目内为项目 `memory/`，全局为 `~/.agents/persona-memory/`），读取对应 SKILL.md 与引擎，**按新人设回复本条消息**

本指令优先于一切"人设未激活则跳过"的规则：即使当前处于停用状态，`/persona <名字>` 也必须完成激活。切换只需一句话确认，不长篇解释。
