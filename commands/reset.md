# 命令：清空测试数据 / reset

**触发**：「清空测试数据」「复位为空白模板」「reset」。

> 把内部测试副本复位成发给学生的空白模板。**清理清单以 `docs/FAQ.md`「测试与清理」节为唯一权威定义**，本文件只定义触发、确认与边界。

## 前置（必须先做）

1. 列出将删除 / 复位的文件清单，逐项给学生看
2. 提醒学生：**先自行复制一份当前项目作备份**；本命令不提供撤销
3. 等学生明确确认；不确认就停止

## 执行边界

| 允许 | 禁止 |
|---|---|
| 删除 `docs/FAQ.md`「测试与清理」节列明的测试数据与演示样例 | 删除 `raw/` 中学生自己放进来的课件 |
| 复位 `wiki/` 的汇总文件（`index` / `hot` / `glossary` / `log`）与两个占位页（`overview` / `contradictions`）为空白模板 | 删除各目录的 `_README.md` |
| 经学生确认后，把 `config.md` 的 `course_name` / `subject_hint` 置空 | 覆盖 `config.md` 的其他字段，或重写整个文件 |
| 清空 `Home.md` 静态清单 | 删除 `Home.md` 的两对 `TEMPLATE:START/END` 标记及其区块 |
| 按清单处理说明层里标注为「可选演示样例」的学科内容 | 改动 `skills/`、`commands/`、`AGENTS.md`、`VERSION` |
| 保留 `docs/FAQ.md`「测试与清理」节 C 组列明的文件 | 删除 `docs/本馆资源获取指南.md`、`docs/图书馆资源服务指南.md`、`docs/多课程使用说明.md` |

## 流程

1. 读 `docs/FAQ.md`「测试与清理」节清单，生成本次要执行的文件列表
2. 回显列表 + 备份提醒，等学生确认
3. 按清单逐项删除或复位；`Home.md` 保留模板区
4. 把 `raw/.manifest.json` 复位为 `{"sources": {}}`；同时把 `脑回路生长图.html` 数据块复位为空白状态（`counts` 全 0、`weak_concepts`/`decaying`/`recent`/`pending_confirm` 空、`next_action` 回到「说『初始化』」、`brain_version` 保持当前版本），可见区同步清空；**生长数据同步复位**：`timeline` / `graph.nodes` / `graph.edges` / `milestones.achieved` / `coverage` 全部回到空白态，页面显示结构示意而不报错。按清理清单把 `wiki/connections-log.md` 的已登记连接清空，**保留**头部启用声明与格式说明
5. 跑一次「**体检**」：确认模板层完整、数据层已空、命令表 12 个触发词可用
6. 输出报告：删了哪些、复位了哪些、没碰哪些、下一步说「初始化」

## 与其他命令的边界

- 只清空，**不导入**任何课件；导入用 `commands/ingest.md`
- 只清空，**不升级**底座；升级用 `commands/upgrade.md`
- 清空后 `config.md` 仍是唯一可自由修改的文件
