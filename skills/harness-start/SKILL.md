---
name: harness-start
description: Use when bootstrapping, setting up, or improving the engineering harness around an existing software repository.
---

# Harness Start

Bootstrap or improve the minimum useful engineering harness around an existing repository so an unfamiliar human or coding agent can work safely.

## Core rules

1. Discover before asking.
2. Use repository evidence before assumptions.
3. Evaluate capabilities, not the presence or absence of named files.
4. Preserve useful existing conventions; extend or link before replacing or duplicating.
5. Keep the harness surface proportional to the repository.
6. Recommend tooling only when evidence shows a real verification or feedback-loop gap.
7. Do not perform a generic code review.

No file is mandatory. Check for equivalent capability elsewhere before recording a gap. A missing path is evidence only when it demonstrates a specific missing, fragmented, contradictory, or hard-to-discover capability.

## Workflow

1. Read [references/discovery.md](references/discovery.md) and inspect high-signal repository evidence before asking questions.
2. Identify real harness gaps in navigation, constraints, setup, feedback loops, verification, or local/CI parity. Record the evidence and affected paths.
3. Ask only important questions that repository evidence cannot answer. Target 0–4 questions for a normal repository.
4. Read [references/harness-patterns.md](references/harness-patterns.md) and propose the smallest useful correction for each evidenced gap. Explain what to reuse or skip.
5. Before significant edits, present the proposal. Modify the repository only when the user has requested or authorized the change, preserving existing ownership and conventions.
6. Verify the resulting harness safely. Classify every command and its full chain before execution. If runtime execution was not requested or cannot be made safe, use static inspection and state that runtime verification was not executed.
7. Report changed paths, verification evidence, unresolved uncertainty, and skipped work.

## Command safety

Apply these rules before any repository command:

1. Inspect the command definition and classify referenced scripts, hooks, and task dependencies. Names such as `verify`, `check`, or `test` do not prove safety.
2. Automatically execute only safe local validation or read-only inspection.
3. Do not execute deployments, publishing, cloud or shared-state mutations, production data changes, destructive migrations, credentialed external operations, `git push`, or commands with unclear effects automatically.
4. For unsafe or unclear verification, ask for approval or report that runtime verification was not executed. Never claim skipped verification passed.
5. If safe checks can be separated from an unsafe command, run only the clearly safe checks and report partial verification.

Prefer an existing canonical command such as `make verify`, `just verify`, a package-manager task, or an equivalent repository-defined path. Do not add a new tool merely to make the harness appear complete.

## Completion criteria

The start workflow is complete only when discovery preceded questions and edits, recommendations are evidence-based and minimal, existing conventions were preserved, verification was run safely or its absence is explicit, and the final report separates changes, evidence, uncertainty, and skipped work.

Do not produce the seven-dimension audit or implement audit findings in this workflow. Use the `harness-audit` skill for a non-mutating audit, then use this skill for an explicitly requested implementation follow-up.
