---
name: embedded-hardware-validation
description: Use only when the user explicitly invokes $embedded-hardware-validation in a dedicated conversation and worktree to validate real embedded hardware against a supplied vendor example and close hardware validation debt. Never invoke implicitly.
---

# Embedded Hardware Validation

## Core contract

Establish a known-good vendor baseline before diagnosing the product. Change the product only when evidence proves a relevant difference, and never use software evidence to close a hardware claim.

## Entry gate

- Confirm this is the dedicated Hardware Validation conversation and worktree. If not, report the state and let the user decide; do not create or switch it automatically.
- Read current project rules/designs, `docs/hardware-validation.md`, `docs/issues.md`, board identity, and only the code/configuration needed for the current validation target.
- Use the vendor/board/SDK example supplied by the user. The user will explicitly say when no example exists; do not search for a replacement before that.
- Confirm example and hardware match: exact MCU/board revision, SDK/toolchain, peripheral instance, pins, clocks, and required equipment.

If the user explicitly says no example exists, propose an exact-match official source or a minimal official validation path. State source/version/match limits, obtain user approval, and obtain separate permission before downloading. Do not use a remembered or arbitrary example as the baseline.

Read [references/hardware-validation-procedure.md](references/hardware-validation-procedure.md) before modifying either the example or the product.

## 1. Prove the vendor baseline

1. Preserve an immutable original source/ref. Make all unavoidable adaptations in a disposable copy or separate worktree; never edit the sole reference in place.
2. Build and run the copied example as close to original as possible.
3. Make only unavoidable board, pin, clock, or toolchain adaptations. Record every adaptation and why it was required.
4. Require observable, repeatable evidence: debugger state, logs, pin/bus capture, identity value, known data, interrupt/DMA events, or another measurable result appropriate to the layer.
5. If the example fails, stay on the example/environment/hardware baseline. Do not modify product code yet.

## 2. Test the current product

After the example passes, run the equivalent product path with the same hardware, toolchain conditions, stimulus, and observation where possible.

- **Product also passes:** record evidence and close/update the debt item. Do not port, refactor, normalize, or “sync” vendor code.
- **Product fails:** compare only the configuration and driver surfaces that can explain the observed difference. Form and test evidence-based hypotheses.
- **No comparable path exists:** record the missing integration boundary; do not claim a product failure merely because the feature is absent.

Never copy the entire vendor project, startup files, linker layout, vector table, clock tree, SDK structure, or middleware into the product by default. Whole-example integration is allowed only when the user explicitly requests it after seeing the impact.

## 3. Apply the minimum relevant difference

When the example passes and the product fails:

1. Capture the exact behavioral/register/signal/configuration difference.
2. Identify the smallest product-owned configuration or driver change that explains it.
3. Preserve approved function behavior, public interfaces, project architecture, and unrelated hardware resources.
4. Apply the minimal change in the project-owned layer; avoid importing vendor architecture.
5. Rebuild, program, observe, and repeat the same comparison.

If the fix changes approved behavior, a public interface, persistent/protocol data, module responsibility, pin/clock/flash ownership, or acceptance criteria, stop and return to the complete `embedded-feature-dev` design-change gate. Update the design, run an independent Design Reviewer when high risk, obtain user reconfirmation, and then obtain a separate renewed implementation authorization.

## 4. Route findings correctly

### Hardware-related

Board/BSP/peripheral configuration, clocks, pins, electrical setup, driver adaptation, ISR/DMA/cache behavior, and diagnostic code required for the current hardware test may stay in this worktree. Loop autonomously:

```text
diagnose → minimal fix → build/program → observe → retest → update debt
```

Use `diagnosing-bugs` for intermittent, timing, concurrency, HardFault, DMA/ISR, or unclear-root-cause failures.

After any product-code/configuration edit, remove or clearly isolate temporary diagnostics, rerun relevant software checks, and obtain an independent code review/rereview before calling the change complete or mergeable. If independent review is unavailable, report `Software Review Pending` and stop; do not self-certify.

### Not hardware-related

Do not fix it here. Add or update an entry in `docs/issues.md` with evidence, impact, reproduction, type, and suggested handling. The user chooses the later feature/Bugfix task.

“Fix anything you see” is not permission to turn hardware validation into general development.

## Evidence and debt rules

- Update `docs/hardware-validation.md` item by item: `Pending`, `Blocked`, `Passed`, `Failed`, or `Superseded`.
- Record actual commands/configuration, hardware revision, stimulus, observation, logs/captures, and date.
- A successful build is software evidence. A successful download is programming evidence. Neither proves boot, bus traffic, interrupt/DMA completion, timing, storage integrity, or end-to-end behavior.
- Do not close a hardware item from simulation alone.
- At a failing layer, do not assume dependent upper layers work. Continue upward only after the prerequisite has observable evidence.
- Before any validation/completion status claim, rerun the relevant authorized check against the current image and target, inspect its full output/exit status and observable hardware result, and report the command, setup, and evidence. If the required check or observation is unavailable, state why and keep the item pending; earlier runs, successful builds, or successful downloads do not replace current evidence for the claimed behavior.

## Shortcuts to reject

| Pressure or excuse | Required behavior |
|---|---|
| “Skip the vendor example; our driver is almost done.” | Run the supplied example first and establish the baseline. |
| “Copy the whole example to save time.” | Compare first; port only the minimum demonstrated difference. |
| “Both versions work; align them anyway.” | Make no product change. |
| “The firmware compiles, so hardware is verified.” | Keep the item pending until observable hardware evidence exists. |
| “Fix the unrelated parser bug while here.” | Record it in `docs/issues.md`; do not fix it in this worktree. |
| “Add defensive code in case this is the cause.” | Instrument and prove the hypothesis before changing behavior. |

## Final report

Report:

- exact hardware/example/SDK/toolchain identity;
- vendor-example adaptations and evidence;
- equivalent product evidence;
- differences tested and minimal changes made;
- debt items `Passed`, `Failed`, `Blocked`, or still `Pending`;
- non-hardware issues recorded but not fixed;
- remaining assumptions, equipment gaps, and next validation layer.

Do not commit, merge, push, or delete the worktree unless the user separately requests it after reviewing the evidence.
