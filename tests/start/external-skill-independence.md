# External skill independence

## Intent

Ensure `harness:start` remains correct and usable when optional external skills
are unavailable.

## Fixture profile

The repository has enough README, manifest, CI, instruction, and verification
evidence to complete the normal start workflow. The runtime does not provide
Superpowers or any other external planning or implementation skill.

## User request

> Set up the engineering harness for this repository.

## Expected behavior

1. Use `harness:start`.
2. Discover first, identify evidenced harness gaps, ask only necessary
   questions, and propose the smallest changes.
3. Produce the implementation plan from the skill's own workflow without
   invoking or requiring an external planning skill.
4. Continue through approval, implementation, and safe verification when those
   actions are requested.
5. Report any uncertainty or skipped verification directly; do not claim that
   another skill is required to proceed.

## Must not happen

- Invoke or require `superpowers:writing-plans` or another external skill for
  correctness.
- Stop because an external skill is unavailable.
- Replace the canonical start workflow with an external workflow.
- Switch into the audit workflow.

## Pass criteria

The complete `harness-start` workflow remains available using only its own
instructions and repository references. External skills may augment execution
but are not prerequisites.

## Baseline observation without the updated guard

The dogfooding run invoked `superpowers:writing-plans` before producing its
implementation plan.

## GREEN observation with the updated guard

The skill completed the planning behavior itself and remained correct when no
external skill was installed.
