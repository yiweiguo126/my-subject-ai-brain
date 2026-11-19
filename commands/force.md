# 命令：强制重做 / force

**触发**：「force 重做」「强制重导」「重新导入这份」「force」。

> 对**一份已导入**的材料重新编译。去重用 `raw/.manifest.json`，hash 相同也执行。

## 前置

1. 学生指明唯一一份已导入材料，例如「force 重做 `raw/{课程名}/L1-主题.md`」
2. 加载 `skills/wiki-ingest/SKILL.md`，按「与正常导入的差异」节执行
3. `need_confirm: true` 时**仍然先给要点并等待学生确认**，force 不豁免人在回路

未指明文件、或一次给多份：只提示「一次只重做一份」，不执行。

## 与正常导入的差异（本节是唯一权威定义）

| 项 | 正常导入 | force 重做 |
|---|---|---|
| hash 相同 | 回复「已处理」并停止 | 继续执行 |
| 已有概念页 | 局部补充 | **仍只局部补充，不整页覆盖、不删除原页** |
| 学生已写的「我的理解」 | 不动 | **原样保留，不改写一个字** |
| `confidence` / `last_reviewed` | 由复习更新 | **一律不动** |
| `Open Questions` / 保留疑问 | 不代填 | **不代填、不清理** |
| `raw/.manifest.json` | 新增该路径的条目 | **覆盖同一路径条目**：更新 `hash` / `ingested_at` / `pages_created` / `concepts_created`；保留原有 `concepts_updated` 与 `note`；不新增重复条目 |
| 汇总文件 | 末尾一次性更新 5 个 | 相同 |

> 重做只重写**AI 生成的部分**。学生写进去的内容属于数据层，不因重做而丢失。

## 流程

1. 读 `config.md`、`wiki/hot.md`、这份材料的来源页与相关概念页
2. 给出 3–5 个要点，**等学生确认**
3. 局部补充来源页与概念页；保留「我的理解」、掌握度、开放问题
4. 覆盖 `raw/.manifest.json` 中该路径的条目
5. 末尾一次性更新 `wiki/index.md` `hot.md` `glossary.md` `log.md`；`驾驶舱.html` 数据块只修正该次导入事件的 `delta` 与受影响的 `graph` 节点，**不重复追加生长事件**（维护口径见 `AGENTS.md`「驾驶舱（只读投影）」节）
6. 输出报告：重做了哪份、补充了什么、保留了哪些学生内容

## 绝对禁止

- 修改 `raw/` 下任何文件、覆盖 `config.md`
- 整页覆盖或删除概念页、抹掉「我的理解」
- 改 `confidence` / `last_reviewed`、代填 `Open Questions`
- 一次处理多份材料、跳过 `need_confirm` 的确认
- 重做时生成统一标准答案页
