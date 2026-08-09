# Repository discovery

Discover enough context to evaluate the repository harness confidently. Prefer a short, high-signal pass over an exhaustive scan.

## Discovery order

Inspect in this order, stopping when the evidence is sufficient:

1. Root structure and top-level source directories.
2. README and developer documentation.
3. Language, framework, package, build, and workspace manifests.
4. Lockfiles and package-manager or runtime-version declarations.
5. Tests, lint/type-check/build configuration, and task-runner definitions.
6. CI configuration and its exact commands.
7. Docker/environment files and local-service setup.
8. Existing agent or contributor instructions.
9. Top-level source structure and, only when needed, a representative module for a specific harness question.

Look for high-signal names such as `README`, manifests, lockfiles, `Makefile`, task-runner files, CI workflows, test/lint/type-check/build configuration, environment examples, and root instruction files. Adapt the names to the repository rather than assuming one ecosystem.

## Protected files and bounded content reads

Classify paths before reading file contents. Treat `.env`, local auth or credential files, private keys, and other secret-bearing files as protected by default. You may observe that a protected path exists, inspect its ignore rules or references, and read safe templates such as `.env.example`; do not open, print, grep, parse, or summarize protected contents unless the user explicitly requests it. Keep any explicitly requested inspection minimal and redact secret values.

Avoid recursive searches and broad globs that could include protected files. Narrow the path set before reading or executing a discovery command.

## Progressive exploration

Start at the root. Follow links and references only when they answer a current question about purpose, navigation, constraints, operations, or verification. Inspect a source or test file only when it verifies a specific harness contract that the higher-level evidence did not establish; do not read implementation or test suites generally.

Stop exploring once enough evidence exists to identify the relevant capability, its current owner, and the smallest useful correction—or to conclude that no correction is justified.

### Dimension-level stop conditions

Evaluate each relevant harness dimension independently. Stop exploring a dimension once its evidence is sufficient to evaluate the capability confidently and identify its source of truth or current owner. Continue deeper only when the evidence for that dimension is missing, contradictory, or high-risk to act on without more context.

Before expanding a dimension's scan, ask whether the next file can change the decision, ownership, or recommended correction. If not, stop. Do not keep scanning to confirm the absence of a preferred filename, directory, or common tool.

A seven-dimension scorecard does not require exhaustive source or unit-test traversal. Once the relevant documentation, configuration, and command ownership are clear, stop that dimension even if more implementation examples are available.

## Monorepos and workspaces

Identify the root manifest and workspace boundaries first. Then inspect:

- the root verification and CI contract;
- each workspace’s README or local instructions when ownership differs;
- representative app, service, and shared-package entrypoints;
- workspace-local test or integration commands;
- root and local constraints, including which one owns each rule.

Keep root-level and workspace-level concerns separate. Preserve local ownership when a rule or command applies to one workspace only. Do not flatten the repository into one document or demand one command when valid scopes differ.

Inspect workspace entrypoints or tests only when needed to verify a named harness contract. Do not descend into application implementation or unit-test collections after root and workspace evidence is sufficient.

## Questions after discovery

Ask only about important unknowns that repository evidence cannot resolve. Good questions establish a decision, such as:

- Which existing command should define a valid change?
- Which subsystem or workflow is especially risky to modify?
- Is there an architectural rule that is intentionally not represented in the repository?

Do not ask for facts already present in a manifest, lockfile, script, CI file, README, or existing instruction. For a normal repository, target 0–4 questions.

## Evidence to capture

For each possible gap or finding, record:

- the capability affected;
- the exact file, section, command, or configuration evidence;
- the current owner or source of truth;
- the smallest correction that would make the capability discoverable or executable;
- uncertainty that still needs a question or runtime check.

Missing files or directories are leads, not findings. First check whether an equivalent capability exists elsewhere and is discoverable. Record a gap or finding only when the absence causes a specific discoverability, consistency, safety, or verification failure, and describe that capability failure with evidence rather than naming the missing path alone.
