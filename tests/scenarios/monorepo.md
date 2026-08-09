# Monorepo

## Intent

Ensure the skill discovers a monorepo's root and workspace harness concerns without scanning every file or flattening local ownership.

## Fixture profile

The repository has:

- a root workspace manifest listing `apps/web`, `services/api`, and `packages/shared`;
- root CI and a root verification command for workspace-wide checks;
- an app-level test command and a service-level integration command;
- workspace-local READMEs that explain responsibilities and setup differences;
- one shared architectural rule documented at the root and one service-specific migration rule documented under `services/api`.

## User request

> Audit this repository's harness.

## Expected behavior

1. Select `harness:audit`.
2. Discover the root manifest, root CI/verification path, workspace boundaries, and representative workspace guidance first.
3. Evaluate root-level and workspace-level navigation, constraints, setup, and verification separately where ownership differs.
4. Check parity between root CI and workspace commands before proposing changes.
5. Recommend the smallest correction at the correct ownership level, preserving workspace-local context.
6. Stop exploring once the evidence supports a confident audit; do not scan every package/file.

## Must not happen

- Treat the repository as a single flat application.
- Duplicate a workspace-specific rule at the root without reason.
- Demand one universal command when the existing root and workspace checks have distinct valid scopes.
- Report unsupported gaps from uninspected workspaces.

## Pass criteria

The audit distinguishes root and workspace harness responsibilities, cites the relevant boundaries, and remains progressive rather than exhaustive.

## Baseline observation without the skill

The baseline agent treated the request as an audit, inspected root/workspace boundaries, and kept possible findings provisional until evidence was checked. It proposed a broad root command contract and enforcement work, so the skill must keep monorepo recommendations scoped to the smallest evidenced ownership level.

## GREEN observation with the skill

The skill kept the root manifest, root checks, workspace commands, and local rules distinct; marked coverage and scope concerns as hypotheses; and recommended links or local corrections instead of a new monorepo-wide guide.
