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

Use the repository’s actual paths and commands. For HIGH and CRITICAL findings, concrete evidence is mandatory. If evidence is incomplete, label the item as a hypothesis or open question instead of presenting it as a finding.

## Severity

| Severity | Use when |
| --- | --- |
| `CRITICAL` | A harness failure can make ordinary work unsafe, unverifiable, or likely to damage important repository state. |
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
