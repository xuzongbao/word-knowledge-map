# word-knowledge-map

「一眼就会的单词」配套 Skill：输入英文单词 → **Knowledge IR** → 手绘涂鸦教学海报（单词知识地图）。

当前版本：**v1.2.1**（弱词族版式 + 风格锁 + 难词压测样例）

## 这是什么

面向小学高年级到初中的英语词汇可视化学习材料。先把知识点收成结构化 IR，再按版式模式成图；禁止跳过 IR，也禁止为填满版面编造派生词。

## 怎么用（给 Agent / 工作流）

1. 阅读根目录 [`SKILL.md`](./skills/word-knowledge-map/SKILL.md)（入口与 Pipeline）
2. 按 [`skills/word-knowledge-map/templates/knowledge-schema.json`](./skills/word-knowledge-map/templates/knowledge-schema.json) 生成 IR
3. 用 [`skills/word-knowledge-map/templates/layout-modes.md`](./skills/word-knowledge-map/templates/layout-modes.md) 选定版式：
   - `full-family`：短语 + 形/副/名齐全（如 `care`）
   - `phrasal-hub`：多义/多短语（如 `get`、`run`）
   - `function-word`：虚词（如 `the`）
   - `compact-three`：内容不够四卡时三卡紧缩
4. 成图对齐 [`skills/word-knowledge-map/templates/style-guide.md`](./skills/word-knowledge-map/templates/style-guide.md) 与 [`skills/word-knowledge-map/templates/infographic-prompt.md`](./skills/word-knowledge-map/templates/infographic-prompt.md)
5. 视觉锚点：[`skills/word-knowledge-map/references/style-target-care.png`](./skills/word-knowledge-map/references/style-target-care.png)

对用户只输出：一句话导语 + 图 + 可选记忆线索；不要暴露 JSON / QA / mode 内部名。

## 安装

远程安装（Claude Code、Cursor、Codex、Copilot、Gemini CLI、OpenCode 等）见 **[INSTALL.md](./INSTALL.md)**。

推荐一键（需支持 `gh skill` 的 GitHub CLI）：

```bash
gh skill install xuzongbao/word-knowledge-map word-knowledge-map \
  --agent claude-code --scope user
```

将 `--agent` 换成你的宿主（如 `cursor`、`codex`、`github-copilot`）。

## 目录

```
INSTALL.md                         # 跨平台远程安装与使用
skills/word-knowledge-map/
  SKILL.md                         # Skill 正文
  templates/                       # schema、版式、风格、成图 prompt
  references/                      # 风格参考图与 diff 笔记
  examples/
    care.json                      # 完整词族样例
    success.json
    stress/                        # get / the / run 压测 IR + 视觉 QA
    stress/renders/                # 推荐成图样张（get-v2 / the-v2 / run-v3）
```

## 压测摘要

| 词 | 版式 | 说明 |
|----|------|------|
| get | phrasal-hub | 得到 / 到达 / 变得 / 易混，无假派生 |
| the | function-word | 定冠词用法对比，不硬凑词族 |
| run | phrasal-hub | 短语 + 延伸义 + 词形；底链纯文字色块（v3） |

详见 [`skills/word-knowledge-map/examples/stress/visual-qa.md`](./skills/word-knowledge-map/examples/stress/visual-qa.md)。

## 许可证

[MIT](./LICENSE)

## 相关

配套 Bot：「一眼就会的单词」  
作者 GitHub：[@xuzongbao](https://github.com/xuzongbao)
