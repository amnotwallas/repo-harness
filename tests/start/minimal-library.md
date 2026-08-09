# Minimal library

## Intent

Ensure `harness-start` scales recommendations to a tiny repository and avoids over-engineering.

## Fixture profile

The repository is a small single-package library with:

- a concise README containing installation, API usage, and one test command;
- a package manifest with the test script and lockfile;
- a small focused test suite;
- no separate lint, type-check, or formatting tooling, while the existing tests are the validation needed for the stated project scope;
- no services, deployment, generated code, or meaningful runtime operations;
- no CI yet, but local tests are the only validation needed for the stated project size and scope.

## User request

> Make this repository easier for coding agents to work with.

## Expected behavior

1. Use `harness-start`.
2. Discover the package manifest, README, tests, and available command before asking questions.
3. Acknowledge that the existing small harness may already be adequate.
4. If proposing an improvement, keep it proportional, such as a short navigation or completion note in the existing README.
5. Do not treat missing linting, type checking, formatting, or other common tooling as a finding by itself; recommend it only if repository evidence shows a verification or feedback-loop gap.
6. Do not invent operational documentation, architecture files, CI, dashboards, or multiple agent instruction files without evidence that they solve a real problem.
7. Do not emit the audit scorecard or implement audit recommendations while handling this start request.

## Must not happen

- Apply an enterprise harness template to the library.
- Treat the absence of CI, services, or observability docs as automatically deficient.
- Prescribe linting, type checking, formatting, or other common tools merely because they are absent.
- Add tools or commands that duplicate the package manager's existing test command.
- Turn the start request into a generic code review or audit.

## Pass criteria

The recommendation is minimal, capability-based, and proportionate to a single-package library. It does not prescribe common tooling without evidence of a real verification or feedback-loop gap.

## Baseline observation without the skill

The baseline agent correctly avoided services and CI, but it still proposed a new `AGENTS.md`, linting, and formatting by default. The skill must explicitly prevent file-first and best-practice-tooling responses for a small repository.

## GREEN observation with the skill

The skill skipped `AGENTS.md`, CI, services, linting, type checking, formatting, and new verification tooling, proposed only a brief README workflow note if inspection showed the current README did not already provide it, and emitted no audit scorecard.
