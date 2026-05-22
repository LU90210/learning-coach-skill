# 安装指南

## 通用准备

```bash
# 创建学习根目录
mkdir -p ~/reading/learning

# 克隆本仓库（用于获取最新 SKILL.md）
git clone https://github.com/<your-username>/learning-coach-skill.git ~/learning-coach-skill
```

如果启用 Obsidian KB 集成，把 SKILL.md 里所有 `<OBSIDIAN_VAULT_ROOT>` 替换为你的 vault 绝对路径：

```bash
# macOS 例子
sed -i '' 's|<OBSIDIAN_VAULT_ROOT>|/Users/yourname/Library/Mobile Documents/iCloud~md~obsidian/Documents/YourVault|g' ~/learning-coach-skill/SKILL.md

# Linux
sed -i 's|<OBSIDIAN_VAULT_ROOT>|/home/yourname/Documents/Obsidian/YourVault|g' ~/learning-coach-skill/SKILL.md
```

不使用 Obsidian：删掉 SKILL.md 里的"Obsidian 知识库路径"段、"处理知识库入库"段、以及 meta.json 模板里的 `knowledge_base` 字段即可。

---

## Claude Code（推荐）

```bash
mkdir -p ~/.claude/skills/learning-coach
cp ~/learning-coach-skill/SKILL.md ~/.claude/skills/learning-coach/SKILL.md
```

测试：在任意 Claude Code session 里说 "开始读《XXX》" 或 "我想学 XXX"，skill 应自动触发。

## Codex CLI

```bash
# 启动 codex 时传入 SKILL.md 作 system prompt
codex --system-prompt-file ~/learning-coach-skill/SKILL.md
```

或者把 SKILL.md 内容粘贴到 codex 的 `~/.codex/instructions.md`（具体路径见 codex 文档）。然后 "开始读 XXX"。

## Cursor

把 SKILL.md 内容（去掉顶部 YAML frontmatter）复制到：
- 项目根目录的 `.cursorrules` 文件，或
- Cursor Settings → Rules for AI 输入框

## Cline / 其他 VS Code AI agent

放进对应的 system instructions / custom prompt 配置项。具体名称视 agent 而异。

## ChatGPT / Claude.ai web

仅内容复用（无法续读本地文件）：

1. 打开 [chat.openai.com](https://chat.openai.com) 或 [claude.ai](https://claude.ai)
2. 创建自定义 GPT / Project
3. 把 SKILL.md 内容（含 frontmatter 删掉）粘贴到 Instructions / Custom Instructions
4. 开新对话："开始读《XXX》"
5. 状态全在对话里，关闭窗口就丢——这是 web 端的硬限制

---

## 验证 skill 工作正常

**测试 1（触发识别）**：在 Claude Code 全新 session 里说 "开始读《纳瓦尔宝典》"。预期：自动加载 skill，问背景/目的/深度/时长（材料已说所以不重复问），然后生成 lesson_01。

**测试 2（不误触发）**：说 "帮我看一下这个函数有没有 bug"。预期：skill 不触发，正常进入代码任务。

**测试 3（续读）**：写完 lesson_01 的"我的思考"后另开 session 说 "继续读"。预期：skill 触发，ls `~/reading/learning/` 列出已有材料，读 meta.json + 最新 lesson 文件，生成 lesson_02。

**测试 4（跨 agent）**：Claude Code 写到 lesson_03 → Codex 启动加载 SKILL.md → 说 "继续读 <slug>"。预期：行为一致，能读到本地文件并续写。
