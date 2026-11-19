---
name: wiki-review
description: 费曼复习。当学生说「复习 / review / 考考我」时加载。AI 提问、学生作答，按学生自评 + AI 反馈更新个人掌握度，AI 不单方判定。
---

# 复习 — wiki-review

> **核心：AI 提问，学生回答。** 掌握度是**个人**的，不是课程大纲，更不是全班统一清单。

## 流程

1. 读 `config.md`（`review_cycle_days`、`my_goal`）与 `wiki/hot.md`
2. 从 `wiki/concepts/` 挑 `confidence: low` 或久未复习的概念，**一次最多 3 个**
3. 按 `my_goal` 调整提问风格：
   - `听懂` → 「用自己的话解释…」「能举个反例吗」
   - `应试` → 「写出定义并列出 3 个要点」
   - `做项目` → 「什么情况下该用它？常见坑是什么」
   - `写论文` → 「这个概念的争议点在哪」
4. **一次只问一题，等学生回答**
5. 学生回答后：给出简短反馈，指出哪里没说清，**不替他补答案**
6. 询问学生自评，结合 AI 的简短反馈，由**学生确认**结论后更新该概念页的 `confidence` 与 `last_reviewed`（局部编辑）；AI 不单方判定
7. 把学生答不上来的点**只提示学生自己填写**到 `Open Questions`；AI 不写入、不代填
8. 最后更新 `wiki/hot.md`（可并入下次批量更新）；同时写 `驾驶舱.html` 数据块（只读投影）：据刚更新的概念页 frontmatter 刷新 `weak_concepts`/`decaying`、追加 `recent`、`generated_at` 设为今天，`next_action` 按优先级重算。**数据块的 `confidence`/`last_reviewed` 派生自概念页，复习只通过「先改概念页、再镜像到数据块」这一路径更新，绝不直接在数据块判定掌握度**；契约见 `AGENTS.md`「驾驶舱（只读投影）」节
9. **末尾追加生长数据**（同一末尾更新点，增量不重建）：`timeline` 追加一条 `type: review` 事件；`graph` 中对应概念节点的 `confidence` 镜像为概念页新值；`coverage` 按 `index.md` 重算；`milestones` 据 `timeline` 判定「第一次复习」。字段契约见 `AGENTS.md`「驾驶舱（只读投影）」节

**日志** 追加到 `wiki/log.md`（复习事件此前无格式约定，v2.10 补齐，供时间轴解析）：

```markdown
## YYYY-MM-DD — 复习: {概念名}
- 学生自评并确认后更新 `confidence` / `last_reviewed`（值见该概念页 frontmatter）
- 未答上来的点只提示学生自己填入 `Open Questions`；AI 不代答、不代填
```

## 更新规则

| 学生自评 + AI 反馈后的确认结果 | confidence |
|---|---|
| 能用自己的话讲清 + 举出例子 | `high` |
| 大致说对但缺例子/细节 | `medium` |
| 说不清或答错 | 保持 `low`，列入薄弱清单 |

- `last_reviewed` 一律更新为今天
- **不得**因「AI 觉得内容简单」而自行提升 confidence
- 复习结果只影响**这一个学生**的 `wiki/`

## 完成报告

> 复习了 N 个概念。当前薄弱：`{概念A}`、`{概念B}`。
> 需要「出题」针对这些练习吗？
