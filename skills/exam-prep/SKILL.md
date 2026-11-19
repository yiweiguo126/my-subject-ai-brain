---
name: exam-prep
description: 出题。当学生说「出题 / exam-prep / 考前练一练」时加载。只挑这个学生自己的薄弱概念，按 my_goal 生成练习题。
---

# 出题 — exam-prep

> 题目**只来自这个学生**的 `confidence: low / medium` 概念，不生成全班统一卷。

## 流程

1. 读 `config.md` 的 `my_goal` 与 `exam_prep`；若为 `false`，提示可在 `config.md` 开启
2. 读 `wiki/hot.md` 取当前薄弱概念；不足时读 `wiki/index.md` 扫 frontmatter
3. 按 confidence 排序：`low` 优先，其次 `medium`；`high` 不出题
4. 生成 5–8 题，写入 `wiki/exam-prep/{课程名}-练习题-NN.md`
5. **只给题目与答题提示，不附标准答案**（答案由学生自己写，避免 AI 代答）
6. 更新 `wiki/index.md` 与 `wiki/log.md`；同时写 `驾驶舱.html` 数据块（只读投影）：更新 `counts.exam_prep`、追加 `recent`、`timeline` 追加一条 `type: exam` 事件、`graph` 追加练习题节点与 `derived` 边（时间取练习题页 `created` 字段）、`milestones` 判定「第一套题」，`next_action` 按优先级重算。**练习题不附答案，数据块也不展示任何答案或答案线索**；字段派生自真相源，契约见 `AGENTS.md`「驾驶舱（只读投影）」节

**日志** 追加到 `wiki/log.md`（v2.10 补齐固定标题，供时间轴解析）：

```markdown
## YYYY-MM-DD — 出题: {课程名}-练习题-{NN}
- 题数：N；依据：confidence low 优先、medium 次之、high 不出题
- 不附答案；答案由学生自己写
```

## 题型随 my_goal 变化

| my_goal | 题型 |
|---|---|
| `听懂` | 概念辨析、举反例、类比迁移 |
| `应试` | 名词解释、简答、对比表、易错判断 |
| `做项目` | 情境应用题、方案设计、决策理由 |
| `写论文` | 论证重构、文献评述、反驳练习 |

## 练习题页格式

```yaml
---
tags: [exam-prep, {course-slug}]
course: {course_name}
created: YYYY-MM-DD
my_goal: 听懂
based_on: [Concept-A, Concept-B]
---
```
章节：本次针对的薄弱概念 → 题目（编号、题干、答题提示）→ 我的作答（留空，学生自己填）→ 自评（留空）

## 硬规则

- 不附标准答案，不代学生作答
- 不出 `confidence: high` 的概念
- 题目可回溯到对应概念页 `[[链接]]`
