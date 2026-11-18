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
6. 更新 `wiki/index.md` 与 `wiki/log.md`

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
