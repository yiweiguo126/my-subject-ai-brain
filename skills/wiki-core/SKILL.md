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

1. **每次会话首先只读 `wiki/hot.md`**（≤500 词）；含所需信息就不再读其他页
2. 需要更多上下文才读 `wiki/index.md`
3. 每次导入最多读 3–5 个已有页面
4. **局部编辑，不全文重写**
5. 概念页 100–300 行为宜，超过则拆分
6. 批量操作时 `index/hot/glossary/log/manifest` **最后一次性更新**
7. 导入前查 `raw/.manifest.json`，hash 相同则跳过

> v0.1 每次导入只维护 5 个汇总文件。`overview.md` / `connections-log.md` / `contradictions.md` 保留占位，默认不自动更新。

## 个体性：先读 config.md

`config.md` 决定「这个大脑按谁的方式长」。每次操作都按它调整：

| 字段 | 影响 |
|---|---|
| `my_goal` | 概念页详略与出题方向（听懂 / 应试 / 做项目 / 写论文） |
| `language` | 中英双语 / 纯中文 / 纯英文 |
| `note_style` | 费曼式 / 应试型 / 极简型 |
| `subject_hint` | 学科写作提示（见下） |
| `need_confirm` | 导入前是否先与学生确认 |
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

## 硬规则

1. 永不改 `raw/`；永不覆盖 `config.md`
2. 不代填概念页 `Open Questions`
3. `confidence` / `last_reviewed` 只由**学生复习时的回答**更新
4. 每条结论可回溯到 `wiki/sources/`
5. 不生成统一标准答案页；不同学生的 `wiki/` 应当不同
6. 名称口径：简称「学科AI大脑」，不可省略 AI
7. 「预习」只基于已导入材料；材料不足时不得生成答案、推测补全或联网，只建议补材料 / 保留疑问并提醒向专业老师提问，待学生形成结论后再入库；预习不改 `confidence`
8. `raw/` 只放 `.md`；原始文件由学生在项目外转换、本人核对后放入，AI 不转换、不导入非 md、不推测补全转换中丢失的内容
9. 「预习 / 复习 / 出题 / 实践」可带课程限定语；实践按一门课分别记录、不跨课混写。**它不是新命令**，`config.md` 结构不变。范围规则见 `docs/多课程使用说明.md`
10. 「实践」只记录学生本人的操作、现象与问题，独立成支，不与出题 / 复习 / 预习互相引用，不设教师裁定栏，不新增 `config.md` 配置项。课件没写的操作报错，只在受控例外三项同时满足时给通用排查思路（核对前提 / 复现步骤 / 读报错原文 / 缩小范围 / 回退上一步 / 记录现象供教师判断），每条单独成段标注「非课件内容，需教师确认」；仍不联网、不生成课件结论、不改 `confidence`。三不（不生成答案、不推测补全、不联网）是「预习」的硬规则，原词与语义不变；两处边界独立，互不覆盖、互不放宽。
