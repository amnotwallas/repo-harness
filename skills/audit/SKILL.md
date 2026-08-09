---
name: audit
description: Use when auditing, evaluating, or diagnosing the engineering harness or agent readiness of a software repository.
---

# Harness Audit

Evaluate how safely and efficiently an unfamiliar human or coding agent can understand, modify, and verify an existing repository. Audit harness quality, not general code quality.

## Non-mutating contract

- Static, repository-file inspection is the default.
- Classify every path before reading its contents. Do not open, print, grep, parse, or summarize `.env` or other secret-bearing/local credential files by default.
- You may observe protected path existence, ignore rules, and references to it, and inspect safe templates such as `.env.example`. Read protected contents only after an explicit user request, using the minimum necessary scope and redacting values.
- Do not modify files, repository state, generated output, or commits during an audit.
- Do not execute tests, builds, lint, type checks, deploys, or other project commands unless the user explicitly requests runtime validation.
- For an explicit runtime request, classify the full command chain first and execute only safe local validation or read-only inspection.
- Ask for approval or report skipped verification when a command is unsafe or unclear. Never claim skipped verification passed.
- Do not implement findings or recommendations in this skill. Use `harness:start` for an explicitly requested implementation follow-up.

## Workflow

1. Read [references/discovery.md](references/discovery.md), classify paths before content reads, then inspect enough high-signal evidence to evaluate each relevant dimension confidently. Stop a dimension when evidence is sufficient; continue only for missing, contradictory, or high-risk evidence.
2. Read [references/audit-rubric.md](references/audit-rubric.md) and score all seven dimensions: Context, Navigation, Constraints, Verification, Feedback Loops, Operational Understanding, and Agent Readiness.
3. Check contradictions between documentation and executable configuration, including dependency/configuration sources that make setup, deployment, verification, or reproducibility contracts unclear. Do not perform package-by-package dependency correctness scanning.
4. Read [references/findings.md](references/findings.md). Make every important finding evidence-backed. Title it with the broken capability or contract; keep missing paths in evidence or affected files, not as the root problem.
5. Inspect source code or tests only to verify a specific harness capability or contract. Do not traverse implementation or test suites generally to search for additional findings.
6. In static mode, separate observed repository facts from inferred runtime impact. Use conditional language such as `may`, `could`, `appears`, or `if` for consequences that were not executed or otherwise proven. Do not state that a deployment or runtime command `fails` without execution or stronger proof.
7. Recommend only the smallest corrections supported by evidence. Describe the desired harness capability or contract; do not prescribe application refactors merely because they are common code-quality practices. Keep scores approximate and calibrate severity to evidence confidence; static inference alone is normally insufficient for `CRITICAL`.
8. Stop discovery for a dimension once it can be scored confidently and its evidence, owner, and smallest useful correction are known. Continue only when the next evidence could change the decision, resolve a contradiction, or address a high-risk uncertainty.
9. Report scores, prioritized findings, recommendations, runtime commands and outcomes if any, unresolved uncertainty, and intentionally skipped work.

## Audit output

Use this shape:

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

Do not turn an audit into a generic code review, a file-presence checklist, or an unrequested implementation.
