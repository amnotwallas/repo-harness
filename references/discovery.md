# Repository discovery

Discover enough context to make a harness decision confidently. Prefer a short, high-signal pass over an exhaustive scan.

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
9. Top-level source structure and representative module context.

Look for high-signal names such as `README`, manifests, lockfiles, `Makefile`, task-runner files, CI workflows, test/lint/type-check/build configuration, environment examples, and root instruction files. Adapt the names to the repository rather than assuming one ecosystem.

## Progressive exploration

Start at the root. Follow links and references only when they answer a current question about purpose, navigation, constraints, operations, or verification. Inspect representative modules when the root docs do not establish ownership; do not read every source file.

Use this stop condition:

> Stop exploring once enough evidence exists to identify the relevant capability, its current owner, and the smallest safe correction—or to conclude that no correction is justified.

Before expanding the scan, ask whether the next file can change the harness decision. If not, stop.

## Monorepos and workspaces

Identify the root manifest and workspace boundaries first. Then inspect:

- the root verification and CI contract;
- each workspace’s README or local instructions when ownership differs;
- representative app, service, and shared-package entrypoints;
- workspace-local test or integration commands;
- root and local constraints, including which one owns each rule.

Keep root-level and workspace-level findings separate. Preserve local ownership when a rule or command applies to one workspace only. Do not flatten the repository into one document or demand one command when valid scopes differ.

## Questions after discovery

Ask only about important unknowns that repository evidence cannot resolve. Good questions establish a decision, such as:

- Which existing command should define a valid change?
- Which subsystem or workflow is especially risky to modify?
- Is there an architectural rule that is intentionally not represented in the repository?

Do not ask for facts already present in a manifest, lockfile, script, CI file, README, or existing instruction. For a normal repository, target 0–4 questions.

## Evidence to capture

For each possible gap, record:

- the capability affected;
- the exact file, section, command, or configuration evidence;
- the current owner or source of truth;
- the smallest correction that would make the capability discoverable or executable;
- uncertainty that still needs a question or runtime check.

Missing files are not evidence by themselves. A file becomes relevant only when its absence causes a discoverability, consistency, safety, or verification failure.
