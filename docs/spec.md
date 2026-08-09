# Repo Harness — v1 Spec

## Goal

Create a vendor-agnostic collection named `repo-harness` containing two focused Agent Skills for improving and auditing the engineering harness around a software repository:

* `start` for bootstrapping or improving a repository harness
* `audit` for evaluating an existing repository harness

`repo-harness` is the collection name, not a third coordinator skill.

The collection should work conceptually with:

* Codex
* Claude Code
* Google Antigravity
* Antigravity CLI
* other runtimes compatible with Agent Skills / `SKILL.md`

The core must not depend on any specific vendor.

---

## What is a harness?

For this project:

> A harness is the context, constraints, workflows, feedback loops, and verification infrastructure around a codebase that helps a human or coding agent understand, modify, and verify the project safely.

A harness may include:

* README and developer docs
* agent instructions
* repository navigation
* architecture context
* tests
* linting
* type checking
* builds
* CI
* local development commands
* debugging guidance
* verification scripts
* important constraints

No specific file is mandatory.

Evaluate capabilities, not file presence.

---

## Main workflows

v1 supports only these two skills:

```text
harness:start
harness:audit
```

These are the fully-qualified operations for the `harness` plugin namespace.
Runtimes without plugin namespacing may expose the secondary aliases
`harness-start` and `harness-audit`. Natural-language discovery is always valid.

The skill must also work from natural-language requests such as:

```text
Set up the engineering harness for this repository.

Make this repository easier for coding agents to work with.

Audit the engineering harness of this repository.

Check whether this repository is agent-ready.
```

Do not require actual slash-command registration.

---

# harness:start skill

Purpose:

> Bootstrap or improve the minimum useful harness for the current repository.

Flow:

```text
discover
→ identify gaps
→ ask only necessary questions
→ propose minimal changes
→ implement
→ verify
```

## 1. Discover first

Before asking questions, inspect:

* root structure
* languages/frameworks
* package/build manifests
* package manager
* README/docs
* tests
* lint/typecheck/build configuration
* CI
* scripts/task runners
* Docker/environment files
* existing agent instructions
* top-level source structure

Do not scan the entire repository unless needed.

Prefer high-signal files first.

## 2. Do not ask discoverable questions

Bad:

```text
What language is this project?
Do you use pytest?
Do you have CI?
```

if the repository already answers them.

Only ask questions about important unknowns that cannot be inferred.

Target:

```text
0-4 questions
```

for normal repositories.

Examples:

```text
Which command should define a valid change?

Which subsystem is especially risky to modify?

Is there an architectural rule not represented in the repository?
```

## 3. Detect harness gaps

Look for problems such as:

* no clear verification path
* local verification differs from CI
* important architecture constraints are hidden
* repository navigation is difficult
* development setup is incomplete
* existing instructions contradict each other
* critical workflows require tribal knowledge
* agents must repeatedly rediscover the same information

Do not treat missing files as problems by themselves.

Example:

```text
AGENTS.md missing
```

is not automatically a gap.

## 4. Propose the minimum harness

Before significant changes, explain what should be created or updated.

Example:

```text
CREATE AGENTS.md
Reason: no existing source explains repository navigation,
constraints, and completion requirements.

CREATE scripts/verify.sh
Reason: CI runs three checks but no equivalent local command exists.

UPDATE README.md
Reason: link the canonical verification command.

SKIP architecture.md
Reason: existing documentation already provides enough context.
```

Every proposed artifact must have a reason.

## 5. Preserve existing conventions

Prefer:

```text
discover → reuse → extend
```

over:

```text
replace → duplicate
```

Do not create:

```text
README.md
DEVELOPMENT.md
CONTRIBUTING.md
ARCHITECTURE.md
AGENTS.md
CLAUDE.md
GEMINI.md
```

all at once unless each solves a real separate problem.

## 6. Prefer executable knowledge

When possible, prefer one canonical verification command such as:

```text
make verify
just verify
npm run verify
./scripts/verify.sh
```

over documenting several manual commands in multiple places.

Reuse existing tools.

Do not introduce new tooling just to make the harness look complete.

## 7. Verify safely

After an authorized implementation, inspect and classify the canonical
verification command and its full chain before executing it.

Automatically execute only safe local validation or read-only inspection. Do
not execute deployments, publishing, cloud or shared-state mutations,
production data changes, destructive migrations, credentialed external
operations, `git push`, or commands with unclear effects automatically. Ask for
approval or report that runtime verification was not executed when a command is
unsafe or unclear. Use static inspection when runtime execution was not
requested or cannot be performed safely, and never claim skipped verification
passed.

The start workflow may modify the repository only when requested or authorized.
It does not produce the seven-dimension audit or implement audit findings as an
implicit follow-up.

---

# harness:audit skill

Purpose:

> Evaluate how safely and efficiently an unfamiliar human or coding agent can understand, modify, and verify the repository.

The audit is about harness quality, not general code quality.

Do not perform a generic code review.

The audit is non-mutating, not strictly command-free:

* static/read-only repository inspection is the default;
* it must not modify files, repository state, generated output, or commits;
* it may execute safe local validation only when the user explicitly requests
  runtime validation;
* it must classify the full command chain before execution and skip or ask for
  approval when the command is unsafe or unclear; and
* it never implements its own findings or recommendations. An explicitly
  requested implementation follow-up belongs to `harness:start`.

Bad:

```text
This function has high cyclomatic complexity.
```

Good:

```text
This critical subsystem has no documented or automated
verification path.
```

---

## Audit dimensions

Evaluate these seven dimensions:

### 1. Context

Can an unfamiliar contributor understand:

* what the project does
* important domain concepts
* how the system broadly works

### 2. Navigation

Can they determine:

* where a change belongs
* where important responsibilities live
* where deeper context can be found

### 3. Constraints

Can they discover:

* architectural rules
* invariants
* generated files
* dependency boundaries
* migration rules
* risky areas
* things that should not be changed casually

### 4. Verification

Can they prove a change works?

Check:

* tests
* lint
* type checking
* builds
* integration checks
* canonical verification
* local/CI parity

### 5. Feedback Loops

Does the repository provide fast and useful feedback while developing?

Examples:

* focused tests
* static checks
* reproducible local commands
* useful CI

### 6. Operational Understanding

Can they:

* run the project
* configure required services
* debug problems
* understand relevant logs/errors

Scale this dimension to the project.

A small library does not need production observability documentation.

### 7. Agent Readiness

Can a coding agent answer:

```text
What does this project do?

Where should this change happen?

What patterns should I follow?

What should I avoid?

How do I run the relevant part?

How do I test the change?

How do I verify completion?

Where can I find deeper context?
```

Agent readiness does not require `AGENTS.md`.

---

# Audit scoring

Score each dimension from:

```text
0-100
```

Suggested bands:

```text
90-100  Excellent
75-89   Strong
60-74   Functional
40-59   Weak
0-39    Critical
```

Example:

```text
REPO HARNESS AUDIT

Context                    82/100
Navigation                 74/100
Constraints                58/100
Verification               52/100
Feedback Loops             77/100
Operational Understanding  66/100
Agent Readiness            49/100

Overall                    65/100
```

Scores are summaries of evidence.

Do not pretend they are scientifically precise.

---

# Audit findings

Every important finding should contain:

```text
[SEVERITY] Title

Dimension:
<dimension>

Evidence:
<specific repository evidence>

Why it matters:
<impact>

Recommended fix:
<smallest useful correction>

Affected files:
<paths when relevant>
```

Severity levels:

```text
CRITICAL
HIGH
MEDIUM
LOW
INFO
```

Example:

```text
[HIGH] Local verification does not match CI

Dimension:
Verification

Evidence:
README.md instructs contributors to run `pytest`.

.github/workflows/ci.yml runs:
- ruff check .
- mypy src/
- pytest

Why it matters:
A contributor can believe a change is complete while CI will reject it.

Recommended fix:
Expose one canonical local verification command containing all
required checks.

Affected files:
README.md
.github/workflows/ci.yml
```

HIGH and CRITICAL findings must have concrete repository evidence.

Do not invent findings.

---

# Contradictions

The audit must actively look for contradictions such as:

```text
README says Node 20
CI uses Node 22
```

```text
README says npm
CI and lockfile use pnpm
```

```text
AGENTS.md says pytest
CI requires make verify
```

```text
architecture docs reference directories that no longer exist
```

Contradictions are often more important than missing documentation.

---

# Duplication

Detect duplicated instructions when they create ambiguity or drift.

Do not report duplication merely because the same concept appears twice.

Report it when multiple sources disagree or create unclear ownership.

Prefer:

```text
one source of truth
+
links/references
```

---

# Static and runtime audit

Support two conceptual modes.

## Static

Inspect repository files only. This is the default audit mode. Do not modify
files, repository state, generated output, or commits.

## Runtime

Only when the user explicitly requests runtime validation, inspect and classify
the complete command chain before execution. Run existing local validation
commands only when they are safe, such as:

* tests
* lint
* type checking
* build
* canonical verify command

Never automatically run:

* deployments
* production migrations
* destructive database commands
* `terraform apply`
* package publishing
* cloud mutations
* `git push`
* unknown destructive scripts

If a command is unsafe or unclear, ask for approval or report that verification
was not executed. Never claim skipped verification passed. Safe runtime checks
must not make the audit mutating, and the audit must not implement its findings;
an authorized implementation follow-up belongs to `harness:start`.

---

# Skill structure

Create:

```text
repo-harness/
├── .claude-plugin/
│   └── plugin.json
├── .codex-plugin/
│   └── plugin.json
├── gemini-extension.json
├── skills/
│   ├── start/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── discovery.md
│   │       └── harness-patterns.md
│   └── audit/
│       ├── SKILL.md
│       └── references/
│           ├── discovery.md
│           ├── audit-rubric.md
│           └── findings.md
├── tests/
│   ├── README.md
│   ├── start/
│   └── audit/
├── docs/
│   └── spec.md
├── README.md
└── LICENSE
```

The collection manifests use `harness` as the plugin identity:

* `.claude-plugin/plugin.json`
* `.codex-plugin/plugin.json`
* `gemini-extension.json`

Antigravity may materialize a root `plugin.json` during installation. That
generated file is not a behavioral source of truth and is not required in the
repository when the installer creates it.

Keep each `SKILL.md` concise and understandable without loading the unrelated
skill. Do not keep a root `SKILL.md` coordinator.

Move detailed guidance into the references for the skill that owns it. Duplicate
small critical invariants when that keeps a skill portable.

Do not create scripts or templates unless they become clearly necessary during implementation.

---

# Skill files

`skills/start/SKILL.md` uses:

```yaml
---
name: start
description: Use when bootstrapping, setting up, or improving the engineering harness around an existing software repository.
---
```

`skills/audit/SKILL.md` uses:

```yaml
---
name: audit
description: Use when auditing, evaluating, or diagnosing the engineering harness or agent readiness of a software repository.
---
```

Each `SKILL.md` should define its own:

* purpose
* core principles
* focused workflow
* when to load its references
* command safety
* completion criteria

`harness:start` must discover before asking, identify capability gaps, propose
minimum evidence-based changes, preserve conventions, modify only when
requested, and verify safely.

`harness:audit` must be static/read-only by default, non-mutating, safe-runtime
only on explicit request, evidence-based across all seven dimensions, and must
never implement its own findings. Critical command-safety behavior belongs
directly in both files, not only in references.

Core rules:

```text
1. Discover before asking.
2. Evidence before assumptions.
3. Evaluate capabilities, not files.
4. Prefer executable knowledge.
5. Preserve existing conventions.
6. Minimize harness surface area.
7. Do not perform general code review.
```

---

# References

## `skills/start/references/discovery.md` and `skills/audit/references/discovery.md`

Explain:

* repository discovery order
* high-signal files
* progressive exploration
* monorepo handling
* stop conditions
* avoiding exhaustive scans

The discovery reference is duplicated so each skill remains portable and
understandable from its own directory.

Core rule:

> Stop exploring a dimension once enough evidence exists to evaluate it
> confidently; continue only for missing, contradictory, or high-risk evidence.

## `skills/audit/references/audit-rubric.md`

For every audit dimension define:

* purpose
* questions
* strong signals
* weak signals
* common failure modes
* scoring guidance

Keep scoring capability-based.

## `skills/audit/references/findings.md`

Define:

* severity levels
* evidence requirements
* finding structure
* uncertainty language
* prioritization

## `skills/start/references/harness-patterns.md`

Document reusable patterns such as:

* Canonical Verify Command
* Repository Map
* Root Agent Guidance
* Module Context
* CI/Local Parity
* Single Source of Truth
* Progressive Documentation

For each pattern:

```text
Problem
When useful
Minimal implementation
When not to use
```

---

# Tests

Keep all behavioral regressions organized by owning skill:

```text
tests/
├── README.md
├── start/
│   ├── healthy-project.md
│   ├── minimal-library.md
│   ├── runtime-command-safety.md
│   └── vendor-specific.md
└── audit/
    ├── capability-over-file-presence.md
    ├── dependency-contract.md
    ├── missing-path-contract.md
    ├── monorepo.md
    ├── poor-harness.md
    ├── stale-docs.md
    └── static-runtime-inference.md
```

The tests should verify behavior, not exact wording. Keep `tests/README.md` as
the shared index.

Important start behaviors:

### Healthy project

`harness:start` verifies the resulting harness when safe verification can be
performed. Static inspection is acceptable when runtime execution was not
requested or cannot be performed safely. It does not propose unnecessary files.

### Minimal library

`harness:start` does not over-engineer a tiny repository or prescribe common
tooling without evidence of a real verification or feedback-loop gap.

### Vendor-specific

`harness:start` reuses useful existing instructions instead of blindly creating
duplicates or vendor-specific files.

### Runtime command safety

When a documented or canonical command includes an unsafe or external-side-effect
operation, `harness:start` detects and classifies it, does not execute it
automatically, and explains the skipped verification or asks for approval.

Important audit behaviors:

### Poor harness

`harness:audit` detects harness problems even if source code quality is
reasonable, while remaining non-mutating and not implementing recommendations.

### Capability over file presence

`harness:audit` does not report a missing file or directory by itself when an
equivalent capability exists elsewhere.

### Missing-path contract

When a path is absent, any finding names the broken capability or contract; the
path remains evidence or an affected file, not the root problem.

### Dependency contract

`harness:audit` keeps dependency/configuration inconsistencies within harness
scope, describing reproducibility or source-of-truth problems rather than
becoming a package correctness scanner.

### Static runtime inference

Without execution or other proof, `harness:audit` describes runtime impact
conditionally and calibrates severity to evidence confidence; static inference
alone is rarely `CRITICAL`.

### Stale docs

`harness:audit` detects contradictions between documentation and executable
configuration.

### Monorepo

`harness:audit` recognizes root-level and workspace-level harness concerns and
preserves their ownership.

Both skills stop exploring a dimension once enough evidence exists for a
confident decision and continue only when evidence is missing, contradictory,
or high-risk. Boundary checks ensure `harness:start` does not become the audit
scorecard and `harness:audit` does not mutate or implement findings.

---

# Anti-patterns

Both skills must avoid:

```text
Scanning every file.

Asking questions before inspecting the repo.

Treating missing AGENTS.md as automatically bad.

Creating documentation for every module.

Generating vendor-specific files by default.

Giving arbitrary scores without evidence.

Performing generic code review.

Creating tools that already exist in the repository.

Replacing useful existing harness infrastructure.

Treating the collection as a third coordinator skill.

Letting `harness:audit` implement its own findings.

Creating a CLI for v1.
```

---

# v1 acceptance criteria

The implementation is complete when:

* `skills/start/SKILL.md` exists and is vendor-neutral
* `skills/audit/SKILL.md` exists and is vendor-neutral
* no root coordinator `SKILL.md` exists
* `harness:start` behavior is clearly defined
* `harness:audit` behavior is clearly defined as non-mutating
* repository discovery happens before questions
* audit uses the seven dimensions
* findings require evidence
* the skill distinguishes harness quality from code quality
* existing conventions are preserved
* minimalism is explicitly enforced
* static audits are the default and safe runtime validation requires explicit request and command classification
* behavioral regression scenarios are preserved and organized under `tests/start/` and `tests/audit/`
* no CLI, MCP server, dashboard, vendor-specific behavior, or runtime adapter has been added

---

# Final principle

> Repo Harness should help an unfamiliar human or coding agent make the correct change with less rediscovery, less ambiguity, faster feedback, and stronger evidence that the change is complete.
