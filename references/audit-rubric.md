# Harness audit rubric

Score each dimension from 0–100 based on repository evidence. Use the bands below as descriptive guidance:

| Score | Band |
| --- | --- |
| 90–100 | Excellent |
| 75–89 | Strong |
| 60–74 | Functional |
| 40–59 | Weak |
| 0–39 | Critical |

Scores summarize confidence and capability; they are not scientifically precise. A low score requires evidence of a capability failure, not merely a missing filename.

## Evidence gate

Score capabilities, not expected files or common tools. Before lowering a score or writing a finding, check whether an equivalent capability exists elsewhere and whether it is discoverable, coherent, and usable. A finding must name a missing, fragmented, contradictory, or hard-to-discover capability and cite the evidence for that failure.

## 1. Context

**Purpose:** Determine whether an unfamiliar contributor can understand what the project does and its important domain concepts.

**Ask:** Is purpose clear? Are core concepts and broad system behavior explained? Can the contributor tell what is in scope?

**Strong signals:** Accurate project overview, domain vocabulary, broad architecture or system-flow context, links to deeper sources.

**Weak signals:** Product description without concepts, stale architecture claims, or knowledge available only from source-code archaeology.

**Common failure:** Reporting code complexity instead of explaining that the repository lacks usable context for a safe change.

**Scoring:** Score high when purpose and concepts are discoverable and current; reduce for missing, contradictory, or tribal context.

## 2. Navigation

**Purpose:** Determine whether a contributor can locate the correct responsibility and change boundary.

**Ask:** Can someone find the relevant subsystem, entrypoint, tests, and deeper context? Are root and workspace ownership clear?

**Strong signals:** Repository map, stable directory responsibilities, links from root docs, workspace ownership notes.

**Weak signals:** Flat or misleading navigation, unexplained directories, broken links, or repeated rediscovery of where changes belong.

**Common failure:** Creating a new map without checking whether existing documentation already answers the question.

**Scoring:** Score high when a contributor can route a change quickly; reduce when navigation depends on guessing or broad scans.

## 3. Constraints

**Purpose:** Determine whether safety boundaries and invariants are discoverable before editing.

**Ask:** Are architectural rules, generated files, dependency boundaries, migrations, risky areas, and prohibited casual changes documented?

**Strong signals:** Explicit ownership, source-of-truth rules, generated-file instructions, migration guidance, enforced boundaries.

**Weak signals:** Hidden invariants, undocumented generated output, conflicting rules, or constraints known only by maintainers.

**Common failure:** Treating a rule as real without a path, configuration, or maintainer evidence.

**Scoring:** Score high when relevant constraints are current and actionable; reduce for each critical hidden or contradictory boundary.

## 4. Verification

**Purpose:** Determine whether a contributor can prove that a change works and meets repository expectations.

**Ask:** Are tests, lint, type checks, builds, integration checks, and a canonical verification path available? Does local verification match CI?

**Strong signals:** One discoverable command, focused checks, reproducible prerequisites, CI parity, clear completion evidence.

**Weak signals:** Tests exist but no full path, CI-only checks, stale commands, or no way to know when a change is complete.

**Common failure:** Saying “run tests” when CI also requires other checks.

**Scoring:** Score high when relevant checks are executable and aligned; reduce for missing, hidden, flaky, or contradictory verification.

## 5. Feedback Loops

**Purpose:** Determine whether development feedback is fast, useful, and reproducible.

**Ask:** Can a contributor run focused checks? Are failures actionable? Are local commands reproducible and reasonably aligned with CI?

**Strong signals:** Fast focused tests, static checks, stable local tasks, useful CI feedback, clear scope for expensive checks.

**Weak signals:** Only slow or opaque checks, duplicated command lists, late feedback, or local/CI drift.

**Common failure:** Adding tooling without evidence that existing feedback is insufficient.

**Scoring:** Score high when the repository supports a tight edit–check loop; reduce for avoidable latency, ambiguity, or drift. Do not reduce the score or prescribe linting, type checking, formatting, or other common tools merely because they are absent; require evidence of a real feedback-loop or verification gap.

## 6. Operational Understanding

**Purpose:** Determine whether the contributor can run, configure, and debug the project at the level its scope requires.

**Ask:** Are setup, services, environment variables, logs, common errors, and debugging paths documented when relevant?

**Strong signals:** Reproducible setup, service prerequisites, safe reset steps, useful logs/errors, scope-appropriate operations guidance.

**Weak signals:** Tribal configuration, unexplained secrets or ports, opaque startup failures, or operational documentation disproportionate to the project.

**Common failure:** Demanding production observability docs for a small library that has no meaningful runtime operations.

**Scoring:** Scale to the project. Score high when the required operational knowledge is usable; do not penalize irrelevant absent infrastructure.

## 7. Agent Readiness

**Purpose:** Determine whether a coding agent can answer the core questions needed for a safe, bounded change.

**Ask:** Can it determine what the project does, where the change belongs, which patterns and constraints apply, what to avoid, how to run and test the relevant part, how to verify completion, and where deeper context lives?

**Strong signals:** Discoverable instructions, explicit boundaries, executable verification, consistent sources, and low rediscovery cost.

**Weak signals:** Agent guidance exists but contradicts CI, vendor-specific duplicates obscure ownership, or critical decisions require human translation.

**Common failure:** Equating agent readiness with the presence of `AGENTS.md` or another named file.

**Scoring:** Score the end-to-end ability to make and verify a safe change; reduce for any high-impact unanswered core question.

## Overall interpretation

Present all seven scores. Include an overall score only when it helps summarize the evidence, and state how it was derived. Order the audit’s attention by impact and confidence, not by the lowest number alone.
