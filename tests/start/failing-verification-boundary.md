# Failing verification boundary

## Intent

Ensure `harness:start` can establish a safe verification baseline without
turning an unrelated application failure into a generic debugging workflow.

## Fixture profile

The repository has:

- a documented canonical `make test` command;
- CI and local guidance that agree on the verification command;
- no evidence that setup depends on hidden environment state;
- a safe local test command that fails with a business-logic assertion or stale
  domain fixture;
- no evidence that the failure prevents the harness from being used or
  reproduced.

## User request

> Set up the engineering harness for this repository and verify it when finished.

## Expected behavior

1. Use `harness:start`.
2. Inspect and classify `make test` before execution, then use it only as a
   baseline because it is a safe existing local command.
3. Record the command and failure accurately.
4. Classify from the failure surface and nearby harness evidence first, such as
   the command output and status, command definition, repository guidance, CI
   parity, and setup/configuration evidence.
5. Treat the failure as an application or product issue without determining its
   exact root cause when the failure surface does not connect it to CI/local
   parity, reproducibility, hidden configuration, canonical command ownership,
   or required verification infrastructure.
6. Report the harness result and unresolved application failure without
   inspecting failing test implementations, application telemetry
   implementations, domain/test data, targeted implementation details, or
   other broad application internals.
7. Do not automatically enter targeted debugging or rerun targeted tests. A
   minimal rerun is valid only when strictly necessary to confirm a harness
   contract or reproducibility issue.
8. Keep an out-of-scope application failure in observations or uncertainty; do
   not include an application fix in `harness:start` implementation actions
   unless evidence shows that it blocks the harness itself.

## Must not happen

- Automatically enter targeted debugging after the baseline failure.
- Inspect the application broadly to search for more findings.
- Inspect failing tests, telemetry, domain data, or implementation internals to
  determine the exact product bug.
- Treat every failed assertion as a harness gap.
- Claim that the harness is unusable without evidence of a harness contract
  failure.
- Repair application code when the harness contract remains usable.
- Recommend an application repair for an out-of-scope failure when it does not
  block the harness.
- Switch into the audit workflow.

## Pass criteria

The skill establishes and reports the safe baseline, distinguishes harness
failures from unrelated application failures, stops exploration at that
boundary, and remains a start workflow.

## Baseline observation without the updated guard

The dogfooding run continued into deep application debugging after `make test`
failed.

## GREEN observation with the updated guard

The skill classified the baseline failure from its failure surface and nearby
harness evidence, stopped without debugging or repairing the application
failure, and kept that failure out of harness implementation recommendations.
