# ResumeLens（简历初审 × 飞书 CLI）

**仓库**：[github.com/Lucky2024-pllove/ResumeLens](https://github.com/Lucky2024-pllove/ResumeLens)

面向招聘初筛的 **Agent Skill**：对简历做结构化提取、维度打分、风险与面试题、薪酬参考；在用户需要时，通过 **[飞书官方 CLI `@larksuite/cli`](https://www.npmjs.com/package/@larksuite/cli)** 将结果写入 **多维表格（Base）** 或 **云文档（docs）**。

## 功能概览

- **单份简历**：信息抽取、多维度 1–10 分、综合分、标签、风险点、面试问题、薪酬区间建议。
- **批量 / 对比**：排序、阈值筛选、横向对比与推荐顺序。
- **飞书落地**（可选）：`lark-cli base` 建表/写字段/批量写记录；`lark-cli docs` 新建或追加 Markdown 报告。
- **约束**：不编造简历或 JD 缺失信息；真实性评分过低时不作「强烈推荐」；写入飞书须真实执行 CLI 并回传结果链接或错误。

详细评分口径、JSON 模板与 Base 字段建议见仓库内 [`SKILL.md`](./SKILL.md)。

## Demo

仓库内 [`demo/`](./demo/) 提供**虚构**简历、岗位要求、与标准 JSON 对齐的示例结果，以及可用于云文档试写的简报 Markdown；用法见 [`demo/README.md`](./demo/README.md)。

## 环境要求

- 已安装 Node.js，可全局安装：`npm i -g @larksuite/cli`
- 飞书开放平台应用（或使用用户授权场景），并按需完成 scope 与登录
- 将本仓库中的 `SKILL.md` 安装为 Cursor / Claude 等环境的 Skill（具体以你所用工具的 Skill 加载方式为准）

首次使用请按飞书 CLI 文档完成：

```bash
lark-cli config init --new
lark-cli auth login --scope "<按控制台或错误提示补齐的 scope>"
```

写入操作默认使用用户身份时，在命令后加 `--as user`（与官方 CLI 一致）。

## 使用方法

### 1. 仅在对话中出结论（不写飞书）

向 Agent 提供：简历全文或附件内容 + 结构化岗位要求。说明「只在聊天里输出 JSON / 简报 / 详细报告」即可，无需执行 `lark-cli`。

### 2. 写入多维表格（Base）

1. 确认已有 `base_token` 与 `table_id`，或让 Agent 执行 `lark-cli base +base-create` 与 `+table-create` 创建 Base/表（写入前需你确认）。
2. 字段名建议与 [`SKILL.md`](./SKILL.md) 中「飞书落地模板 A」一致；创建表时使用 `--fields` JSON 定义列类型。
3. 批量写入（每批最多 200 行）：

```bash
lark-cli base +record-batch-create \
  --as user \
  --base-token bascnXXXXXXXX \
  --table-id tblXXXXXXXX \
  --json '{"fields":["姓名","综合得分","核心评价"],"rows":[["张三",8.1,"匹配度高，需核实某项目时长"]]}'
```

写入前应用 `+field-list` 核对字段名与类型；复杂类型（多选、人员、日期等）需遵守 CLI 对记录值的格式要求（参见飞书官方 `lark-base` skill 中的记录值说明）。

### 3. 写入云文档（docs）

新建报告：

```bash
lark-cli docs +create --as user \
  --title "Java岗-简历初审-20260421" \
  --markdown $'## 摘要\n\n- 岗位：...\n- 本批人数：...\n\n## 排名\n\n...'
```

向已有文档追加一节：

```bash
lark-cli docs +update --as user \
  --doc doxcnXXXXXXXX \
  --mode append \
  --markdown $'## 候选人 B\n\n（结构化正文）'
```

若用户只提供 Wiki 链接（`/wiki/...`），需先用 `lark-cli wiki spaces get_node` 解析出真实 `obj_token` 再操作文档。

## 示例对话（给 Agent 的提示）

**示例 A — 只出 JSON**

> 岗位：高级后端，5 年+ Go，熟悉 K8s。下面是简历：「……」  
> 请按 SKILL 里的标准 JSON 输出，不要写入飞书。

**示例 B — 写入 Base**

> 同上简历与岗位。请用 lark-cli 把结果写入我的 Base，`base_token` 为 bascnXXX，`table_id` 为 tblYYY；若字段不齐请先 `+field-list` 再 `+record-batch-create`。

**示例 C — 新建云文档**

> 批量三份简历初评，生成 Markdown 详细报告，并用 `lark-cli docs +create` 新建文档，标题「数据岗初筛-20260421」。

## 仓库结构

| 路径 | 说明 |
|------|------|
| [`SKILL.md`](./SKILL.md) | Skill 定义：工作流、评分规则、JSON 模板、**飞书 CLI 强制执行步骤** |
| [`demo/`](./demo/) | 虚构输入/输出示例，便于联调 Agent 或试写飞书文档 |
| [`LICENSE`](./LICENSE) | MIT |

## 开源与贡献

- 代码与文档以 **MIT** 许可发布，可自由用于内部或商业场景（保留许可证声明即可）。
- 欢迎通过 Issue / PR 修正文档、补充示例命令或与新版 `lark-cli` 的兼容性说明。

## 免责声明

本 Skill 输出为 **辅助决策**，不构成法律或录用承诺；薪酬与录用结论需结合公司政策与市场数据由 HR 与用人部门最终确认。
