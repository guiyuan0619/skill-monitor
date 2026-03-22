# Skill Monitor

Skill Monitor is a Codex skill for auditing which other skills influenced the current reply and, when evidence is strong enough, suggesting which skills to keep enabled, observe, switch to on-demand use, or consider disabling.

It is designed for people who actively tune their Codex skill setup and want a lightweight, evidence-based view of what is helping, what is noisy, and what may not be worth leaving on all the time.

## Features

- Reports which non-self skills were actually used in the current reply
- Excludes `skill-monitor` from its own monitoring output
- Uses a fixed bilingual output format
- Adds evidence-based toggle suggestions
- Distinguishes between facts and heuristic recommendations
- Falls back to `Insufficient evidence` instead of over-claiming

## Recommendation Model

Skill Monitor does not make absolute decisions. It gives heuristic recommendations based on observable evidence from the current turn.

Its recommendation logic is based on three signals:

- Direct contribution: whether a skill clearly improved structure, knowledge, workflow, or tool use
- Context cost: whether a skill appears to add complexity, noise, or routing burden
- Trigger quality: whether the skill was activated in an appropriate, high-value context

Allowed recommendation labels:

- `Keep`
- `Observe`
- `Use On Demand`
- `Consider Disabling`

If the available evidence is weak, Skill Monitor explicitly reports insufficient evidence instead of pretending to know.

## Output Format

Skill Monitor appends two compact sections at the end of a reply:

```markdown
Triggered Skills
- Skill Name: `openai-docs` | Reason: current official docs lookup | Confidence: High

Skill Suggestions
- Skill Name: `openai-docs` | Recommendation: Keep | Basis: strong task-to-skill match | Confidence: High
```

If no other skills were used:

```markdown
Triggered Skills: None
Skill Suggestions: Insufficient evidence
```

## Installation

Install `skill-monitor` with `$skill-installer` using the GitHub repository URL:

```text
https://github.com/<your-username>/skill-monitor
```

Example install prompt:

```text
Use $skill-installer to install the skill from:
https://github.com/<your-username>/skill-monitor
```

After installation, the skill should appear under your local Codex skills directory:

```text
~/.codex/skills/skill-monitor
```

On Windows, this is typically:

```text
C:\Users\<your-user>\.codex\skills\skill-monitor
```

Expected structure:

```text
skill-monitor/
|-- SKILL.md
|-- agents/
|   `-- openai.yaml
`-- references/
    `-- reporting-format.md
```

## Usage

Invoke the skill explicitly when you want monitoring and toggle suggestions for the current reply.

Example prompt:

```text
Use $skill-monitor to answer my question and append a skill report with recommendations.
```

More explicit example:

```text
Use $skill-monitor to answer my question and append:
1. a bilingual report of triggered skills
2. the reason each skill was triggered
3. evidence-based suggestions on which skills to keep, observe, use on demand, or consider disabling
```

## Design Principles

- Evidence before suggestion
- Conservative recommendations for single-turn evidence
- No self-reporting
- No fake certainty
- Keep the monitor output short and practical

## Validation

This skill passes the official minimal validation script from the Codex `skill-creator` tooling.

## License

Add the license of your choice before publishing.
