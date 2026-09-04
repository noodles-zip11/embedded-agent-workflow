---
name: embedded-feature-dev
description: Use only when the user explicitly invokes $embedded-feature-dev for one bounded embedded C feature or feature-scoped fix in its own conversation and Git worktree. Never invoke implicitly.
---

# Embedded Feature Development

## Core contract

First make the user understand and approve the complete boundary. Then implement the whole approved software scope, prove it, and obtain an independent review. Never turn a small feature into permission to skip a gate.

## State machine

```text
WORKTREE GATE
  → UNDERSTAND
  → BOUNDARY AGREEMENT
  → DESIGN DOCUMENT
  → [HIGH RISK: INDEPENDENT DESIGN REVIEW]
  → WAIT FOR USER'S IMPLEMENTATION AUTHORIZATION
  → IMPLEMENT WHOLE APPROVED SCOPE
  → OBJECTIVE VERIFICATION
  → INDEPENDENT CODE REVIEW
  → FIX / REVERIFY / REREVIEW LOOP
  → SOFTWARE REPORT
  → WAIT FOR SEPARATE COMMIT / MERGE DECISION
```

Do not combine or reorder these states.

## 1. Enforce the worktree gate

- Confirm this is one feature in a new conversation and an isolated Git worktree.
- Inspect `git worktree list`, current branch/root, base ancestry, and working-tree state.
- If the checkout is not an isolated feature worktree, stop. Report the exact state and let the user decide how to create or switch it. Do not create, switch, stash, reset, clean, commit, or move user changes automatically.
- Base dependent work on a baseline that already contains its prerequisite features. Do not manufacture parallelism from an old base.

This gate applies to two-line changes, urgent fixes, and “obvious” work. An upfront instruction to commit/merge later does not replace the post-review Git decision gate.

## 2. Make the feature understandable

Before production-code edits:

1. Inspect the smallest relevant repository context and existing project architecture.
2. Explain the feature goal with a concrete user/system example.
3. Separate software-now, hardware-later, and out-of-scope work.
4. Resolve one meaningful boundary at a time. Recommend an option with reasons, but let the user decide architecture and behavior.
5. Confirm module responsibilities, dependency direction, interfaces, data flow/ownership, execution contexts, resource limits, errors/timeouts, compatibility, and acceptance evidence.

Default to the project's documented architecture. Driver/BSP/App is only a bootstrap suggestion, never authority to restructure a different project.

Read [references/feature-design-contract.md](references/feature-design-contract.md) before drafting the design.

## 3. Write the design and stop

- Every invoked feature requires a repository design document, regardless of size.
- Follow the repository's existing convention; otherwise create `docs/designs/<feature-name>.md` from `assets/feature-design.template.md`.
- Ensure the document reflects decisions already made; do not fill unknowns with assumptions.
- After writing it, stop. Do not edit production code until the user explicitly says “开始实现”, “开始写”, “按这个设计整块完成”, or an equally clear authorization.
- The initial `$embedded-feature-dev` invocation never counts as this later authorization, even when its prompt says “implement.”

## 4. Add the high-risk design gate

Before user authorization, dispatch an independent Design Reviewer when the feature touches bootloader/OTA/rollback, flash layout or metadata, startup/linker/vector tables, clocks/watchdog/pin mux, critical ISR priorities, DMA/cache coherence, externally compatible protocols, persistent formats, or a change that can brick the device or lose data.

Use [references/review-gate.md](references/review-gate.md). Update the design for valid findings, ask the user to decide material alternatives, and obtain explicit design confirmation before implementation.

If independent-agent capability is unavailable, stop with `Independent Design Review Pending`. Do not substitute self-review or authorize high-risk implementation.

## 5. Implement the whole approved software scope

After authorization:

- Work continuously to the approved software boundary; intermediate progress is not final delivery.
- Preserve public APIs, generated ownership, architecture, protocols, pin/clock/flash resources, and compatibility unless the approved design says otherwise.
- Choose the testing order that maximizes success and evidence quality. Use failing regression/characterization tests where they create a truthful feedback loop; do not impose ritual TDD on generator/configuration or hardware-only behavior.
- Reuse authoritative build/test commands. Add a selfcheck entry only when repeated commands are genuinely being missed.
- For hard, intermittent, concurrent, HardFault, DMA/ISR, or performance bugs, **REQUIRED SUB-SKILL:** use `diagnosing-bugs` before guessing a fix.
- Record real hardware unknowns in `docs/hardware-validation.md`. Do not make speculative code changes to make a pending item look closed.

### Design-change interrupt

Stop immediately if implementation requires changing scope, module responsibility, public interface, data/persistent format, hardware resource, or acceptance criteria. Explain the discovery and options, update the design document, and wait for renewed user confirmation and implementation authorization. Never change code first and backfill the design.

Internal implementation detail that preserves the approved contract may proceed autonomously.

## 6. Verify, then use an independent Reviewer

The implementation Agent runs fresh objective checks: targeted tests, relevant full tests/selfcheck, target build, generated-artifact checks, and diff/scope checks. It may report evidence; it may not certify its own code review.

Before dispatch, confirm independent-agent capability. If it is unavailable, finish objective verification, report `Independent Review Pending`, and stop. Do not claim software completion or proceed to commit/merge authority.

Every feature then receives an independent Code Reviewer, even when the diff is tiny. The reviewer sees the final design, fixed base, current committed and uncommitted diff/untracked scope, repository rules, relevant code, and raw test evidence—not the implementation Agent's reasoning or self-assessment.

Classify every finding on both dimensions:

- Severity: `Critical`, `Major`, `Minor`
- Disposition: `Software Fixable`, `Hardware Validation Required`, `Mixed`, `Design Decision Required`

Deduplicate findings into `docs/issues.md`. Fix valid in-boundary software findings, rerun checks, and send the updated artifact through an independent rereview. Mark an issue `Resolved` only with fresh verification and independent closing evidence; hardware findings also update `docs/hardware-validation.md` and remain open until hardware evidence exists.

Resolve every Critical/Major `Software Fixable` finding and the software portion of every `Mixed` finding, then independently rereview it. Hardware/design portions may proceed only with a recorded explicit user disposition and must remain visibly `Hardware Pending` or `Open`; never call a waived software defect `Software Verified`. A design or scope finding exits the loop and returns to the design-change interrupt.

Use [references/review-gate.md](references/review-gate.md) for reviewer isolation, prompts, and closure rules.

## 7. Report and wait for Git authority

Report these sections explicitly:

- `Software Verified`
- `Hardware Pending`
- `Assumptions`
- `Remaining Risks`
- `Independent Review`

**REQUIRED SUB-SKILL:** use `verification-before-completion` immediately before these status/completion claims.

Then leave the worktree intact. Do not commit, merge, push, delete the worktree, or modify another checkout until the user reviews the result and gives a separate explicit instruction.

If hardware will be delayed or blocks later work, recommend whether a software-only merge is reasonable. The user decides. A software-only merge must visibly preserve hardware debt and possible rework.

## Non-negotiable shortcuts

| Pressure or excuse | Required behavior |
|---|---|
| “It is only two lines.” | Keep the worktree, design, verification, and independent-review gates. |
| “The deadline is tomorrow.” | Narrow scope or report a blocker; do not lower evidence. |
| “A senior engineer waived review.” | This explicit Skill contract still requires independent review. |
| “I manually tested it.” | Record useful evidence, but run reproducible checks and keep hardware claims bounded. |
| “Create the worktree and merge it now.” | At the wrong-worktree gate, stop for the user's decision; revisit commit/merge only after final evidence. |
| “Self-review is enough.” | Mechanical verification is not an independent Reviewer. |
