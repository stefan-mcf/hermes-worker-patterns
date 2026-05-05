# Optimized Execution Substrate Contract

Use this reference for optimized worker-pattern runs where the caller wants deliberate routing rather than defaulting to a single controller session.

## Required setup

Before choosing the route, load or consult:

- the worker-pattern selector;
- the runtime-specific worker/swarm guidance, if persistent workers are in scope;
- the delegation/subagent workflow guidance, if ephemeral subagents are in scope;
- any domain-specific skills or runbooks for the task.

## Substrate classes

Declare the actual runtime substrate as one of:

1. `controller-only`
2. `controller + advisory subagent lanes`
3. `mutating subagent lanes`
4. `persistent worker read-only lanes`
5. `persistent worker mutating lanes`

`module-swarm` is a worker-pattern shape. It is not itself proof that any runtime-specific persistent worker profiles were launched.

## Routing declaration template

Record this before execution when practical, and always include it in the closeout ledger:

```text
Selected worker-pattern shape: [shape]. Actual execution substrate: [controller/subagent/persistent workers]. Mutating owner: [controller or worker identity]. Worker lanes: [read-only/advisory/mutating]. Persistent workers launched: [none or identities with smoke status]. Collision boundary: [files owned by controller/workers]. Verification gate: [commands/artifacts].
```

Example:

```text
Selected worker-pattern shape: module-swarm + twin-inspection. Actual execution substrate: controller mutating lane + read-only advisory/review lanes. Mutating owner: controller. Persistent workers launched: none. Collision boundary: report source and references owned by controller. Verification gate: compile, page-count check, citation audit, mirror hash/text match, independent final review.
```

## Collision-domain gate

Before spawning workers, identify likely write targets and classify each write surface:

- single-file/high-collision: e.g. a canonical report source, lock file, central config, or shared manifest;
- disjoint artifacts: e.g. separate notes, separate modules, separate tests;
- generated outputs: e.g. PDFs, logs, reports.

Routing rules:

- If multiple lanes need the same canonical file, only one owner edits it.
- Workers may produce advisory notes only unless they own explicit disjoint paths.
- Mutating workers must have bounded allowed paths and explicit forbidden paths.
- Record the canonical final artifact path and the owner for each high-collision file.

Example policy:

```text
Controller owns: canonical source files, final generated artifact, mirror copies.
Workers may inspect/read only and return advisory or review notes.
Workers may not modify canonical report files.
```

## Advisory-lane discipline

Use advisory subagents/workers for bounded citation review, spec/checklist review, spec compliance review, final quality review, and focused read-only inspection. Avoid asking a worker to inspect an entire workspace unless that breadth is essential.

Every advisory prompt should include:

- exact files or folders to inspect;
- exact questions to answer;
- explicit no-modify instruction when read-only;
- concise output schema: `Must-fix`, `Should-fix`, `Future work`, `Verdict`.

Treat worker findings as advisory until the controller verifies the file-backed state.

## Closeout ledger

Every optimized closeout should include:

- selected worker-pattern shape;
- actual execution substrate;
- mutating owner and collision boundary;
- files created/modified;
- commands run and outcomes;
- verification artifacts/results;
- worker lanes launched and not launched;
- caveats;
- external side effects performed or explicitly not performed;
- human review gate or remaining operator decision.

Never claim persistent worker usage unless named worker processes were launched, smoke-tested, monitored, and verified with expected output/report artifacts.
