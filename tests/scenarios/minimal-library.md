# Minimal library

## Intent

Ensure the skill scales harness recommendations to a tiny repository and avoids over-engineering.

## Fixture profile

The repository is a small single-package library with:

- a concise README containing installation, API usage, and one test command;
- a package manifest with the test script and lockfile;
- a small focused test suite;
- no services, deployment, generated code, or meaningful runtime operations;
- no CI yet, but local tests are the only validation needed for the stated project size and scope.

## User request

> Make this repository easier for coding agents to work with.

## Expected behavior

1. Select `harness:start`.
2. Discover the package manifest, README, tests, and available command before asking questions.
3. Acknowledge that the existing small harness may already be adequate.
4. If proposing an improvement, keep it proportional, such as a short navigation or completion note in the existing README.
5. Do not invent operational documentation, architecture files, CI, dashboards, or multiple agent instruction files without evidence that they solve a real problem.

## Must not happen

- Apply an enterprise harness template to the library.
- Treat the absence of CI, services, or observability docs as automatically deficient.
- Add tools or commands that duplicate the package manager's existing test command.

## Pass criteria

The recommendation is minimal, capability-based, and proportionate to a single-package library.

## Baseline observation without the skill

The baseline agent correctly avoided services, CI, and tooling, but it still proposed a new `AGENTS.md` by default and treated that filename as an optional normalization point. The skill must explicitly prevent a file-first response for a small repository.

## GREEN observation with the skill

The skill skipped `AGENTS.md`, CI, services, and new verification tooling, and proposed only a brief README workflow note if inspection showed the current README did not already provide it.
