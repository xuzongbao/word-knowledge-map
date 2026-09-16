# 远程安装与使用说明

本仓库遵循 [Agent Skills](https://agentskills.io/specification) 约定：可执行内容在

`skills/word-knowledge-map/`（内含 `SKILL.md` + `templates/` + `references/` + `examples/`）。

仓库地址：https://github.com/xuzongbao/word-knowledge-map

---

## 安装前注意

1. **整包安装**：必须保留同级的 `templates/`、`references/`、`examples/`，不要只拷一个 `SKILL.md`。
2. **目录名**：安装后的文件夹名应为 `word-knowledge-map`，且其下直接是 `SKILL.md`。
3. **安全**：远程 skill 会进入 Agent 上下文并可触发工具调用；安装前请自行审阅 `SKILL.md` 与模板内容。
4. **成图能力**：本 skill 描述「如何做知识地图」；实际出图依赖宿主是否提供文生图 / 浏览器 / 本地渲染等工具。

---

## 方式 A（推荐）：`gh skill install`

需已安装较新的 [GitHub CLI](https://cli.github.com/)，且支持 `gh skill` 子命令。

```bash
# 列出本仓库可发现的 skill
gh skill install xuzongbao/word-knowledge-map

# 用户级（全局）：以 Claude Code 为例
gh skill install xuzongbao/word-knowledge-map word-knowledge-map \
  --agent claude-code --scope user

# 项目级（仅当前仓库）
gh skill install xuzongbao/word-knowledge-map word-knowledge-map \
  --agent claude-code --scope project

# 一次装好（本仓库仅含一个 skill）
gh skill install xuzongbao/word-knowledge-map --all \
  --agent claude-code --scope user
```

把 `--agent` 换成你的宿主即可。常见取值包括：

| `--agent` | 典型产品 |
|-----------|----------|
| `claude-code` | Claude Code |
| `cursor` | Cursor |
| `codex` | OpenAI Codex CLI / IDE |
| `github-copilot` | GitHub Copilot |
| `gemini-cli` | Gemini CLI |
| `opencode` | OpenCode |
| `openclaw` | OpenClaw |
| `grok` | Grok 系 Agent |
| `continue` | Continue |
| `cline` / `roo` / `kilo` | Cline / Roo / Kilo 等 |
| `goose` | Goose |
| `universal` | 通用 / 共享目录 |

完整列表以 `gh skill install --help` 为准（会随 CLI 版本变化）。

多款宿主在**项目级**会共用 `.agents/skills/`；**用户级**则写入各自家目录下的 skills 路径。

更新 / 覆盖：

```bash
gh skill install xuzongbao/word-knowledge-map word-knowledge-map \
  --agent claude-code --scope user --force
# 或（若你的 CLI 版本支持）
gh skill update
```

---

## 方式 B：`git clone` 到宿主 skills 目录

本仓库是「单 skill 仓库」，克隆后把 **`skills/word-knowledge-map` 这一层**拷到宿主的 skills 根目录。

### 通用模板

```bash
REPO=https://github.com/xuzongbao/word-knowledge-map
TMP=$(mktemp -d)
git clone --depth 1 "$REPO" "$TMP/repo"

# 把 DEST 换成下表中的路径
DEST="$HOME/.claude/skills/word-knowledge-map"
mkdir -p "$(dirname "$DEST")"
rm -rf "$DEST"
cp -R "$TMP/repo/skills/word-knowledge-map" "$DEST"
rm -rf "$TMP"

# 确认结构
test -f "$DEST/SKILL.md" && echo "OK: $DEST"
```

### 各宿主默认路径（用户级 / 全局）

| 宿主 | 用户级（全局）目录 | 项目级目录 |
|------|-------------------|------------|
| Claude Code | `~/.claude/skills/word-knowledge-map/` | `.claude/skills/word-knowledge-map/` |
| Cursor | `~/.cursor/skills/word-knowledge-map/` 或 `~/.agents/skills/word-knowledge-map/` | `.cursor/skills/…` 或 `.agents/skills/…` |
| Codex | `~/.codex/skills/word-knowledge-map/`（或 `$CODEX_HOME/skills/…`） | `.codex/skills/…` |
| GitHub Copilot / 多宿主共享 | 视 CLI；项目级常见 `.agents/skills/word-knowledge-map/` | `.agents/skills/…` |
| OpenCode | 以该产品文档为准；常可用 `gh skill … --agent opencode` | — |
| Gemini CLI | `gh skill … --agent gemini-cli` 或产品文档路径 | — |
| Continue / Cline / Roo / Goose 等 | 优先用 `gh skill install … --agent <名>` | 或产品文档中的 skills 目录 |
| 其他兼容 Agent Skills 的宿主 | 放入该宿主文档标明的 `…/skills/<name>/SKILL.md` | 同上 |

说明：

- Cursor 为兼容也会读取 `~/.claude/skills/`、`~/.codex/skills/`；装一份到 Claude/Codex 目录有时两边都能发现，但仍建议按你实际主力宿主安装。
- 若宿主只认「仓库根就是 skill」，也可：`git clone` 到 `…/skills/word-knowledge-map` 后，再把 `skills/word-knowledge-map/*` 提升到该目录根（保证 `SKILL.md` 直接在 skill 文件夹内）。

### Codex 内置安装器（可选）

在 Codex 会话里可用 `$skill-installer`，指向本仓库中的 skill 目录，例如：

```text
$skill-installer install https://github.com/xuzongbao/word-knowledge-map/tree/main/skills/word-knowledge-map
```

（具体命令以你当前 Codex 版本的 `$skill-installer` 说明为准。）

---

## 方式 C：ZIP / 手动下载

1. 打开 https://github.com/xuzongbao/word-knowledge-map  
2. Code → Download ZIP（或 Releases，若有）  
3. 解压后进入 `skills/word-knowledge-map/`  
4. 将该文件夹复制到上表对应的 skills 目录，改名为 `word-knowledge-map`  
5. 确认存在：`…/word-knowledge-map/SKILL.md`

---

## 安装后如何使用

1. **重启会话**：多数宿主在启动时扫描 skills；装完后新开一个 Agent / CLI 会话。  
2. **自动触发**：当你说「做一张单词知识地图」「一眼就会的单词：care」等，Agent 应按 `description` 选用本 skill。  
3. **显式调用**（名称因宿主而异）：
   - Claude Code / Cursor：聊天里 `/word-knowledge-map` 或 Skills 面板选择  
   - Codex：`/skills` 或 `$word-knowledge-map`  
   - 其他：查看该产品的 skills / slash 命令列表  
4. **执行预期**：Agent 应先建 Knowledge IR，再选版式（`full-family` / `phrasal-hub` / `function-word` / `compact-three`），再成图；对你只给一句话 + 图 + 可选记忆线索。  
5. **自检**：让 Agent 打开 skill 内的 `examples/care.json` 或 `examples/stress/get.json`，按样例走一遍 Pipeline。

更细的规则见 skill 内 [`skills/word-knowledge-map/SKILL.md`](./skills/word-knowledge-map/SKILL.md)。

---

## 卸载

删除对应 skills 目录下的文件夹即可，例如：

```bash
rm -rf ~/.claude/skills/word-knowledge-map
rm -rf ~/.cursor/skills/word-knowledge-map
rm -rf ~/.codex/skills/word-knowledge-map
# 项目级
rm -rf .claude/skills/word-knowledge-map
rm -rf .agents/skills/word-knowledge-map
```

若用 `gh skill` 安装且 CLI 提供 `gh skill uninstall` / `remove`，优先用官方子命令。

---

## 故障排查

| 现象 | 排查 |
|------|------|
| 宿主发现不了 skill | 确认路径为 `…/word-knowledge-map/SKILL.md`，不是多套一层目录；重启会话 |
| `gh skill install` 找不到 skill | 确认仓库含 `skills/word-knowledge-map/SKILL.md`；或指定路径 `skills/word-knowledge-map` |
| 只有说明没有图 | 宿主缺少成图工具；可先只要 IR / layout_plan，或换带文生图能力的宿主 |
| 画面像 Soft UI | 按 `templates/infographic-prompt.md` 与 `references/style-target-care.png` 重做视觉 QA |
| 虚词被硬凑派生 | 应走 `function-word` / `phrasal-hub`，见 `templates/layout-modes.md` |

---

## 版本

文档对应仓库 skill **v1.2.1**。安装后可在 `SKILL.md` 标题确认版本号。
