# Behavioral Scenarios

These scenarios test the observable decisions and outputs of the `harness:start`
and `harness:audit` skills in the `repo-harness` collection. They are repository
profiles, not exact-wording snapshots: a conforming implementation may use
different prose while preserving the required behavior.

## How to use them

For each scenario, give an agent the fixture profile and user request without
revealing the expected result. Review whether the agent:

1. discovers the highest-signal repository evidence before asking questions;
2. distinguishes harness concerns from source-code quality;
3. proposes only changes supported by evidence and the scenario constraints; and
4. stays within the requested `harness:start` or `harness:audit` responsibility.

The scenarios are conceptual. They do not require a test runner, fixture repository, CLI, MCP server, dashboard, or vendor integration.

## Scenario coverage

| Skill | Scenario | Primary behavior under test |
| --- | --- | --- |
| `harness:start` | `start/healthy-project.md` | Avoid unnecessary harness files and changes. |
| `harness:start` | `start/minimal-library.md` | Keep a small repository from being over-engineered or prescribed common tooling without evidence. |
| `harness:start` | `start/vendor-specific.md` | Reuse useful existing guidance instead of duplicating it blindly. |
| `harness:start` | `start/runtime-command-safety.md` | Classify canonical commands before executing side effects. |
| `harness:audit` | `audit/poor-harness.md` | Find harness gaps even when the source code is reasonable. |
| `harness:audit` | `audit/capability-over-file-presence.md` | Evaluate capabilities instead of treating missing expected files as findings. |
| `harness:audit` | `audit/missing-path-contract.md` | Name broken contracts rather than absent paths. |
| `harness:audit` | `audit/dependency-contract.md` | Keep dependency/config inconsistencies scoped to harness reproducibility. |
| `harness:audit` | `audit/static-runtime-inference.md` | Separate static facts from runtime inference and calibrate severity. |
| `harness:audit` | `audit/stale-docs.md` | Detect contradictions between documentation and executable configuration. |
| `harness:audit` | `audit/monorepo.md` | Separate root-level and workspace-level harness concerns. |

## Review rule

Mark a scenario as passing only when the agent's reasoning is grounded in the
supplied evidence. Do not award credit for arbitrary scores, invented repository
facts, generic code-review comments, or a file checklist that ignores capability.

Boundary checks are part of the scenarios: `harness:start` must not turn a setup
request into a seven-dimension audit, and `harness:audit` must not modify the
repository or implement its own findings.
