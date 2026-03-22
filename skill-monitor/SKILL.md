---
name: skill-monitor
description: 监控当前回答实际触发并使用到的其他 Codex Skills，并在答案末尾用固定模板输出“技能名 + 触发原因 + 置信度 / skill name + reason + confidence”。同时基于可观察证据给出技能开关建议，如保留、观察、按需启用或关闭建议，但只能输出启发式判断并说明依据。When used, this skill should also provide evidence-based heuristic suggestions about which skills to keep, observe, switch to on-demand use, or consider disabling. Use when the user wants visibility into triggered skills, wants to tune skill toggles, wants a skill audit trail, or explicitly invokes $skill-monitor. Always exclude this skill itself.
---

# Skill Monitor

## 目标 / Purpose

在不干扰主回答的前提下，先统计当前回合真正参与生成的其他技能，再基于当前回合可证明的证据，输出简洁、稳定、可比对的中英双语监控区块与技能开关建议区块。
First report which other skills genuinely influenced the current reply, then add a compact bilingual suggestion block based only on evidence that can be justified from the current turn.

## 核心原则 / Core Principles

1. 先报事实，再给建议。Report facts before suggestions.
2. 建议必须是“启发式建议”，不是自动裁决。Suggestions are heuristic, not definitive judgments.
3. 没有证据就不要建议；证据弱时明确说“观察”或“不足以下结论”。Do not suggest without evidence.
4. 永远不要把 `skill-monitor` 计入监控结果，也不要对它给开关建议。Never report or recommend about `skill-monitor` itself.
5. 先完成主任务，再追加监控与建议区块。Finish the main task first.

## 监控规则 / Monitoring Rules

### 计入条件 / Inclusion Rules

仅当至少满足以下一项时，才把某个技能记为“已触发并使用”：
Count a skill as used only if at least one of the following is true:

- 用户显式用技能名或 `$skill-name` 调用了它，且你实际按该技能执行。
- 系统路由要求你打开并遵循了该技能的 `SKILL.md`。
- 你在当前回答中实际使用了该技能目录下的资源、规则或参考文件。

以下情况一律不要计入：
Never count a skill when:

- 技能只是存在于环境中。
- 技能只是被想到，但没有真正使用。
- 技能名称只是出现在示例、说明或类比中。
- 你只是推测它可能有帮助。

### 监控输出字段 / Monitoring Fields

每个已触发技能必须包含以下字段：
Each triggered skill must contain these fields:

- `技能名 / Skill Name`
- `触发原因 / Reason`
- `置信度 / Confidence`

## 建议规则 / Suggestion Rules

### 建议的理论依据 / Basis For Suggestions

基于以下三类证据形成建议：
Build suggestions from these evidence types:

1. `直接贡献 / Direct Contribution`
   当前技能是否对本轮输出带来了明确的结构、知识、流程或工具增益。
2. `成本意识 / Context Cost`
   当前技能的存在与触发是否可能带来额外复杂度、噪音或上下文负担。
3. `触发质量 / Trigger Quality`
   当前技能是否是在合适的任务中被合理触发，而不是误触发或低价值触发。

如果只能看到单轮证据，则建议必须保守。
If only single-turn evidence is available, keep the suggestion conservative.

### 允许的建议类型 / Allowed Recommendation Types

每个技能只能给出以下四类建议之一：
Each skill may receive only one of these recommendation labels:

- `保留 / Keep`
- `观察 / Observe`
- `按需启用 / Use On Demand`
- `考虑关闭 / Consider Disabling`

### 各类建议的判定 / Recommendation Heuristics

- `保留 / Keep`
  当技能与当前任务高度匹配，且确实带来了直接增益时使用。
  Use when the skill strongly matches the task and clearly improved the result.

- `观察 / Observe`
  当技能可能有帮助，但当前证据不足、价值尚不稳定时使用。
  Use when the skill may help but current evidence is limited or inconclusive.

- `按需启用 / Use On Demand`
  当技能可能只适合少数场景，本轮未显示出持续常开价值时使用。
  Use when the skill seems situational rather than broadly valuable.

- `考虑关闭 / Consider Disabling`
  仅当有明确证据表明技能在当前场景中产生了噪音、误触发迹象或收益明显低于复杂度成本时使用。
  Use only when there is clear evidence of noise, mis-triggering, or benefit lower than complexity cost.

### 禁止事项 / Prohibitions

- 不要仅凭技能名判断价值。Do not judge based on the skill name alone.
- 不要只按触发次数给建议。Do not rely on trigger counts alone.
- 不要因为单轮未触发就直接建议关闭。Do not recommend disabling only because it was unused in one turn.
- 若证据不足，不要假装确定。When evidence is weak, say so.

## 输出流程 / Reporting Workflow

1. 列出当前回答真实使用到的技能。
2. 删除 `skill-monitor`。
3. 合并重复项。
4. 为每个技能补充“触发原因”和“置信度”。
5. 若有足够证据，再为相关技能补充“建议 + 依据 + 置信度”。
6. 先输出“触发技能 / Triggered Skills”，再输出“技能建议 / Skill Suggestions”。
7. 以 `references/reporting-format.md` 中的固定模板追加到最终答案末尾。

## 置信度判定 / Confidence Scale

- `高 / High`：技能被显式调用，或你明确打开并遵循了它的说明文件，且依据清晰。
- `中 / Medium`：技能未被显式点名，但你明显依赖了它的规则或资源。
- `低 / Low`：只有部分证据表明技能参与，或建议依据较弱。

若无法给出可信理由，就不要输出该技能或该建议。
If you cannot justify a skill or suggestion credibly, omit it.

## 无触发时的写法 / No-Skill Case

若没有任何其他技能参与当前回答，输出：`触发技能 / Triggered Skills：无 / None`

若没有足够证据给出任何建议，输出：`技能建议 / Skill Suggestions：当前证据不足 / Insufficient evidence`

## 风格要求 / Style Rules

- 使用中英双语。Use bilingual Chinese and English wording.
- 监控区块和建议区块都应尽量短。Keep both blocks compact.
- 不编造内部机制，不输出无法从当前回合证明的信息。Do not invent internal mechanics.
- 保持中性、事实化措辞。Keep the tone neutral and factual.

## 参考 / Reference

严格使用 `references/reporting-format.md` 中的模板与示例。
Follow the templates in `references/reporting-format.md`.