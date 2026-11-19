# 命令：复习 / review

**触发**：「复习 第三章」「review」「考考我」。可带课程限定语，指定一门或多门课。

→ 加载 `skills/wiki-review/SKILL.md`。

带课程名时只在该课 `raw/` 与 `wiki/` 的匹配页面范围内提问；不带时按 `config.md` 的 `course_name`。范围规则见 `docs/多课程使用说明.md`。

一次只问一题，等学生作答；按**学生自评 + AI 反馈、学生确认后**更新 `confidence` / `last_reviewed`，并同步写 `驾驶舱.html` 数据块（只读投影：先改概念页、再镜像 weak_concepts/decaying/recent，不得直接在数据块判定掌握度；契约见 `AGENTS.md`「驾驶舱（只读投影）」节）。
AI 不代答、不单方判定掌握度、不代填 `Open Questions`。
