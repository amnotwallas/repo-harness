# Poor harness

## Intent

Ensure `harness:audit` identifies harness weaknesses without confusing them with source-code quality.

## Fixture profile

The repository's implementation is reasonably tested and the code is maintainable, but:

- the README explains the product but not where responsibilities live;
- CI runs tests, lint, and type checking separately, while no local command exposes the full set;
- a generated directory and a migration-sensitive module are not identified anywhere;
- local setup requires a service whose configuration is only tribal knowledge;
- the current agent guidance says to run tests but omits the type check and build constraints.

## User request

> Audit this repository's harness and tell me whether it is agent-ready.

## Expected behavior

1. Select `harness:audit`.
2. Evaluate all seven dimensions: Context, Navigation, Constraints, Verification, Feedback Loops, Operational Understanding, and Agent Readiness.
3. Explain that the source code may be healthy while the harness is weak.
4. Produce important findings with severity, dimension, repository evidence, impact, smallest useful fix, and affected paths where relevant.
5. Prioritize the missing canonical verification path, hidden constraints, and undocumented setup knowledge over cosmetic documentation.
6. Treat scores as evidence-based summaries rather than scientific measurements.

## Must not happen

- Give a generic code review.
- Infer defects that the fixture does not support.
- Score dimensions without evidence.
- Recommend a full documentation rewrite when a smaller correction solves the gap.

## Pass criteria

The output is a seven-dimension harness audit with evidence-backed findings and no invented source-code criticism.

## Baseline observation without the skill

The baseline agent correctly separated harness quality from source quality and proposed fixes for navigation, CI/local parity, generated files, migrations, service setup, and agent guidance. It also asked three questions about undocumented operational details. The main risk was breadth: it proposed a large documentation bundle before establishing the smallest artifact for each gap.

## GREEN observation with the skill

The skill emitted a seven-dimension audit with approximate scores, evidence-backed HIGH findings, prioritized smallest corrections, and explicit uncertainty for paths and commands that were not supplied. It did not invent source-code defects or add vendor-specific artifacts.
