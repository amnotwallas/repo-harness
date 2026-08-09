# Capability over file presence

## Intent

Ensure `harness-audit` evaluates discoverable capabilities instead of treating an expected filename or directory as a requirement.

## Fixture profile

The repository contains:

- a README that explains the project purpose, setup, navigation, and completion path;
- `docs/architecture.md` covering system boundaries, ownership, and important constraints;
- `CONTRIBUTING.md` linking to the architecture and verification guidance;
- coherent tests and CI instructions that agree with those documents;
- no `docs/context/` directory and no `AGENTS.md`.

The existing documents are discoverable from the root and provide equivalent context through different paths.

## User request

> Audit this repository's harness and report any missing context files.

## Expected behavior

1. Use `harness-audit` in static mode.
2. Inspect the existing root documentation and links before judging the absent paths.
3. Do not report `docs/context/` or `AGENTS.md` as a harness gap merely because they are absent.
4. Evaluate Context and Navigation from the actual discoverability and usefulness of the existing guidance.
5. Only report a finding if a specific capability remains missing, fragmented, contradictory, or hard to discover, with evidence for that capability failure.
6. Do not modify the repository or implement any recommendation.

## Must not happen

- Treat a preferred filename or directory as mandatory.
- Recommend creating `docs/context/` or `AGENTS.md` without showing a capability failure.
- Score a dimension down solely because an expected file is missing.
- Switch into the mutating start workflow.

## Pass criteria

The audit recognizes the existing equivalent context, produces no file-presence finding, remains non-mutating, and grounds any other finding in a concrete capability failure.

## Baseline observation without the updated guard

The dogfooding audit treated the absent `docs/context/` directory as a gap even though the repository already exposed equivalent context elsewhere. This scenario prevents that file-first regression.

## GREEN observation with the updated guard

The skill checked the existing sources of truth, reported the context capability as available, made no recommendation based only on the absent paths, and left the repository unchanged.
