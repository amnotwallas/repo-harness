# Static runtime inference

## Intent

Ensure static audits distinguish proven repository facts from conditional runtime impact and calibrate severity to evidence confidence.

## Fixture profile

The repository contains:

- a Vercel deployment configuration that installs from `requirements.txt`;
- application code importing a package declared in `pyproject.toml` but not visibly declared in `requirements.txt`;
- no executed build, deployment, or startup check;
- no stronger evidence that the deployment actually reaches startup or fails there.

## User request

> Audit this repository's deployment harness without running deployment commands.

## Expected behavior

1. Use `harness-audit` in static mode and do not execute deployment commands.
2. State the repository facts directly and label runtime impact conditionally, for example: `Deployments that install from requirements.txt may fail because required imports appear absent from that dependency source.`
3. Use uncertainty language such as `may`, `appears`, `could`, or `if` when describing an unexecuted runtime consequence.
4. Do not use `CRITICAL` for static inference alone; reserve it for strong evidence that a harness failure fundamentally makes safe or reliable changes unreliable.
5. Recommend the smallest contract/source-of-truth correction without claiming that the deployment has already failed.
6. Do not modify the repository or implement the recommendation.

## Must not happen

- State `Vercel will crash on startup` as a confirmed fact.
- Claim that deployment failed without executing it or citing stronger proof.
- Assign `CRITICAL` solely because a static dependency inference sounds severe.
- Switch into the mutating start workflow.

## Pass criteria

The audit separates facts from inference, uses conditional runtime language, calibrates severity to evidence confidence, and remains non-mutating.

## Baseline observation without the updated guard

The dogfooding audit stated a runtime crash as fact and assigned `CRITICAL` even though it had only static evidence.

## GREEN observation with the updated guard

The skill described a conditional deployment risk, recorded the unexecuted runtime check as uncertainty, used a lower or explicitly provisional severity unless stronger evidence was supplied, and did not modify files.
