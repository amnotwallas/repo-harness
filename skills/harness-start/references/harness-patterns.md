# Harness patterns

Use these patterns as options, not as a checklist. Select a pattern only when repository evidence shows that it solves a real capability gap.

## Canonical Verify Command

**Problem:** Contributors and CI run different or incomplete sets of checks.

**When useful:** Tests, lint, type checks, builds, or integration checks are scattered or CI-only.

**Minimal implementation:** Reuse an existing task runner or command as the canonical entry point; make it invoke the relevant checks and have CI call the same path. Document focused commands separately when they serve local iteration.

**When not to use:** An existing command already matches CI, or the repository is small enough that its current single check is the complete contract.

## Repository Map

**Problem:** An unfamiliar contributor cannot determine where responsibilities and change boundaries live.

**When useful:** Top-level directories, packages, or workspaces have meaningful ownership that is not discoverable from existing docs.

**Minimal implementation:** Add a short map to the existing root documentation, linking to deeper context only where necessary. Cover only stable responsibilities and entrypoints.

**When not to use:** README or workspace docs already route changes clearly, or a new map would duplicate a maintained source.

## Root Agent Guidance

**Problem:** Agents repeatedly miss repository-specific constraints or completion steps.

**When useful:** Existing discoverable documentation does not answer where to change, what to avoid, or how to verify.

**Minimal implementation:** Extend the existing source of truth or add one concise root entrypoint that links to it. Include navigation, constraints, relevant commands, and definition of done; avoid runtime-specific copies.

**When not to use:** A useful `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, contributor guide, or README already provides the capability. Do not create a file just to satisfy a filename convention.

## Module Context

**Problem:** A critical subsystem has no local explanation of responsibilities, invariants, or focused verification.

**When useful:** Root guidance is insufficient and contributors repeatedly need tribal knowledge for one module or workspace.

**Minimal implementation:** Add a small context note at the module’s existing documentation boundary, covering purpose, ownership, constraints, and focused checks. Link it from the root or parent source.

**When not to use:** The module is ordinary, self-explanatory, or already documented by a maintained source.

## CI/Local Parity

**Problem:** A contributor can pass local checks while CI rejects the change, or cannot reproduce a CI failure.

**When useful:** CI duplicates commands, uses different versions/package managers, or runs checks with no local equivalent.

**Minimal implementation:** Align documentation and existing tasks with the executable CI contract; prefer CI invoking the canonical local command. Record required versions and prerequisites in one source.

**When not to use:** CI and local verification already share a clear, reproducible contract.

## Single Source of Truth

**Problem:** Multiple documents or commands describe the same workflow and drift apart.

**When useful:** README, agent guidance, CI, and workspace docs disagree or duplicate ownership.

**Minimal implementation:** Choose the source closest to the executable authority, make it canonical, remove or correct conflicting copies, and link from secondary entrypoints.

**When not to use:** Repetition is intentionally scoped, accurate, and linked to one maintained owner.

## Progressive Documentation

**Problem:** Important context is either missing or too large for an agent to use efficiently.

**When useful:** The repository has multiple levels of context, such as root, workspace, and module concerns.

**Minimal implementation:** Keep a concise root orientation and link to deeper documents only when a task needs them. Place each rule at the narrowest stable ownership boundary.

**When not to use:** A small repository is already clear in one short document, or splitting it would add navigation cost.
