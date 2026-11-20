---
name: wiki-ingest
description: 消化课件 md。当学生说「导入 / ingest」、把新的 .md 放进 raw/、或要求把课件变成笔记时加载。含格式前提、去重、确认、页面生成与 6 个末尾更新点。
---

# 导入 — wiki-ingest

把 `raw/` 中的一份**课件 md** 编译成 `wiki/` 页面。

## 前提（先于步骤执行）

`raw/` 只存放 `.md`。若学生指向 PDF / PPT / doc 等非 md 文件，或在 `raw/` 里发现这类文件：**停止本次导入**，只回复「请先按 `docs/材料准备指南.md` 把原始文件转成 md、核对后再放进 `raw/`」。AI 不转换文件、不把非 md 写进 `raw/`、不代学生核对转换是否完整、不推测补全转换中丢失的内容。

学生只说了「导入」、没给文件名时：先列出 `raw/` 下尚未进 `raw/.manifest.json` 的 md，让学生选一份。仍一次一份，不批量。一份都没有时，只回复「`raw/` 里还没有待导入的 md，请按 `docs/材料准备指南.md` 转换并核对后再放进来」，不猜、不代转。

## 步骤

1. **去重检查**：`md5 -q {file}`（macOS）或 `md5sum {file} | cut -d' ' -f1`；查 `raw/.manifest.json`，hash 相同则回复「已处理。要重做请说『强制重做 {file}』，按 `commands/force.md` 执行」并停止
2. **读设置**：读 `config.md`（`my_goal` / `language` / `note_style` / `subject_hint` / `need_confirm`）
3. **读上下文**：只读 `wiki/hot.md`；需要定位时再读 `wiki/index.md`
4. **定位已有页**：最多读 3–5 个相关概念页，判断是新建还是补充
5. **读课件 md**，提炼 3–5 个关键要点（材料已由学生转换并核对；转换中丢失的表格、公式、图示由学生本人对照原始文件修好 md 后再导入，AI 不代核、不推测补全）
6. **与学生确认**（若 `need_confirm: true`，**不可跳过**）：展示要点清单，等学生确认或修正
7. **创建来源页** `wiki/sources/{name}.md`
8. **创建/更新课程总览** `wiki/courses/{课程名}-overview.md`（不存在则建，已有则只在概念图谱追加新链接）
   - 若本份课件 md **明确写出**与前序讲次的关联，或学生本人确认了连接，则在末尾更新时把它登记到 `wiki/connections-log.md`（格式见该文件头部）；`graph` 的 `kind: link` 边只从已登记记录派生，**AI 不推测补全连接**。登记细则见下方「概念间连接登记」节
9. **创建/更新概念页**（最多 3–5 页）：
   - 新概念 → 新建，`confidence: low`，`created_at` 为今天
   - 已有概念 → 局部编辑补充内容，**不改 `confidence` / `last_reviewed`**（掌握度只由「复习」时学生的回答更新，硬规则第 5 条）
   - 按 `subject_hint` 调整写法；正文按 `AGENTS.md`「输出风格约束」节写（STE 80%：一句一事、主动语态、一词一义），细则见 `docs/风格约束.md`
   - 流程 ≥3 步、对比 ≥2 项、时间趋势、层级依赖四类内容，按 `wiki-diagram` 配 1–2 张 Mermaid 图，图注写明来源页与章节
10. **最后一次性更新 6 个末尾更新点**：真相源 `wiki/index.md`、`wiki/hot.md`、`wiki/glossary.md`、`wiki/log.md`、`raw/.manifest.json`，加只读投影 `脑回路生长图.html` 的数据块（写入触发点与字段见本文件「脑回路生长图数据块写入」节，契约见 `AGENTS.md`「脑回路生长图（只读投影）」节）。**脑回路生长图数据块字段全部派生自上述真相源与 config，不在其中独立录入或修正学生状态；它只在末尾更新时写，不在步骤中途写。**

> 不自动更新 `overview.md` / `contradictions.md`；`connections-log.md` 自 v2.00 起启用，登记规则见本文件「概念间连接登记」节。
> 数量口径的唯一权威见 `AGENTS.md`「数量口径的唯一权威」节。

## Manifest 格式

```json
{
  "sources": {
    "raw/{课程名}/L1-主题.md": {
      "hash": "md5值",
      "course": "{course_name}",
      "ingested_at": "YYYY-MM-DD",
      "pages_created": ["wiki/sources/L1.md"],
      "concepts_created": ["Example-Concept"],
      "concepts_updated": [],
      "goals": "{my_goal}"
    }
  }
}
```

## 页面格式

**来源页** `wiki/sources/{name}.md`
```yaml
---
tags: [source, {course-slug}]
course: {course_name}
ingested: YYYY-MM-DD
source_file: raw/{课程名}/{filename}.md
original_file: "{原始文件名与项目外的归档位置}"   # 可选：课件 md 由哪份原始文件转换而来
---
```
章节：关键要点(3–5) → 图表描述 → 新概念列表 → 更新了哪些页面

**课程总览** `wiki/courses/{课程名}-overview.md`
```yaml
---
tags: [course-overview, {course-slug}]
course: {course_name}
updated: YYYY-MM-DD
---
```
章节：综述（一句话）→ 概念图谱 → 我的薄弱环节（静态清单 + 可选 Dataview）→ 来源

**日志** 追加到 `wiki/log.md`：
```markdown
## YYYY-MM-DD — 导入: {source-file}
- 创建概念: `{概念A}`, `{概念B}`
- 更新概念: `{概念C}`
- 目标: {my_goal}
```

## 完成报告

> 处理 1 个来源。创建 X 页，更新 Y 页。按你的目标「{my_goal}」调整了笔记写法。
> 请在 Obsidian 打开 `Home.md` 查看，或打开项目根的 `脑回路生长图.html` 看「下一步」。有疑问的地方我留在了各概念页的 Open Questions，等你来填。

## 脑回路生长图数据块写入（末尾更新点）

在第 10 步末尾一次性更新时，同步改写 `脑回路生长图.html` 的 `<script id="dashboard-data" type="application/json">` 数据块（契约见 `AGENTS.md`「脑回路生长图（只读投影）」节）：

- `generated_at` = 今天；`brain_version` = 当前 `VERSION`。
- `config` 取 `config.md` 的 `course_name` / `my_goal` / `need_confirm`。
- `counts` / `courses` / `weak_concepts` / `decaying` / `islands` 从概念页 frontmatter 与 `wiki/index.md`、`wiki/hot.md` 派生。
- `recent` 追加本次导入记录（与 `wiki/log.md` 一致）。
- `pending_confirm`：要点清单已展示且**学生已确认**后清空该条；确认前保留，不提前写入"已确认"。
- `next_action` 按优先级重算；`question` 写成「打开 AI → 打开项目 → 对 AI 说：「…」」的三步操作指令，不写「回『好』即可」这类本页回答话术（口径见 `AGENTS.md`「脑回路生长图（只读投影）」节）；若本条刚确认、无其他待办，则据实际状态给出「预习 / 看结果」之一。
- **不改 `confidence` / `last_reviewed`**，不把保留疑问写成结论，不把实践内容聚合为课程结论。字段一律从真相源派生，不在数据块里独立录入。
- **生长数据（v2.00，增量追加不重建）**：`timeline` 追加 `type: ingest` 事件（`date` 取 manifest 的 `ingested_at`，`delta` 记本次新建来源页 / 概念页数，`ref` 指向来源页）；`graph` 追加来源节点、`concepts_created` 的概念节点与对应 `derived` 边；若 `connections-log.md` 本次有登记，追加 `kind: link` 边与 `type: connection` 事件；`coverage` 按 `index.md` 重算；`milestones` 据 `timeline` 判定「第一份课件 / 第一个概念 / 第一次连接」。时间字段口径：练习题页取 `created`，其余页取 `created_at`。契约见 `AGENTS.md`「脑回路生长图（只读投影）」节。

## 概念间连接登记

- **可登记**：课件 md 里明确写出的跨讲次关联（写明讲次或概念名），或学生本人明确确认的连接。
- **不可登记**：AI 觉得两个概念相似、主题接近、可类比等推测性关联；材料不足时按「不推测补全」处理，不生成连接、不补写 `note` 结论。
- 每条按 `wiki/connections-log.md` 头部格式追加，须有 `from` / `to` / `source` / `basis`，未填齐不登记。
- 登记与 `graph` 边写入同在「末尾一次性更新」完成，不在步骤中途写。

## 绝对禁止

- 修改 `raw/` 下任何文件
- 覆盖 `config.md`
- 代填 `Open Questions`
- 一次处理多份课件，或未经确认就批量生成（`need_confirm: true` 时）—— 多份文件逐份触发、逐份确认
- 导入 PDF / PPT / doc 等非 md 文件，或替学生转换文件、把原始文件写进 `raw/`
- 代学生核对转换是否完整，或推测补全转换中丢失的表格、公式、图示
- 把学生的原话改写成统一句式（学生内容只追加，不润色）
- 自行建页沉淀对话内容（只能建议，学生确认后再写）
- 在导入后改动课件 md 的文件名、正文标题或内容
