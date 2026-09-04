---
name: embedded-stage-review
description: Use only when the user explicitly invokes $embedded-stage-review in a new conversation to review a user-selected embedded C project stage and record findings without fixing production code. Never invoke implicitly.
---

# Embedded Stage Review

## Core contract

Review the user-selected stage as an independent, read-heavy activity. Preserve the evidence: record problems in the project's single issue ledger and leave remediation to a later user-selected conversation/worktree.

## Permission boundary

Allowed writes:

- create or update only the canonical project issue ledger, normally `docs/issues.md`;
- create a separate detailed review artifact only when evidence is too large for the ledger, then link it from the ledger.

Forbidden without a later explicit request:

- production code, tests, build files, generated files, hardware configuration, or design changes;
- formatting or “obvious” cleanup;
- commits, merges, pushes, branches, worktrees, stashes, resets, or issue closure.

The user decides when the review starts and which features/modules form the stage. Do not expand the scope merely because nearby code looks interesting.

## Workflow

1. **Pin scope and evidence.** Record repository/branch/HEAD, working-tree state, stage modules/features, relevant design documents, project `AGENTS.md`, validation results, and known hardware debt. If scope is ambiguous, ask one focused question.
2. **Build a stage map.** Trace interfaces and dependency direction across the selected modules. Identify execution contexts, shared state, ownership transfers, buffers, queues, storage/protocol boundaries, and failure/recovery paths.
3. **Choose review axes.** Read [references/stage-review-rubric.md](references/stage-review-rubric.md). Use independent Subagents only for genuinely separable read-only axes whose results can be aggregated without shared writes. Do not create a ceremonial fixed team.
4. **Verify findings.** Cite concrete file/line or design evidence and describe the failure mode. Inspect a diagnostic/test command before running it. Run only commands proven side-effect-free apart from disposable/ignored outputs; if it can program hardware, mutate fixtures/external state, or write outside disposable outputs, request permission or use existing evidence. Do not modify code to prove a finding.
5. **Deduplicate.** Search the canonical ledger before adding a finding. Update an existing issue's evidence when it is the same root problem.
6. **Write findings only.** Create `docs/issues.md` if absent, using the schema below. Do not fix findings in this conversation.
7. **Report.** Summarize reviewed scope, evidence/commands, counts by severity/type, release or stage blockers, hardware limitations, and the ledger path. Leave remediation ordering to the user.

Before any pass/fail or completion-style claim, **REQUIRED SUB-SKILL:** use `verification-before-completion` against the evidence actually collected.

## Finding schema

Each independently actionable finding needs:

- stable ID and concise title;
- source review/stage and status;
- `Critical`, `Major`, or `Minor`;
- `Software Fixable`, `Hardware Validation Required`, `Mixed`, or `Design Decision Required`;
- file/line or design-section evidence;
- concrete failure mode and impact;
- reproducible check or missing evidence;
- smallest credible handling direction;
- closing evidence required.

Default status is `Open` or `Hardware Pending`, never `Resolved` during the review.

## Review quality rules

- Review the project-defined architecture; Driver/BSP/App is not universal.
- Separate a documented violation from a judgement call.
- Skip style issues already enforced by tooling unless the tooling is not actually run.
- Do not dilute safety/correctness findings with large cleanup lists.
- Do not claim a race, overflow, or lifetime bug without a concrete interleaving/data path.
- Do not let one axis mask another: spec compliance, architecture/standards, embedded correctness, and verification evidence remain distinguishable.
- Hardware uncertainty is not proof of a defect. Record the assumption and required test.

## Shortcuts to reject

| Pressure or excuse | Required behavior |
|---|---|
| “You are already in the code; fix it.” | Record the finding only; remediation belongs to a new user-selected task. |
| “It is just formatting.” | Record a Minor issue only if useful; do not edit it. |
| “Release is tomorrow.” | State the blocker/risk; do not turn review into an unreviewed hotfix. |
| “Give us one clean commit.” | Do not commit. The user decides remediation and Git actions later. |
| “There is no issue ledger.” | Create only the canonical `docs/issues.md`; do not scatter reports. |

## Final report

Use:

- `Reviewed scope`
- `Findings recorded`
- `Critical / Major / Minor`
- `Hardware evidence gaps`
- `Tools and commands used`
- `Ledger updated`
- `Risks / next steps`

State explicitly that production code was not modified.
