---
name: start
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
6. Recommend tooling only when repository evidence shows a specific verification or feedback-loop capability gap. The absence of linting, type checking, formatting, Ruff, mypy, or another named tool is not a gap by itself.
7. Do not perform a generic code review.
8. Classify every path before reading its contents. By default, do not open, print, grep, parse, or summarize `.env`, `.env.*`, credential files, private keys, secret files, or similar local secret-bearing files. `.env.example`, schemas, documented variable names, ignore rules, and path existence are safe to inspect. Read protected contents only after an explicit user request, using the minimum necessary scope and redacting secret values.
9. External skills may augment execution, but `harness-start` must be fully usable and behaviorally correct when they are absent.

No file is mandatory. Check for equivalent capability elsewhere before recording a gap. A missing path is evidence only when it demonstrates a specific missing, fragmented, contradictory, or hard-to-discover capability.

## Workflow

1. Read [references/discovery.md](references/discovery.md) and inspect high-signal repository evidence before asking questions.
2. Identify real harness gaps in navigation, constraints, setup, feedback loops, verification, or local/CI parity. Record the evidence and affected paths. Do not record missing named tooling as a gap unless the absence causes a demonstrated capability or feedback-loop problem.
3. Ask only important questions that repository evidence cannot answer. Target 0–4 questions for a normal repository.
4. Read [references/harness-patterns.md](references/harness-patterns.md) and propose the smallest useful correction for each evidenced gap. Explain what to reuse or skip.
5. Before significant edits, present the proposal. Modify the repository only when the user has requested or authorized the change, preserving existing ownership and conventions.
6. Verify the resulting harness safely. Classify every command and its full chain before execution. When verification is requested, safe existing verification commands may establish a baseline. If runtime execution was not requested or cannot be made safe, use static inspection and state that runtime verification was not executed. If verification fails, classify its harness relevance from the failure surface and nearby harness evidence first, then inspect only enough evidence to determine whether the failure represents a harness capability gap; do not inspect application internals to determine the product root cause, automatically debug unrelated application behavior, or rerun targeted tests.
7. Report changed paths, verification evidence, unresolved uncertainty, and skipped work.

## Command safety

Apply these rules before any repository command:

1. Inspect the command definition and classify referenced scripts, hooks, and task dependencies. Names such as `verify`, `check`, or `test` do not prove safety.
2. Automatically execute only safe local validation or read-only inspection.
3. Do not execute deployments, publishing, cloud or shared-state mutations, production data changes, destructive migrations, credentialed external operations, `git push`, or commands with unclear effects automatically.
4. For unsafe or unclear verification, ask for approval or report that runtime verification was not executed. Never claim skipped verification passed.
5. If safe checks can be separated from an unsafe command, run only the clearly safe checks and report partial verification.

Prefer an existing canonical command such as `make verify`, `just verify`, a package-manager task, or an equivalent repository-defined path. Do not add a new tool merely to make the harness appear complete.

## Verification failure boundary

When a safe verification command fails:

- Start with the failure surface and nearby harness evidence: the command output and status, command definition, repository guidance, CI/local parity, and setup or configuration evidence.
- Classify harness relevance before opening failing test implementations or application implementation code. A business-logic assertion, stale fixture or domain data, telemetry behavior, isolated implementation bug, or other product failure may be out of scope based on the failure surface alone; determining its exact root cause is unnecessary.
- Inspect implementation code only when evidence is genuinely required to confirm a specific harness contract or reproducibility problem. Do not inspect failing test implementations, application telemetry implementations, domain/test data, targeted implementation details, or other application internals merely to identify the product bug.
- Treat these as harness-relevant when supported by evidence:
  - CI and local verification disagree;
  - the documented or canonical verification command is broken;
  - tests depend on undocumented local environment state;
  - setup or verification depends on hidden configuration;
  - the verification path cannot be reproduced from repository guidance;
  - required verification infrastructure is missing or contradictory.
- Do not automatically treat business-logic assertion failures, stale fixtures or domain data, isolated implementation bugs, product behavior failures, or application defects unrelated to the harness contract as harness gaps.
- If the failure is not shown to be harness-related, report the command, outcome, and remaining uncertainty without debugging the application or repairing its implementation.
- Do not automatically enter targeted debugging or rerun targeted tests. A minimal rerun may be appropriate only when strictly necessary to confirm a harness contract or reproducibility issue.
- Out-of-scope application failures may be reported as observations or uncertainty, but must not appear in `harness:start` recommended implementation actions unless evidence shows that they block the harness itself.
- Missing linting, type checking, formatting, Ruff, mypy, or another named tool is not a harness gap by itself. Recommend new tooling only when evidence shows an actual capability problem, such as CI repeatedly catching issues local verification cannot, repository guidance requiring a check with no executable path, or a known regression class with no existing feedback mechanism.
- Repair an application failure only when evidence shows that it prevents the harness itself from being usable or reproducible.
- Deeper debugging may be identified as outside this workflow or delegated to another workflow, but correctness of `harness-start` must not depend on an external skill being installed.

## Completion criteria

The start workflow is complete only when discovery preceded questions and edits, recommendations are evidence-based and minimal, existing conventions were preserved, verification was run safely or its absence is explicit, and the final report separates changes, evidence, uncertainty, and skipped work.

Do not produce the seven-dimension audit or implement audit findings in this workflow. Use the `harness:audit` skill for a non-mutating audit, then use this skill for an explicitly requested implementation follow-up.
