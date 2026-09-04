# Independent Review Gate

Load this reference for high-risk design review and every post-implementation code review.

## Reviewer isolation

- Use a non-implementation Agent with independent context.
- Check that independent-agent capability is available before entering the gate. If unavailable, report the review as pending and stop; never fall back to implementation-Agent self-review.
- Supply raw artifacts: final design, repository rules, fixed base/ref, exact diff command, status/untracked list, relevant files, and raw verification output.
- Do not supply the implementation Agent's conclusions, suspected weaknesses, or desired verdict.
- If changes are uncommitted, include tracked working-tree/index changes and explicitly enumerate untracked feature files; do not rely on `<base>...HEAD` alone.

## Design Reviewer brief

Review the design only. Check completeness, contradictions, module ownership, dependency direction, ISR/thread context, DMA/cache/memory lifetime, error/recovery behavior, resources, compatibility, hardware assumptions, and whether acceptance can prove the goal. Classify findings by severity and disposition. Do not implement.

## Code review axes

Prefer two independent passes when capacity permits:

1. **Spec pass:** missing/partial requirements, wrong behavior, unrequested scope, and tests that do not prove the design.
2. **Standards/embedded pass:** repository standards, architecture, ownership, concurrency, ISR/DMA/cache, resource/error paths, compatibility, generated-code boundaries, and unnecessary complexity.

When a fixed base, complete committed diff, spec, and `code-review` repository prerequisites already exist, **REQUIRED SUB-SKILL:** use `code-review` for its independent Standards/Spec structure. Pass the approved worktree base and feature-design path directly. In this composed workflow, do not run `setup-matt-pocock-skills` or discover/configure an issue tracker. If those prerequisites are absent, or uncommitted/untracked feature scope is not completely visible to `code-review`, run equivalent isolated passes with the raw working-tree artifact instead of mutating the repository or omitting files.

## Finding format

For each actionable finding give:

- stable ID and concise title;
- `Critical`, `Major`, or `Minor`;
- `Software Fixable`, `Hardware Validation Required`, `Mixed`, or `Design Decision Required`;
- file/line or design-section evidence;
- concrete failure mode and impact;
- smallest credible correction;
- verification needed to close it.

No praise-only report and no speculative finding without evidence.

## Closure

- Critical/Major software findings: fix, run fresh verification, independently rereview.
- Minor: fix if low-risk and in-boundary, otherwise record for the user.
- Hardware: add/update `docs/hardware-validation.md`; software evidence cannot close it.
- Design decision: stop implementation, update the design, ask the user.
- Every finding is deduplicated into `docs/issues.md`; mark `Resolved` only after fresh verification and independent rereview.
- Resolve and independently rereview all Critical/Major software-fixable work, including the software part of a Mixed finding. Only hardware/design portions may be explicitly deferred by the user, and they remain visibly open/pending rather than resolved.
- A reviewer disagreement must be resolved with code/spec evidence; do not blindly accept or dismiss it.
