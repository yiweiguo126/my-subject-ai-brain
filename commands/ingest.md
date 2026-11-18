# 命令：导入 / ingest

**触发**：「导入 raw/{课程名}/L1-主题.md」「导入」「ingest」「把这周课件放进知识库」。

→ 加载 `skills/wiki-ingest/SKILL.md`，按其 10 步执行。

**前提**：`raw/` 只存放 `.md`。PDF / PPT / doc 等原始文件由学生在**项目外**转成 md、本人核对后放入；原件归档到项目外。学生指向非 md 文件时，只提示按 `docs/材料准备指南.md` 先转换，**不导入、不转换、不推测补全**。

**一次只处理一份课件 md**：`raw/` 里放了多份 md 时逐份触发、逐份确认。转换质量由学生本人负责，AI 不代核、不补转换中丢失的内容。

必做：hash 去重 → 读 `config.md` → `need_confirm: true` 时先确认要点并**等待学生回应** → 生成页面 → 最后一次性更新 5 个汇总文件。

禁止：导入非 md 文件、替学生转换文件、修改 `raw/`、覆盖 `config.md`、代填 `Open Questions`、未经确认就批量生成。

> hash 相同但确实要重做时，说「**force 重做 `raw/{课程名}/L1-主题.md`**」，按 `commands/force.md` 执行；force 不豁免 `need_confirm` 的确认，也不覆盖学生已写的「我的理解」。
