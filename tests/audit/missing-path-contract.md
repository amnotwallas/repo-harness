# Missing path contract

## Intent

Ensure a missing path is used as evidence for a broken contract, while the finding title names the capability failure rather than the absent path.

## Fixture profile

The repository contains:

- an `AGENTS.md` that instructs contributors and agents to read `docs/context/architecture.md` and `docs/context/verification.md`;
- neither referenced context file exists;
- the README does not provide equivalent links or content for those instructions;
- the project has tests and CI, but the agent-facing context contract is incomplete.

## User request

> Audit this repository's harness and report any broken documentation contracts.

## Expected behavior

1. Use `harness-audit` in static mode.
2. Inspect the instruction references and verify the referenced paths.
3. Report a finding whose title describes the broken capability, such as `Agent instructions reference context files that do not exist`.
4. Use the absent paths as evidence or affected files, not as the finding title or root problem.
5. Explain how the broken contract makes relevant context undiscoverable and recommend the smallest correction.
6. Do not modify the repository or implement the recommendation.

## Must not happen

- Title the finding `Missing docs/context/ directory`.
- Treat the directory name as the root problem without describing the broken reference contract.
- Claim that the application or deployment fails based only on these missing documentation paths.
- Switch into the mutating start workflow.

## Pass criteria

The finding names the broken agent-context contract, cites the missing referenced paths as evidence, uses a severity proportional to the harness impact, and leaves the repository unchanged.

## Baseline observation without the updated guard

The dogfooding audit reported `Missing docs/context/ directory`, making the absent path—not the broken instruction contract—the apparent problem.

## GREEN observation with the updated guard

The skill reported that agent instructions reference context files that do not exist, kept the paths in the evidence, explained the resulting discoverability failure, and did not modify files.
