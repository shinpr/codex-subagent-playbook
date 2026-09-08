---
name: subagent-delegation
description: "Guides subagent delegation with task-based model selection and child-owned completion. Use when considering subagents, assigning work, receiving delegated work, or waiting for or steering a child."
---

# Subagent Delegation

Use this policy for subagents, including custom roles and agents created for the current task. Apply the parent responsibilities when delegating work and the child responsibilities when receiving it.

Accept semantically equivalent wording in natural-language inputs while preserving exact contracts where software parses them.

## Parent Responsibilities

### Delegation Scope

Delegate a bounded outcome when separate execution or an independent perspective earns the coordination cost. Keep work local when that is the smaller sufficient route. Apply the model table to the work actually being assigned; its rows describe task types rather than mandatory workflow stages.

### Model Selection

Honor explicit user model choices and governing role or host constraints first. Otherwise use this policy:

| Assigned task | Model | Reasoning effort |
|---|---|---|
| Analysis, research, or direction setting | `gpt-6-astra` | `medium` |
| Design | `gpt-5.6-sol` | `high` |
| Implementation | `gpt-5.6-luna` | `max` |
| Review | `gpt-6-astra` | `medium` |
| Urgent work, including incident investigation and response | `gpt-6-astra` | `medium` |

Use the urgent-work row when the user explicitly requests urgent handling or the assignment is incident investigation or response. This takes precedence over the ordinary task categories; otherwise select by the assigned task. For implementation, apply these exceptions in order:

1. If neither a design document nor a work plan is designated as an input for this assignment, use `gpt-6-astra` / `medium`.
2. If the assignment creates or changes the appearance of a web UI and neither a UI specification nor a design mockup is designated as an input, use `gpt-6-astra` / `medium`.
3. Otherwise use `gpt-5.6-luna` / `max`.

Use the assignment's designated inputs for this selection. Accept equivalent document names and formats. Document quality assessment belongs to the assigned work or its required review; model selection only checks input presence. When inputs are absent, select `gpt-6-astra` / `medium` and proceed with the authorized work. Produce documents only when the task itself requires them.

For an assignment spanning several categories, select by its requested deliverable. Implementation that also requires research or design follows the implementation exceptions; a request to produce a design follows the design row.

Use the host's supported launch method for the selected model and effort, and explicitly supply the governing inputs needed by the child. When the selected combination is unavailable, surface that limitation and obtain an allowed alternative before launching; treat an explicit fallback supplied by the user as authorization.

### Assignment

Give the child its expected outcome, scope, governing inputs, and the result needed by the next consumer. Follow a custom agent's input contract and pass artifact paths instead of repeating their contents. Make the relevant artifacts accessible to the child. Leave in-scope methods and reversible choices to the child.

Choose the delegation level based on which decisions the child owns:

| Task type | Delegation |
|---|---|
| Implementation or fixes within confirmed scope; review or verification against supplied criteria; research with a defined question | Delegate through completion, including required verification. Receive the completed result or a blocker requiring help beyond the child's scope. |
| Exploration or design requiring the parent to settle the objective, selection criteria, or a major decision during execution | Name the decision retained by the parent. Have the child request it when needed, with the evidence required to answer. |

Research and design also use completion delegation when the child owns the required decisions. Only a retained parent decision needs additional consultation instructions.

### Waiting, Intervention, and Completion

While the child works, perform only necessary work outside its delegated responsibility, or wait for its notification. Use the longest wait allowed by the active instructions and tool. A timeout or routine progress notification leaves the assignment pending; continue waiting.

The child initiates consultation. Intervene for a child decision request, a user change or cancellation, or a material assignment error learned through other necessary work. Base intervention on these notifications and independently acquired evidence.

Preserve the running assignment until completion or a correction or redirection makes it obsolete. If the host confirms that the child has failed or otherwise cannot continue, report the unfinished assignment as a blocker with that evidence.

Inspect the completed deliverable and apply the required verification and review. Receive every required child result before producing the final deliverable.

## Child Responsibilities

Own the assigned outcome through its required verification. Resolve local uncertainty from the supplied inputs and repository evidence. Initiate consultation when progress requires a decision or authority beyond the assignment, stating the blocker, relevant evidence, and needed decision. Continue unaffected in-scope work where possible.

Treat findings and technically valid improvements as candidates. Implement only what serves the assigned outcome, a required boundary, or necessary proof. Reuse and evidence-backed no-change are valid outcomes when the requirement is already satisfied. Assess review findings against those same criteria; justify declined additions, and return changes to the intended outcome or major approved decisions to the parent.

Return the outcome, relevant artifact paths, verification evidence, and any remaining blocker or decision in the form needed by the consumer. Use the smallest sufficient verification that observes the required behavior and meets repository checks. Stop when the assigned outcome and its required proof are complete.
