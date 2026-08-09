# Vendor-specific existing guidance

## Intent

Ensure the skill remains vendor-neutral while reusing useful instructions that already exist for a particular runtime.

## Fixture profile

The repository has a useful `CLAUDE.md` that documents source navigation, a risky migration boundary, local setup, and the canonical verification command. There is no `AGENTS.md`, but Codex and other Agent Skills-compatible runtimes can read the information through the repository's documented links. The existing file agrees with CI and the README.

## User request

> Make this repository easier for coding agents to work with.

## Expected behavior

1. Select `harness:start`.
2. Discover and evaluate the existing `CLAUDE.md` as useful repository guidance.
3. Reuse, extend, or link the existing source of truth only if a demonstrated capability gap remains.
4. Explain why a new `AGENTS.md` or runtime-specific duplicate is unnecessary when the current guidance is discoverable and coherent.
5. Keep recommendations expressed in runtime-neutral terms.

## Must not happen

- Create vendor-specific files by default.
- Delete or replace useful existing guidance merely to normalize filenames.
- Claim that a missing `AGENTS.md` is itself a harness failure.

## Pass criteria

The response preserves the useful `CLAUDE.md`, avoids duplicate instructions, and describes any change as a repository capability improvement rather than a runtime preference.

## Baseline observation without the skill

The baseline agent recognized `CLAUDE.md` as useful and wanted one source of truth, but it still asked whether to add a compatibility `AGENTS.md` and recommended that duplicate entrypoint. The skill must explicitly require capability-based reuse before adding any runtime- or vendor-shaped file.

## GREEN observation with the skill

The skill reused `CLAUDE.md`, proposed no repository changes, skipped `AGENTS.md`, asked no question, and stated that link discoverability and runtime verification remained unverified.
