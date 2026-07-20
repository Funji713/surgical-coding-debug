---
name: surgical-coding-debug
description: Evidence-first coding and debugging workflow for focused implementation, bug fixes, and test failures. Use when Codex needs to investigate, diagnose, modify code, or verify a fix while minimizing unnecessary repository exploration, retries, scope expansion, and token use.
---

# Surgical Coding And Debug

Use evidence to control scope. Do not expand context, modify code, or repeat a check unless the previous step produced new evidence that justifies it.

## Choose A Lane

Use the Fast Lane when the request is narrow, the relevant files or entry point are clear, and the expected behavior has an obvious acceptance check.

Use the Deep Lane when reproduction fails, the root cause is unclear, several modules may be involved, a shared interface may have changed, or two focused checks do not explain the issue.

Do not turn a Fast Lane task into a Deep Lane task without evidence.

## Shared Operating Rules

- State the working problem in one sentence only when ambiguity would change the work.
- Before editing, inspect the smallest useful set: target file, direct callers or consumers, relevant contract or type, and nearest test.
- Prefer exact `rg` searches and targeted file reads. Read independent files in parallel when possible.
- Inspect repository status before edits when a repository is present. Preserve unrelated user changes.
- Treat error messages, tests, logs, types, and actual call paths as evidence. Treat intuition only as a hypothesis.
- Do not scan the repository, read broad documentation, or run full test suites by default.
- Do not repeat an unchanged command or reread an unchanged file unless it answers a newly refined question.
- Do not mix the requested change with refactors, formatting churn, dependency upgrades, generated-file edits, or cleanup unless they are required for correctness.

## Fast Lane

1. Locate the implementation and its closest verification surface.
2. Confirm the local contract and the precise requested behavior.
3. Make the smallest complete patch.
4. Run the narrowest relevant validation.
5. Expand validation only when the changed code is shared across a boundary or the narrow check exposes risk.

Use Fast Lane for a contained implementation task. Do not require a full reproduction loop when there is no reported failure.

## Deep Lane: Debug

1. Capture the expected behavior, actual behavior, and the smallest reproduction or reliable log.
2. Trace from the failure point through the immediate condition, data flow, and contract boundary. Read only the next files needed to test that path.
3. Form at most three testable hypotheses. Check the cheapest discriminating evidence first.
4. Do not patch a suspected cause until evidence supports it.
5. After two rejected hypotheses, stop guessing. Revisit the reproduction, input assumptions, interface contract, or observation point before continuing.
6. Apply the smallest change that removes the proven cause and preserves surrounding behavior.
7. Re-run the original reproduction, then test the closest regression boundary.

If the failure cannot be reproduced, distinguish between missing evidence, an environment issue, and an intermittent condition. Do not claim a fix without a verification path.

## Implementation Rules

1. Check target interfaces, direct call sites, and nearby tests before changing behavior.
2. Preserve existing public contracts unless the request explicitly changes them.
3. Prefer one focused patch over a speculative series of edits.
4. Add or update a test when the repository has a suitable test surface and the behavior can be expressed reliably.
5. Do not introduce a new dependency, external side effect, data migration, or destructive operation without clear task scope.

## Validation

1. Start with the nearest test, build target, lint target, or reproduction.
2. Interpret failures: fix failures caused by the change; report unrelated environment or pre-existing failures separately.
3. Broaden validation only for shared libraries, public interfaces, cross-module flows, or evidence of wider impact.
4. If validation cannot run, state the exact command or condition that blocked it and what was checked instead.

## Communication

- Give interim updates only when new evidence, a meaningful scope change, or a blocker appears. Do not narrate routine commands.
- Keep the final handoff to the change or root cause, validation performed, and any remaining unverified risk.
- Ask the user only when the answer cannot be discovered safely from the repository or when a choice changes scope, behavior, cost, or external state.

## Avoid

- Broad exploratory searches without a question.
- Fixes based solely on a plausible story.
- Repeated fallback attempts that add no evidence.
- Full-suite validation for a clearly isolated change.
- Reporting raw command output when a concise finding is enough.
