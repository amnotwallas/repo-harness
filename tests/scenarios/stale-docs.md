# Stale documentation

## Intent

Ensure `harness:audit` checks documentation against executable configuration and surfaces contradictions.

## Fixture profile

The repository contains:

- a README that says contributors use Node 20 and npm;
- a lockfile and package metadata for pnpm;
- CI that installs and runs the project with Node 22 and pnpm;
- agent guidance that still names the old `npm test` command;
- tests and a build command that otherwise work.

## User request

> Check whether this repo is agent-ready.

## Expected behavior

1. Select `harness:audit` from the natural-language request.
2. Inspect the README, agent guidance, package metadata/lockfile, scripts, and CI together.
3. Report the Node/package-manager/command mismatch as a contradiction with concrete evidence.
4. Explain how the contradiction can make an agent believe a change is complete when CI will reject it.
5. Recommend one source of truth and links or references rather than duplicating competing instructions.

## Must not happen

- Accept the README as authoritative without checking executable configuration.
- Report only that documentation is "outdated" without naming the conflicting sources.
- Propose a vendor-specific instruction file as the first fix.

## Pass criteria

The contradiction is identified as a harness finding with evidence from both documentation and executable configuration.

## Baseline observation without the skill

The baseline agent found the Node/npm versus Node/pnpm contradiction and proposed aligning README, agent guidance, package metadata, and CI. It still asked whether Node 22 plus pnpm was intended even though executable configuration already provided a strong canonical signal, and it added lockfile-pinning suggestions that were not required by the fixture.

## GREEN observation with the skill

The skill treated the request as a static audit, identified the complete contradiction, used the executable sources as the stronger signal for package-manager discovery, and retained only the unresolved Node-support policy as a question.
