# Audit findings

## Required shape

Write every important finding using this structure:

```text
[SEVERITY] Title

Dimension:
<one of the seven audit dimensions>

Evidence:
<specific repository path, section, command, configuration, or observed runtime result>

Why it matters:
<impact on understanding, changing, or verifying the repository>

Recommended fix:
<smallest useful correction>

Affected files:
<paths when relevant>
```

## Title and root problem

Title the finding with the broken capability or contract, not the absent path that helped reveal it. A missing file or directory belongs in `Evidence` or `Affected files` only when it demonstrates a real capability failure.

Prefer:

```text
[HIGH] Agent instructions reference context files that do not exist
```

over:

```text
[HIGH] Missing docs/context/ directory
```

Use the repository’s actual paths and commands. For HIGH and CRITICAL findings, concrete evidence is mandatory. If evidence is incomplete, label the item as a hypothesis or open question instead of presenting it as a finding.

Keep dependency and configuration inconsistencies inside harness scope: report them when they make setup, deployment, verification, or another repository contract non-reproducible or unclear. Do not expand the finding into package-by-package dependency correctness, vulnerability, or modernization advice.

## Harness boundary

Describe the broken harness capability or contract and the desired outcome. Implementation details are evidence only when they demonstrate a specific failure of discoverability, reproducibility, safety, feedback, or verification. Do not turn code-quality observations—such as sleeps, import style, or architecture preferences—into findings or prescribe application refactors without that connection.

Recommendations should improve the repository's harness contract or its discoverability. When an implementation change is genuinely required to restore that contract, state the requirement and evidence rather than prescribing an unrelated refactor.

## Severity

| Severity | Use when |
| --- | --- |
| `CRITICAL` | Strong evidence shows a harness failure makes ordinary work fundamentally unsafe, unverifiable, or unreliable, or likely to damage important repository state. Static inference alone is normally insufficient. |
| `HIGH` | A missing, contradictory, or hidden capability is likely to block or mislead normal changes. |
| `MEDIUM` | A real gap creates recurring friction or avoidable uncertainty but has a safe workaround. |
| `LOW` | A bounded improvement would reduce minor friction without affecting the main workflow. |
| `INFO` | A useful observation or confirmed strength does not require corrective action. |

Do not inflate severity to make a recommendation persuasive. Explain the consequence instead.

## Evidence and uncertainty

- Prefer a direct path plus section, command, or configuration fact.
- Compare documentation with executable configuration when reporting drift.
- State what was inspected and what remains unknown when evidence is partial.
- Keep likely risks separate from confirmed findings.
- In a static audit, separate observed facts from inferred runtime impact. Use conditional language such as `may`, `appears`, `could`, or `if` unless the consequence was executed or otherwise proven. Do not say a deployment or runtime command `fails` as a confirmed fact without execution or stronger proof.
- In runtime mode, record the command and outcome; do not claim a check passed without running it.
- Do not infer a gap from a missing file when the capability is already available elsewhere.

## Prioritization

Order findings by the combination of impact, likelihood of misleading work, and confidence in the evidence. Prioritize:

1. unsafe or unverifiable changes;
2. contradictions between local instructions and CI or executable configuration;
3. hidden constraints, generated-file ownership, or migration boundaries;
4. blocked setup and missing fast feedback;
5. navigation and lower-impact documentation improvements.

Recommend one small correction per finding when possible. Link to or extend the existing source of truth before creating a new document. Keep a small repository’s findings small.
