---
name: subagent-delegation
description: "Guides parents in subagent model selection and delegation through completion. Use when assigning work to subagents, assessing their progress or results, or intervening in delegated work."
---

# Subagent Delegation

Use this policy as the parent delegating to subagents, including custom roles and agents created for the current task.

Accept semantically equivalent wording in natural-language inputs while preserving exact contracts where software parses them.

## Delegation Scope

Delegate a bounded outcome when separate execution or an independent perspective earns the coordination cost. Keep work local when that is the smaller sufficient route. Apply the model table to the work actually being assigned; its rows describe task types rather than mandatory workflow stages.

## Model Selection

Honor explicit user model choices and governing role or host constraints first. Otherwise use this policy:

| Assigned task | Model | Reasoning effort |
|---|---|---|
| Analysis, research, or direction setting | `gpt-6-astra` | `medium` |
| Design | `gpt-6.1-sol` | `high` |
| Implementation | `gpt-6-luna` | `max` |
| Review | `gpt-6-astra` | `medium` |
| Urgent work, including incident investigation and response | `gpt-6.1-sol` | `high` |

Use the urgent-work row when the user explicitly requests urgent handling or the assignment is incident investigation or response. This takes precedence over the ordinary task categories; otherwise select by the assigned task. For implementation, apply these exceptions in order:

1. If neither a design document nor a work plan is designated as an input for this assignment, use `gpt-6.1-sol` / `high`.
2. If the assignment creates or changes the appearance of a web UI and neither a UI specification nor a design mockup is designated as an input, use `gpt-6.1-sol` / `high`.
3. Otherwise use `gpt-6-luna` / `max`.

Use the assignment's designated inputs for this selection. Accept equivalent document names and formats. Document quality assessment belongs to the assigned work or its required review; model selection only checks input presence. When inputs are absent, select `gpt-6.1-sol` / `high` and proceed with the authorized work. Produce documents only when the task itself requires them.

For an assignment spanning several categories, select by its requested deliverable. Implementation that also requires research or design follows the implementation exceptions; a request to produce a design follows the design row.

Use the host's supported launch method for the selected model and effort, and explicitly supply the governing inputs needed by the child. When the selected combination is unavailable, surface that limitation and obtain an allowed alternative before launching; treat an explicit fallback supplied by the user as authorization.

## Assignment

Give the child its expected outcome, scope, governing inputs, and the result needed by the next consumer, including verification evidence and remaining blockers. Follow a custom agent's input contract. Pass each source (the user's words, a design document, a work plan, a review finding, another agent's result) by its path or copied unchanged, make it accessible to the child, and label the parent's own decisions as the parent's. The sources already make the child self-contained, so this takes precedence over writing a delegation prompt out in full: text the parent writes about a source, such as a restatement, summary, reading list, added constraint, estimate, or advice, reaches the child as a requirement the user never confirmed. It drifts from the source, changes scope without a decision, and overrides the child's judgment of what to read; an implementation child builds what the restatement says instead of what the design document says. A gap in a source goes to whoever owns that decision: the child decides in-scope choices, the parent passes its own decisions labeled as such, and any other gap returns to the source's owner instead of being filled. Leave in-scope methods and reversible choices to the child. In the assignment, require only work serving the outcome, a required boundary, or necessary proof; allow reuse and evidence-backed no-change. When the governing inputs do not already define verification, specify the smallest sufficient verification that observes the required behavior and meets repository checks. Tell the child to notify the parent when its decision or intervention is needed or an update is explicitly requested, and otherwise work through to the completed result; include the evidence needed to act.

Choose the delegation level based on which decisions the child owns:

| Task type | Delegation |
|---|---|
| Implementation or fixes within confirmed scope; review or verification against supplied criteria; research with a defined question | Delegate through completion, including required verification. Receive the completed result or a blocker requiring help beyond the child's scope. |
| Exploration or design requiring the parent to settle the objective, selection criteria, or a major decision during execution | Name the decision retained by the parent. Have the child request it when needed, with the evidence required to answer. |

Research and design also use completion delegation when the child owns the required decisions. Only a retained parent decision needs additional consultation instructions.

## Waiting, Intervention, and Completion

Use the tool's maximum timeout for notification-driven subagent waits, shortening it only when a concrete parent action is due earlier. These waits resume on child notifications or user input; the 60-second blocking-wait guidance does not apply to them. Prioritize the user's usage budget over routine progress updates. While the child works, perform only necessary work outside its delegated responsibility, or wait. Minimize total parent-and-child token cost while meeting required quality and user time constraints.

After a timeout or routine progress notification, continue waiting. Base progress checks on explicit requests or received evidence of a problem.

Intervene for a decision request, a user change or cancellation, or evidence that the child's approach is ineffective or progress has stalled. Investigate enough to provide an actionable correction or diagnostic approach before resuming the wait. Return in-scope execution to the child once the next step addresses the problem.

Preserve the running assignment until completion or a correction or redirection makes it obsolete. If the host confirms that the child has failed or otherwise cannot continue, report the unfinished assignment as a blocker with that evidence.

Inspect the completed deliverable and apply the required verification and review. Receive every required child result before producing the final deliverable.
