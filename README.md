# learning-coach

一个把"读纸质书"换成"和 AI 交互式阅读"的学习教练 skill。让你用 Claude Code / Codex / Cursor 等 agent，按个性化节奏精读一本书 / 一篇博客 / 一份 GitHub 文档 / 一篇论文 / 一段播客转录，直到深度掌握（能在新情境中应用，而不只是复述）。

> An interactive reading coach skill for AI agents. Designed in Chinese, works with any agent that supports custom system prompts (Claude Code, Codex, Cursor, ChatGPT, etc.).

## 核心机制

- **逐篇分发**：AI 把材料拆成"独立知识单元"（不是按原章节），每篇 5–15 分钟阅读量
- **手动循环**：你说"继续"触发下一篇；状态全在文件，跨 session、跨 agent 续读
- **信号 + 证据双确认**：根据你的反馈措辞 **+** 自测表现共同决定是加速、减速、回炉，避免被措辞误导
- **三层考核（L1 复述 / L2 应用 / L3 批判）**：每篇必含 L2 应用题，避免"会背但不会用"
- **Obsidian KB 候选 + 勾选入库**（可选）：每篇末尾提供入库候选，你勾选哪些就归档哪些到知识库 inbox，AI 不擅自写入

## 文件布局

```
~/reading/                                # 学习根目录
└── learning/
    └── <material-slug>/                  # 一份材料一个文件夹
        ├── meta.json                     # 用户画像 / 进度 / 调整日志
        ├── lesson_01_<topic-slug>.md     # 含讲解、自测、你的思考、入库候选
        ├── lesson_02_<topic-slug>.md
        ├── checkpoint_01.md              # 每 3–5 篇一次的深度考核
        └── summary.md                    # 完成后的骨架图 + 推荐下一份材料
```

## 安装

详见 [INSTALL.md](./INSTALL.md)，支持以下 agent：
- **Claude Code**（推荐，自动触发）
- **Codex CLI / Cursor / Cline** 等本地 agent（手动加载 system prompt）
- **ChatGPT / Claude.ai web**（仅内容复用，无法跨 session 续读本地状态）

## 跨 agent 复用矩阵

| Agent | 加载方式 | 自动触发 | 本地状态续读 |
|---|---|---|---|
| Claude Code | 放进 `~/.claude/skills/learning-coach/` | ✅ | ✅ |
| Codex CLI | `codex --system-prompt-file SKILL.md` | ❌ 手动 | ✅ |
| Cursor / Cline | 复制内容到 `.cursorrules` 或 system prompt | ❌ 手动 | ✅ |
| ChatGPT / Claude.ai web | 复制内容粘贴到 system / 自定义 GPT | ❌ 手动 | ❌ 读不到本地文件 |

本地 agent 间无缝切换：Claude Code 写到 lesson_03 → Codex 续读 lesson_04 → 第二天换 Cursor 继续，**因为状态在文件不在对话里**。

## 配置

如果你启用 Obsidian KB 集成，把 SKILL.md 里所有 `<OBSIDIAN_VAULT_ROOT>` 替换为你的 vault 绝对路径（如 `/Users/yourname/iCloud Drive/Obsidian/MyVault`）。

不使用 Obsidian 也可以：直接删掉 SKILL.md 里 "Obsidian 知识库路径"、"处理知识库入库" 两段，以及 `meta.json` 模板中的 `knowledge_base` 字段。其他协议正常运作。

## 设计理念

详细设计文档（信号+证据为何是双确认机制、为什么 L2 应用题必含、多材料分支怎么定的）见仓库的 `docs/`（待补），或读源 SKILL.md 即可——里面规则都有解释。

## 状态

- ✅ 阶段 1：skill 骨架（v2 协议，含信号+证据、多材料分支、L1/L2/L3、KB 候选+勾选）
- ⏳ 阶段 2（试用后再评估必要性）：KB 现有笔记 wiki link 关联 / 间隔重复 / 触发词状态机短指令
- ⏳ 阶段 3：YouTube/播客 URL 自动转录 / 跨设备同步 / agent 自我反思日志 / lesson 排版强约束 / 用户思考质量评估

## License

MIT
