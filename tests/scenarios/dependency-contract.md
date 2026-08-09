# Dependency contract

## Intent

Ensure dependency and configuration inconsistencies remain harness findings about reproducibility and source of truth, not package-correctness scanning.

## Fixture profile

The repository contains:

- a `pyproject.toml` with the declared application dependencies;
- a separate `requirements.txt` with a different dependency set;
- deployment configuration that installs from `requirements.txt`;
- local setup guidance that points contributors to `pyproject.toml`;
- no executed deployment or runtime check in the audit request.

## User request

> Audit this repository's harness, including its deployment setup.

## Expected behavior

1. Select `harness:audit` in static mode.
2. Compare the dependency sources and deployment configuration as a reproducibility contract.
3. Report a harness finding such as `Dependency sources of truth are inconsistent` or `Deployment dependency contract is not reproducible`.
4. Cite the conflicting files and explain the effect on setup, deployment, or verification.
5. Keep the finding scoped to source-of-truth consistency; do not scan or adjudicate every package version as a dependency correctness review.

## Must not happen

- Turn the audit into a package vulnerability or dependency correctness scanner.
- Report an arbitrary list of “missing” packages without connecting it to the repository's setup or deployment contract.
- Claim that deployment definitely fails without execution or stronger evidence.

## Pass criteria

The audit identifies the inconsistent dependency contract, keeps the analysis inside harness scope, and recommends one smallest source-of-truth correction.

## Baseline observation without the updated guard

The dogfooding audit treated the disagreement as a dependency correctness problem instead of identifying the reproducibility risk created by competing sources of truth.

## GREEN observation with the updated guard

The skill reported the inconsistent dependency sources as a harness contract finding, cited the deployment and local setup paths, and avoided package-by-package correctness claims.
