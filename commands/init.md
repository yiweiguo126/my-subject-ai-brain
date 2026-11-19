# 命令：初始化 / init

**触发**：学生第一次打开项目，说「初始化」「init」「开始用」。

## 行为

1. 读 `AGENTS.md` → `config.md` → `wiki/hot.md`
2. 若 `config.md` 已由学生填好，**只确认、不覆盖**；发现被预填的测试值时，询问是否改为自己的课程。若学生想整包复位，提示说「**清空测试数据**」（`commands/reset.md`），本命令不删除任何文件
3. 引导回答三个问题，逐项写入 `config.md`（🟢 自由改区）：
   - 课程名 `course_name`
   - 语言 `language` / 笔记风格 `note_style`
   - **你这门课想达到什么目标？** `my_goal`（听懂 / 应试 / 做项目 / 写论文）
4. 询问是否要填 `subject_hint`（学科写作提示，可留空）
5. 确认 `raw/` 与 `wiki/` 目录完整；目录清单含 `concepts/` `courses/` `sources/` `exam-prep/` `preview/` `practice/`，缺目录则照该处 `_README.md` 约定补建占位（各目录 `_README.md` 属模板层，不写学生内容）
6. 在 `Home.md` 写入课程名与初始面板
7. **写入 `驾驶舱.html` 的初始数据块**：`config` 取刚填的 `course_name`/`my_goal`/`need_confirm`；`counts` 保持 0；`next_action` 改为「把课件转成 md 放进 raw/，然后说『导入』」（`source: "no_data_after_init"`）；可见区同步改写。数据块契约见 `AGENTS.md`「驾驶舱（只读投影）」节，字段一律派生自真相源，不在其中独立录入
8. 输出 5 步上手提示，指向 `START-HERE.md`

## 硬规则

- **不覆盖** `config.md` 中学生已有的设置
- 不生成任何示例笔记、不导入任何课件
- 不修改 `raw/`
- 不把学科内容写进 `skills/` 或 `AGENTS.md`
