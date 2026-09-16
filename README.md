# word-knowledge-map

「一眼就会的单词」配套 Skill：输入英文单词 → **Knowledge IR** → 手绘涂鸦教学海报（单词知识地图）。

当前版本：**v1.2.1**（弱词族版式 + 风格锁 + 难词压测样例）

## 这是什么

面向小学高年级到初中的英语词汇可视化学习材料。先把知识点收成结构化 IR，再按版式模式成图；禁止跳过 IR，也禁止为填满版面编造派生词。

## 怎么用（给 Agent / 工作流）

1. 阅读根目录 [`SKILL.md`](./SKILL.md)（入口与 Pipeline）
2. 按 [`templates/knowledge-schema.json`](./templates/knowledge-schema.json) 生成 IR
3. 用 [`templates/layout-modes.md`](./templates/layout-modes.md) 选定版式：
   - `full-family`：短语 + 形/副/名齐全（如 `care`）
   - `phrasal-hub`：多义/多短语（如 `get`、`run`）
   - `function-word`：虚词（如 `the`）
   - `compact-three`：内容不够四卡时三卡紧缩
4. 成图对齐 [`templates/style-guide.md`](./templates/style-guide.md) 与 [`templates/infographic-prompt.md`](./templates/infographic-prompt.md)
5. 视觉锚点：[`references/style-target-care.png`](./references/style-target-care.png)

对用户只输出：一句话导语 + 图 + 可选记忆线索；不要暴露 JSON / QA / mode 内部名。

## 目录

```
SKILL.md                 # Skill 正文
templates/               # schema、版式、风格、成图 prompt
references/              # 风格参考图与 diff 笔记
examples/
  care.json              # 完整词族样例
  success.json
  stress/                # get / the / run 压测 IR + 视觉 QA
  stress/renders/        # 推荐成图样张（get-v2 / the-v2 / run-v3）
```

## 压测摘要

| 词 | 版式 | 说明 |
|----|------|------|
| get | phrasal-hub | 得到 / 到达 / 变得 / 易混，无假派生 |
| the | function-word | 定冠词用法对比，不硬凑词族 |
| run | phrasal-hub | 短语 + 延伸义 + 词形；底链纯文字色块（v3） |

详见 [`examples/stress/visual-qa.md`](./examples/stress/visual-qa.md)。

## 许可证

[MIT](./LICENSE)

## 相关

配套 Bot：「一眼就会的单词」  
作者 GitHub：[@xuzongbao](https://github.com/xuzongbao)
