# Demo 说明

本目录提供**虚拟**简历与岗位要求，仅用于演示 [`SKILL.md`](../SKILL.md) 的输入输出形态；**不得**当作真实候选人或真实招聘依据。

## 文件

| 文件 | 用途 |
|------|------|
| [`sample-jd.md`](./sample-jd.md) | 岗位要求（最少输入之一） |
| [`sample-resume.md`](./sample-resume.md) | 简历正文（最少输入之一） |
| [`sample-result.json`](./sample-result.json) | 与 Skill「标准 JSON」字段对齐的**示例**评估结果（分数与结论为演示用） |
| [`sample-brief.md`](./sample-brief.md) | 简报体 Markdown，可粘贴到 Agent 或用于 `lark-cli docs +create --markdown` 试写云文档 |

## 在 Agent 里怎么用

1. 把 `sample-jd.md` 与 `sample-resume.md` 全文贴进对话，说明「按 ResumeLens SKILL 出标准 JSON，不写飞书」。
2. 对比 Agent 输出与 `sample-result.json` 的字段是否齐全（数值不必一致，口径以 SKILL 为准）。
3. 若要试飞书写入：在已登录 `lark-cli` 的前提下，用 `sample-brief.md` 作为 `--markdown` 的素材来源（注意 shell 转义与 Lark-flavored Markdown 规则）。
