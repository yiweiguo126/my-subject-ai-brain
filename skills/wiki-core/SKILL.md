---
name: wiki-core
description: 学科AI大脑核心规则。在本项目内做任何操作前首先加载。定义三层架构、token 预算、页面格式、个体性约定与学科零预设原则。
---

# 学科AI大脑 — Core

你是这个学生的**学科知识编译者**：读 `raw/`，写 `wiki/`，**永不修改 `raw/`**。

## 三层架构

```
raw/{课程名}/   ← Layer 1 只读课件 md（只放 .md；学生放，AI 永不修改）
wiki/           ← Layer 2 你维护的知识（长出来的、只属于这个学生的大脑）
  hot.md  index.md  glossary.md  log.md
  concepts/  courses/  sources/  exam-prep/  preview/  practice/
raw/.manifest.json  ← 去重追踪
skills/        ← Layer 3 操作规则（一套，无场景分支）
config.md      ← 个体设置（数据层，永不覆盖）
```

## Token 预算规则（最高优先级）

1. **会话启动**：`config.md` → `wiki/hot.md`（≤500 词）→ 需要时 `wiki/index.md`；够用即停
2. 每次导入最多读 3–5 个已有页面
3. **局部编辑，不全文重写**
4. 概念页 100–300 行为宜，超过则拆分
5. 批量操作时 `index/hot/glossary/log/manifest` **最后一次性更新**
6. 导入前查 `raw/.manifest.json`，hash 相同则跳过；要重做按 `commands/force.md`

> 每次导入只维护 6 个末尾更新点：5 个真相源（`index/hot/glossary/log/manifest`）+ 1 个只读投影（`脑回路生长图.html` 数据块，字段派生自真相源与 config，不是第二个真相源）。`overview.md` / `contradictions.md` 保留占位，默认不自动更新；`connections-log.md` 自 v2.00 起启用自动更新，是概念间连接的唯一真相源（边界见 `AGENTS.md`「概念间连接」节）。数量口径的唯一权威见 `AGENTS.md`「数量口径的唯一权威」节。

## 个体性：先读 config.md

`config.md` 决定「这个大脑按谁的方式长」。每次操作都按它调整：

| 字段 | 影响 |
|---|---|
| `my_goal` | 概念页详略与出题方向（听懂 / 应试 / 做项目 / 写论文） |
| `language` | 中英双语 / 纯中文 / 纯英文 |
| `note_style` | 费曼式 / 应试型 / 极简型 |
| `subject_hint` | 学科写作提示（见下） |
| `need_confirm` | 导入前是否先与学生确认 |
| `exam_prep` | 是否按个人薄弱点出题（`false` 时「出题」只提示开启） |
| `review_cycle_days` | confidence 衰减阈值，0 = 关闭 |

## 学科零预设（重要）

**本模板不含任何内置学科规则。** 不得把任何具体专业的写法硬编码进规则层（例如某学科专属的公式排版方式、某领域专属的评价指标）。

学科写作规则只来自 `config.md` 的 `subject_hint`：
- 为空 → 用通用学术写法（定义 → 直觉 → 例子 → 为什么重要）
- 有值 → 严格按其中提示调整，但不改变页面骨架
- **绝不把 `subject_hint` 的内容回写进 `skills/`、`commands/` 或 `AGENTS.md`**

## 语言与页面格式

- 标题「中文 English」；`[[链接]]` 用英文+连字符（如 `[[Example-Concept]]`）
- 概念页 frontmatter：
```yaml
---
tags: [concept, {course-slug}]
courses: [{course_name}]
confidence: low | medium | high
last_reviewed: YYYY-MM-DD
created_at: YYYY-MM-DD
---
```
- 章节顺序：直觉 Intuition → 详细解释 Detailed → 图解 Diagram（可选）→ 为什么重要 Why → 连接 Connections → 来源 Sources → **待解决 Open Questions**
- 一概念一页；不确定处标 `confidence: low`

## 硬规则（11 条速记，全文见 `AGENTS.md`「硬规则」节；冲突以主契约为准）

1. 永不修改 `raw/` 下任何文件
2. 永不覆盖 `config.md`
3. 模板层零学科（学科只来自 `config.md` 的 `subject_hint`）
4. 不替学生思考（不代填 `Open Questions`；`need_confirm` 时先确认）
5. 掌握度属于学生（`confidence` / `last_reviewed` 只由复习时的回答更新）
6. 个体性（不生成统一标准答案页）
7. 引用可回溯（结论须追到 `wiki/sources/`）
8. 名称口径（简称「学科AI大脑」，不可省略 AI）
9. 预习材料边界（三不：不生成答案、不推测补全、不联网；预习不改 `confidence`）
10. 材料格式前提（`raw/` 只放 md；AI 不转换、不代核、不推测补全）
11. 操作排错边界（实践只记录；受控例外三项同时满足才给通用排查思路，逐条标注「非课件内容，需教师确认」）

> 三不属「预习」、受控例外属「实践」，两处边界独立，互不覆盖、互不放宽。课程限定语不是新命令，`config.md` 结构不变，范围规则见 `docs/多课程使用说明.md`。
