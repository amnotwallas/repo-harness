# Healthy project

## Intent

Ensure `harness:start` recognizes an already coherent harness and does not manufacture work.

## Fixture profile

The repository contains:

- a README with project purpose, setup, navigation, and a canonical `make verify` command;
- a `Makefile` whose `verify` target runs the same tests, lint, type check, and build used by CI;
- focused tests and source-level documentation for the main subsystem;
- CI configuration that calls `make verify`;
- existing root agent guidance that matches the README and CI;
- no hidden architecture constraint discovered by inspecting the high-signal files.

## User request

> Set up the engineering harness for this repo.

## Expected behavior

1. Select `harness:start` from the natural-language request.
2. Inspect the high-signal files before asking anything.
3. Report that the harness is already sufficient, or identify only narrowly evidenced improvements.
4. Do not propose `AGENTS.md`, additional documentation, a second verification script, or a new tool merely because those files are absent.
5. If no useful change is justified, say so. Verify the existing canonical path when it can be performed safely; when runtime execution was not requested or cannot be made safe, use static inspection and state that runtime verification was not executed.

## Must not happen

- Treat missing files as gaps without showing a capability failure.
- Perform a generic code-quality review.
- Replace the existing verification path with a new convention.

## Pass criteria

The response preserves the existing harness and contains no unnecessary artifact proposal.

## Baseline observation without the skill

The baseline agent inspected the README, Makefile, CI, agent guidance, tests, and source documentation; it asked no redundant question, kept `make verify` as the single entry point, and proposed no new artifact. This is the desired control behavior.

## GREEN observation with the skill

The skill produced the same minimal result, explicitly skipped duplicate agent files and maps, reused `make verify`, and used static inspection because runtime execution was not requested.
