# codex-subagent-playbook

![Codex Subagent Playbook — Delegate. Finish. Follow through.](assets/banner.jpg)

[![Codex CLI](https://img.shields.io/badge/Codex%20CLI-Compatible-10a37f)](https://developers.openai.com/codex/cli)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-blue)](https://developers.openai.com/codex/skills/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

This plugin helps Codex choose suitable models for your tasks and follow delegated work through to a verified result. It favors longer waits to reduce the usage spent on check-ins, with closer attention when work gets stuck.

The plugin contains one Agent Skill. Use it with individual subagents or an existing workflow.

---

## Quick Start

Requires the latest Codex CLI and access to GPT-6 Astra, GPT-5.6 Sol, and GPT-5.6 Luna.

Register the marketplace directly from GitHub:

```bash
codex plugin marketplace add shinpr/codex-subagent-playbook
```

In Codex, open `/plugins`. Find **Subagent Playbook** in **Codex Subagent Playbook**, install it, and start a new session. You can confirm the installation by opening the skill picker and checking that `subagent-playbook:subagent-delegation` is listed.

The skill can activate automatically when Codex considers or manages subagents. For a first task, try:

```text
Use subagents to implement this work plan and review the result.
```

To request the skill explicitly, select `subagent-playbook:subagent-delegation` from the skill picker before sending your request.

---

## Model Selection

The defaults reflect the author's measurements and daily use. Astra handles research and review because judging what is worth changing matters there, and those tasks consume more input than output. Sol handles design after the direction has been set. Luna handles implementation from supplied documents to keep execution costs down, with Astra providing the review that can catch mistakes and redirect the work.

| Task | Model | Effort |
|---|---|---|
| Analysis, research, and direction setting | Astra | medium |
| Design | Sol | high |
| Implementation | Luna | max |
| Review | Astra | medium |
| Urgent work, including incident investigation and response | Astra | medium |

These are the model and effort choices Codex applies when starting a child. Explicit user choices and existing host or role constraints take precedence.

The implementation and urgency rules are:

- Implementation uses Astra medium when neither a design document nor a work plan is designated as an input.
- Creating or changing a web UI's appearance also requires a UI specification or mockup to use Luna. Otherwise, it uses Astra medium.
- Urgent work uses Astra medium, taking precedence over the ordinary task categories.

The implementation rules check which inputs are supplied, not whether their contents pass a design review. Point Codex to the documents you already have; work can proceed with Astra medium when those inputs are absent. The web UI rule reflects the author's experience with UI generation.

<details>
<summary>Background on the defaults</summary>

- [Reasoning Effort Is Not a Quality Setting](https://www.norsica.jp/blog/reasoning-effort-is-not-a-quality-setting) compares design, implementation, and review across several configurations, including Sol high and Luna max.
- [Astra and Sol in Galley: Evaluation Methods and Results](https://www.norsica.jp/resources/astra-effort-evaluation) compares Astra effort levels with Sol high in analysis, implementation, and code review.

These are case studies from one repository. They inform the defaults alongside daily use; the web UI rule remains an experience-based choice rather than a result of these evaluations.

</details>

---

## Delegation

Subagents get time to finish their work, including the checks needed to show it works. Small tasks can stay in the main session when splitting them up would add more overhead than value.

Frequent check-ins consume your usage budget. This plugin encourages Codex to wait longer, then find out how the work is going. If an agent is stuck on a test that never finishes, for example, Codex investigates and helps it move forward.

You can change direction while work is running. Before reporting completion, Codex checks the results and carries out any required review.

---

## License

[MIT](LICENSE)
