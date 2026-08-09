---
name: repo-harness
description: Use when setting up or auditing a repository harness, improving agent readiness, or diagnosing development and verification infrastructure around a software repository.
---

# Repo Harness

Improve the context, constraints, workflows, feedback loops, and verification infrastructure that help an unfamiliar human or coding agent work safely in a repository.

## Select the intent

Treat these as logical intents; do not require slash-command registration.

| User request | Intent |
| --- | --- |
| “Set up the engineering harness”, “make this repo easier for coding agents” | `harness:start` |
| “Audit the harness”, “check whether this repo is agent-ready” | `harness:audit` |

If the request is ambiguous, inspect the repository first, then ask one focused question.

## Core rules

1. Discover before asking.
2. Use repository evidence before assumptions.
3. Evaluate capabilities, not the presence or absence of named files.
4. Prefer executable knowledge and one source of truth.
5. Preserve useful existing conventions; extend or link before replacing or duplicating.
6. Keep the harness surface proportional to the repository.
7. Diagnose harness quality, not general code quality.
8. Recommend tooling only when evidence shows a real verification or feedback-loop gap; the absence of a common tool is not evidence by itself.

No file is mandatory. A missing `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, or similar entrypoint is not a gap unless agents demonstrably cannot discover or use required guidance.
Before recording a gap, check for equivalent capability elsewhere. A missing file or directory is not a finding by itself; name the affected capability and evidence that it is missing, fragmented, contradictory, or hard to discover.

## Command execution safety

Apply these rules before executing any repository command in either workflow:

1. Inspect the command definition and classify the full command chain, including referenced scripts, hooks, and task dependencies. Do not treat names such as `verify`, `check`, or `test` as proof of safety.
2. Automatically execute only commands classified as safe local validation or read-only inspection, such as tests, lint, type checks, builds, repository queries, or a canonical verification command whose complete chain has been classified as safe.
3. Do not automatically execute deployments, publishing, cloud mutations, production or shared-environment data changes, credentialed external operations, `git push`, or commands with unclear effects.
4. For an unsafe or unclear command, ask for approval before that step or report that runtime verification was not executed. Never claim that skipped verification passed.
5. When safe checks are separable from an unsafe command, run only the clearly safe checks and report the result as partial verification with the skipped limitation.

## `harness:start`

Bootstrap or improve the minimum useful harness for the current repository.

1. Read [references/discovery.md](references/discovery.md) and perform high-signal discovery before asking questions.
2. Identify capability gaps: unclear navigation, hidden constraints, incomplete setup, weak feedback loops, missing verification, or local/CI drift. Record the evidence and affected paths.
3. Ask only for important unknowns that the repository cannot answer. Target 0–4 questions for a normal repository. Do not ask about language, package manager, tests, CI, or commands that inspection can reveal.
4. Read [references/harness-patterns.md](references/harness-patterns.md) and propose the smallest useful correction for each evidenced gap. For every proposed artifact, state its reason; explicitly state what to reuse or skip.
5. Before significant edits, present the proposal. Implement only the approved or clearly authorized scope, preserving existing ownership and conventions.
6. Apply the command execution safety rules above before verification. Do not execute the canonical command blindly. If it is unsafe or unclear, ask for approval or report that runtime verification was not executed. If no canonical path exists, use the smallest safe set of existing checks and explain the resulting limitation.
7. Report changed paths, verification evidence, unresolved uncertainty, and any skipped work.

Prefer a single canonical command such as an existing `make verify`, `just verify`, package-manager task, or equivalent. Do not introduce a new tool merely to make the harness appear complete.

## `harness:audit`

Evaluate how safely and efficiently an unfamiliar human or coding agent can understand, modify, and verify the repository. Do not perform a generic code review.

1. Choose static mode unless the user explicitly requests runtime checks and they are safe. Static mode inspects repository files only.
2. Read [references/discovery.md](references/discovery.md), then inspect enough high-signal evidence to support a confident audit. Evaluate each relevant dimension independently and stop exploring that dimension once the evidence is sufficient to score it confidently. Continue deeper only when evidence is missing, contradictory, or high-risk; never scan exhaustively for confirmation.
3. Read [references/audit-rubric.md](references/audit-rubric.md) and score all seven dimensions from 0–100: Context, Navigation, Constraints, Verification, Feedback Loops, Operational Understanding, and Agent Readiness.
4. Check contradictions between documentation and executable configuration, and duplication that creates unclear ownership or drift. Keep dependency/configuration inconsistencies scoped to setup, deployment, verification, or reproducibility contracts; do not turn the audit into a dependency correctness scanner.
5. Read [references/findings.md](references/findings.md). Make every important finding evidence-backed and include a title naming the broken capability or contract, severity, dimension, evidence, impact, smallest useful fix, and affected paths when relevant. Keep missing paths in the evidence, not as the root problem or title.
6. Recommend only corrections supported by evidence. Do not prescribe linting, type checking, formatting, or other common tooling merely because it is absent; recommend it only when repository evidence shows a real verification or feedback-loop gap. Keep scores as approximate summaries, not scientific measurements; label hypotheses and unknowns instead of inventing facts.
7. In static mode, separate repository facts from inferred runtime impact and use conditional uncertainty language for consequences that were not executed or otherwise proven. Do not state an inferred runtime failure as confirmed.
8. In runtime mode, apply the command execution safety rules above and run only existing checks classified as safe.

Use an output shape like:

```text
REPO HARNESS AUDIT

Context                    <score>/100
Navigation                 <score>/100
Constraints                <score>/100
Verification               <score>/100
Feedback Loops             <score>/100
Operational Understanding  <score>/100
Agent Readiness            <score>/100

Findings
<evidence-backed findings, ordered by priority>

Recommended next steps
<smallest useful corrections>
```

## Reference routing

- Load `discovery.md` for either intent, before questions or broad inspection.
- Load `harness-patterns.md` when proposing or selecting start-work changes.
- Load `audit-rubric.md` for every audit; do not omit a dimension because the repository is small.
- Load `findings.md` when writing audit findings or communicating uncertainty and priority.

Read only the references needed for the selected workflow. Keep detailed domain context in the repository’s own sources when they already provide it.

## Completion criteria

Consider the work complete only when:

- the selected intent was followed;
- discovery preceded questions and changes;
- recommendations are capability-based, minimal, and convention-preserving;
- an audit covers all seven dimensions and important findings contain evidence;
- verification was run safely or its absence is explicit; and
- the final report separates completed work, evidence, remaining uncertainty, and intentionally skipped work.

## Red flags

Stop and re-check the evidence when the next step is:

- creating an agent-specific file because its filename is missing;
- adding documentation for every module or a new command that duplicates an existing one;
- asking a question already answered by a manifest, lockfile, script, CI file, or README;
- assigning a score without a repository path, command, or configuration fact;
- commenting on code quality without explaining the harness impact; or
- expanding a small repository into an enterprise-style harness.
