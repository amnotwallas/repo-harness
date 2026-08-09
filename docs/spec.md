# Repo Harness — v1 Spec

## Goal

Create a vendor-agnostic Agent Skill called `repo-harness` for improving and auditing the engineering harness around a software repository.

The skill should work conceptually with:

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

v1 supports only:

```text
harness:start
harness:audit
```

These are logical intents.

The skill must also work from natural-language requests such as:

```text
Set up the engineering harness for this repo.

Make this repository easier for coding agents to work with.

Audit this repository's harness.

Check whether this repo is agent-ready.
```

Do not require actual slash-command registration.

---

# harness:start

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

---

# harness:audit

Purpose:

> Evaluate how safely and efficiently an unfamiliar human or coding agent can understand, modify, and verify the repository.

The audit is about harness quality, not general code quality.

Do not perform a generic code review.

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

Only inspect repository files.

Do not execute project commands.

## Runtime

When requested and safe, run existing local validation commands such as:

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

---

# Skill structure

Create:

```text
repo-harness/
├── SKILL.md
├── README.md
├── LICENSE
├── references/
│   ├── discovery.md
│   ├── audit-rubric.md
│   ├── findings.md
│   └── harness-patterns.md
└── tests/
    ├── README.md
    └── scenarios/
```

Keep `SKILL.md` concise.

Move detailed guidance into `references/`.

Do not create scripts or templates unless they become clearly necessary during implementation.

---

# SKILL.md

Recommended frontmatter:

```yaml
---
name: repo-harness
description: Use when setting up, evaluating, improving, or diagnosing the development and agent-readiness infrastructure around a software repository.
---
```

`SKILL.md` should define:

* purpose
* workflow selection
* core principles
* `start` workflow
* `audit` workflow
* when to load each reference
* command safety
* completion criteria

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

## references/discovery.md

Explain:

* repository discovery order
* high-signal files
* progressive exploration
* monorepo handling
* stop conditions
* avoiding exhaustive scans

Core rule:

> Stop exploring once enough evidence exists to make the harness decision confidently.

## references/audit-rubric.md

For every audit dimension define:

* purpose
* questions
* strong signals
* weak signals
* common failure modes
* scoring guidance

Keep scoring capability-based.

## references/findings.md

Define:

* severity levels
* evidence requirements
* finding structure
* uncertainty language
* prioritization

## references/harness-patterns.md

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

Create behavioral scenarios for at least:

```text
healthy-project
poor-harness
stale-docs
vendor-specific
monorepo
minimal-library
```

The tests should verify behavior, not exact wording.

Important expected behaviors:

### Healthy project

Do not propose unnecessary files.

### Poor harness

Detect harness problems even if source code quality is reasonable.

### Stale docs

Detect contradictions between documentation and executable configuration.

### Vendor-specific

Reuse useful existing instructions instead of blindly creating duplicates.

### Monorepo

Recognize root-level and workspace-level harness concerns.

### Minimal library

Do not over-engineer a tiny repository.

---

# Anti-patterns

The skill must avoid:

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

Creating a CLI for v1.
```

---

# v1 acceptance criteria

The implementation is complete when:

* `SKILL.md` exists and is vendor-neutral
* `harness:start` behavior is clearly defined
* `harness:audit` behavior is clearly defined
* repository discovery happens before questions
* audit uses the seven dimensions
* findings require evidence
* the skill distinguishes harness quality from code quality
* existing conventions are preserved
* minimalism is explicitly enforced
* static and safe-runtime audits are supported conceptually
* behavioral test scenarios exist
* no CLI, MCP server, dashboard, or vendor plugin has been added

---

# Final principle

> Repo Harness should help an unfamiliar human or coding agent make the correct change with less rediscovery, less ambiguity, faster feedback, and stronger evidence that the change is complete.
