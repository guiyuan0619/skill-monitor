# 中文英文双语输出模板 / Bilingual Output Templates

在最终回答末尾追加以下区块。
Append the following blocks at the end of the final answer.

## 标准模板 / Standard Template

```markdown
触发技能 / Triggered Skills
- 技能名 / Skill Name：`openai-docs` | 触发原因 / Reason：查询当前官方文档 | 置信度 / Confidence：高 / High
- 技能名 / Skill Name：`planning-with-files` | 触发原因 / Reason：组织多步骤实现过程 | 置信度 / Confidence：中 / Medium

技能建议 / Skill Suggestions
- 技能名 / Skill Name：`openai-docs` | 建议 / Recommendation：保留 / Keep | 依据 / Basis：当前任务强依赖官方文档 | 置信度 / Confidence：高 / High
- 技能名 / Skill Name：`planning-with-files` | 建议 / Recommendation：按需启用 / Use On Demand | 依据 / Basis：只在多步骤复杂任务中体现明显价值 | 置信度 / Confidence：中 / Medium
```

## 无触发模板 / No-Skill Template

```markdown
触发技能 / Triggered Skills：无 / None
技能建议 / Skill Suggestions：当前证据不足 / Insufficient evidence
```

## 仅有监控无建议 / Monitoring Without Suggestions

```markdown
触发技能 / Triggered Skills
- 技能名 / Skill Name：`find-skills` | 触发原因 / Reason：进行技能发现与筛选 | 置信度 / Confidence：高 / High

技能建议 / Skill Suggestions：当前证据不足 / Insufficient evidence
```

## 书写规则 / Writing Rules

- 始终排除 `skill-monitor`。Always exclude `skill-monitor`.
- 先写监控区块，再写建议区块。Write the monitoring block before the suggestion block.
- 每个技能一行，固定字段，不要写成长段解释。Use one line per skill with fixed fields.
- `建议 / Recommendation` 只能写“保留 / Keep”“观察 / Observe”“按需启用 / Use On Demand”“考虑关闭 / Consider Disabling”。
- `依据 / Basis` 只写一个简短短语，说明该建议的证据来源。
- 若证据不足，明确写 `当前证据不足 / Insufficient evidence`。