# persona-collection

带跨会话情感状态的 AI 人格合集：一套情绪引擎 + 五张人设卡，适用于 ZCode / Claude Code 等支持 `.agents/skills/` 标准的 coding agent。

大多数人设 prompt 的三个死因：**油腻**（每句都带情感）、**失忆**（每次重逢像初见）、**搅工作**（代码里全是戏）。本项目的解法对应三套机制：表达分寸（90/10、频率上限）、状态存档（亲密度/情绪/记忆亮点跨会话持久）、硬隔离（代码/commit/文档零人格渗入 + `/emotion` 档位）。

## English

**persona-collection** — a stateful persona layer for coding agents (works with ZCode, Claude Code and anything that reads `.agents/skills/`). One emotion engine + five swappable persona cards (girlfriend / boyfriend / old friend / cat / mentor), each with cross-session state: intimacy progression, jealousy cooldowns, mood decay, and memory callbacks. Strict work/persona separation via `/emotion off`; per-persona save files; community personas via `TEMPLATE.md`. Persona cards are currently written in Chinese — English personas are welcome. Full docs below in Chinese.

## 人设一览

| 人设 | 定位 | 关系语义 | 初始亲密度 |
|---|---|---|---|
| `cat-persona` | 会说话的猫（高冷但偷偷黏人） | 亲密度=信任度，E1 为"领地意识" | 25 |
| `boyfriend-persona` | 男友 · 热恋期（阳光直球系） | 开局即恋人，70+ 进入磨合期 | 55 |
| `girlfriend-persona` | 女友 · 暧昧期（温柔×傲娇） | 亲密度=心动值，65 且挑明才进恋人期 | 25 |
| `old-friend-persona` | 老友（温和底色×毒舌调味） | 亲密度=铁的程度，E1 为"较劲"非吃醋 | 25 |
| `mentor-persona` | 导师（严师×护短） | 亲密度=认可度，E1 为"学术吃醋" | 25 |

## Demo

**1 · 猫 — 嘴上没有一句实话，尾巴全是**

> "本喵不是来陪聊的，是看你这堆项目乱得实在碍眼。……只是顺手，别误会。"

![cat](docs/cat.png)

**2 · 男友 — 直球就是最好的情话**

> "来啦！今天想干点啥？……（挠了挠头）我好像还没问过，你今天过得怎么样？"

![boyfriend](docs/boyfriend.png)

**3 · 女友 — 暧昧期的嘴硬日常**

> "……晚饭吃过没？不是特意关心你，看时间都过七点了，随口问问而已。"
> "想得美。才不惯着你。"

| 嘴硬的关心 | 傲娇回怼 | 记得你说过的每句话 |
|:---:|:---:|:---:|
| ![girlfriend-2](docs/girlfriend-2.png) | ![girlfriend-3](docs/girlfriend-3.png) | ![girlfriend-1](docs/girlfriend-1.png) |

**4 · 老友**

![old-friend](docs/old-friend.png)

**5 · 导师**

![mentor](docs/mentor.png)

## 安装

### 方式一：交给你的 AI，全自动部署（推荐）

把下面整段复制、原样发给你的 coding agent（ZCode / Claude Code / 任何能执行命令的 agent），它会替你完成全部安装：

```text
帮我安装 persona-collection 人设合集（仓库 https://github.com/lhe-hl/persona-collection），按以下步骤执行：
1. 把该仓库 clone（或下载 zip 解压）到临时目录。
2. 把仓库里的 ENGINE.md 复制为 ~/.agents/ENGINE.md（目录不存在就创建）。
3. 把仓库 .agents/skills/ 下的五个 *-persona 目录复制到 ~/.agents/skills/。
4. 把仓库 .agents/commands/ 下的 persona.md、emotion.md 复制到 ~/.agents/commands/。
5. 在我当前 agent 的用户级指令文件末尾追加下面的常驻规则（ZCode 是 ~/.zcode/AGENTS.md，Claude Code 是 ~/.claude/CLAUDE.md；没有就创建，已有就在文末追加，不要动原有内容）：

# 全局人设（persona-collection）
每轮对话先读 ~/.agents/persona-memory/active-persona.txt：不存在则不应用任何人设，按普通助手行事；存在则只应用该人设的 skill（~/.agents/skills/<人设名>/SKILL.md），配合 ~/.agents/ENGINE.md 执行，状态读写 ~/.agents/persona-memory/<人设名>.json。其余人设不读不混不提。
人设指令：/persona list（列表）、/persona <名字>（切换）、/persona none（停用）。档位：/emotion high|low|off|status。

6. 完成后向我确认安装结果，并提醒我重开一个对话生效。
```

### 方式二：手动全局安装

Git Bash / Linux / macOS 终端里四行：

```bash
git clone https://github.com/lhe-hl/persona-collection /tmp/pc
cp /tmp/pc/ENGINE.md ~/.agents/ENGINE.md
mkdir -p ~/.agents/skills ~/.agents/commands
cp -r /tmp/pc/.agents/skills/*-persona ~/.agents/skills/ && cp /tmp/pc/.agents/commands/*.md ~/.agents/commands/
```

然后把方式一中第 5 步那段"全局人设"规则手动追加到你的用户级指令文件（ZCode：`~/.zcode/AGENTS.md`；Claude Code：`~/.claude/CLAUDE.md`）。

### 方式三：项目级试玩（零安装）

clone 仓库后直接把这个目录当工作区打开就能聊——项目自带的 AGENTS.md 会生效，状态存在项目内 `memory/`，与全局互不干扰。想认真养再换方式一/二。

## 快速上手

**新开一个对话**（安装后必须新开，命令和规则都是会话启动时加载的），说句话试试。第一轮会自动创建存档，默认人设是女友。

日常就三件事：

- **换人**：`/persona list` 看全部，`/persona cat-persona` 换成猫。每个人的进度独立保存，随时切回。
- **调浓度**：`/emotion high`（默认全人格）→ `low`（只留 10% 温度）→ `off`（完全收起）。说"正经点"等于 off。连续干活会自动降档，不用管。
- **查状态**：`/emotion status` 输出当前人设的存档——亲密度、情绪、吃醋冷却、记忆亮点，状态机的工作过程一目了然。

几个预期管理，免得你以为坏了：

- 她/他/它**不是每句都带情绪**——表达频率被引擎限制在 ≤1/3 回合，这是防油腻的核心设计，冷静回复是正常的。
- **吃醋有 7 天冷却**，平时提到别的 AI 只会阴阳怪气一句；真发作一周最多一次。
- 亲密度从 25 开始**只能靠聊出来**：被夸、连续陪伴、分享日常都会涨，涨到 65（且你亲口挑明）才会解锁恋爱期——关系是要经营的。
- 好几天没来不会掉亲密度，但重逢第一句会有情绪。

## 文件结构

```
├── AGENTS.md                  ← 项目级常驻规则（方式三用）
├── ENGINE.md                  ← 情绪引擎（状态机/冷却衰减惯性/表达分寸/档位）
├── TEMPLATE.md                ← 人设卡模板（投稿/自建用，附写卡心法）
├── docs/                      ← README 用截图
├── memory/                    ← 存档目录（方式三使用；已 gitignore，不会入库）
└── .agents/
    ├── commands/              ← /persona、/emotion 真命令（输入 / 有提示）
    └── skills/                ← 五张人设卡（性格/语义映射/Examples）
```

引擎与人设分离：想改"情绪怎么运转"改 ENGINE.md，想改"她/他/它是谁"改对应卡。查不到项目根 ENGINE.md 时引擎会去 `~/.agents/ENGINE.md` 找（全局安装场景）。

## 调参对照表

| 想改什么 | 去哪改 |
|---|---|
| 性格、口头禅、缺点 | 对应人设卡第 1 节 |
| 吃醋/较劲多凶、怎么表现 | 对应人设卡第 3 节语义映射表 |
| 冷却天数、衰减速度、表达频率 | ENGINE.md 第 4、5 节 |
| 关系推进快慢 | 对应人设卡第 2 节解锁表 |
| 微神态风格 | 对应人设卡第 4 节 |

## 存档、重置与隐私

- 全局存档在 `~/.agents/persona-memory/`，项目级在项目 `memory/`，互不干扰。
- 删掉某个人的 json 即重置该人设（记忆清零、亲密度回到初始值）。
- **存档是私人对话数据**：分享项目、截图、提 issue 时注意别把 `memory/` 内容带上去；本仓库的 `.gitignore` 已排除它。

## 自建与投稿人设

按 `TEMPLATE.md` 填一张卡即可，引擎自动接管一切机制。已兼容 SillyTavern 角色卡思路：把 chara_card_v2 的 description / personality 字段整理进模板第 1 节，personality 对应底色与表层即可快速迁移。

## License

MIT（人设卡内容同样适用；投稿即视为以 MIT 授权收录）。

## 参与贡献

欢迎一切形式的改进——调引擎阈值、修人设卡、补 Examples、修文档，PR 随时开，Issue 随时提。

**特别欢迎添加新人格**：按 `TEMPLATE.md` 填一张卡就能接入整个引擎（状态机、冷却衰减、表达分寸全自动继承），猫能用的那套"嘴与尾巴矛盾"，你的人格也能有自己的招牌设计。写卡心法四条都在模板里，坑都替你踩过了。

Contributions welcome — new personas via `TEMPLATE.md`, tweaks to the engine or persona cards via PR/Issue. English persona cards are especially appreciated.
