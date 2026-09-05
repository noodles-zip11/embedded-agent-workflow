---
name: embedded-project-bootstrap
description: Use only when the user explicitly invokes $embedded-project-bootstrap to turn an already chosen MCU, board, RTOS, SDK, and toolchain into a clean, reproducible embedded C project. Never invoke implicitly.
---

# Embedded Project Bootstrap

## Core contract

Land the user's chosen stack; never choose it for them. Prefer the official starting point and prove only what the available evidence supports.

## Hard gates

- Do not start until the user has selected the technology stack and confirmed the project architecture/dependency direction. Ask for one blocking fact at a time when the exact MCU/board revision, RTOS, SDK, generator, toolchain, or architecture is missing.
- Do not silently substitute a remembered board, package, SDK version, or “latest” dependency.
- Prefer, in order: the exact vendor/board/RTOS template, the vendor configuration or code-generation tool, the official SDK scaffold, then a from-scratch project only when no suitable official base exists.
- Treat generator configuration as the source of truth. Modify supported user regions or separate modules. Before editing generator-owned code directly, explain why, alternatives, regeneration risk, and wait for explicit approval.
- Do not add product features to the empty template.
- Do not modify the global `AGENTS.md`. Generate or update only the project `AGENTS.md` from confirmed project decisions.

## Workflow

1. **Inspect narrowly.** Read the repository rules, provided examples, generator files, build entry points, and relevant local SDK metadata. Do not crawl vendor trees without a concrete query.
2. **Confirm the contract.** Record the exact target, board revision, RTOS/bare-metal choice, SDK/tool versions, build system, debugger, and intended observable boot signal. The user owns these choices.
3. **Confirm architecture.** Suggest Driver/BSP/App only as a default. Preserve the official template's structure unless the user explicitly approves different module responsibilities and dependency direction.
4. **Choose the official base.** Use a user-provided example when available. Explain any mismatch before proceeding. Do not mix multiple starters.
5. **Resolve dependencies.** Give exact version, official source, integrity/version evidence, and destination. The user may download manually, or the agent may download only after explicit permission for that download.
6. **Recommend dependency layout.** Compare the ecosystem-supported package manager, submodule, vendored copy, or pinned external SDK. Explain reproducibility, repository size, upgrade, and collaboration tradeoffs; wait for the user's decision.
7. **Configure minimally.** Preserve debug access and official regeneration. Keep application-owned code outside generated ownership where possible.
8. **Create project contracts.** Use [assets/project-agents.template.md](assets/project-agents.template.md) as a starting shape, not as invented facts. If `AGENTS.md` exists, merge minimally and preserve unrelated rules; stop before deleting a rule or resolving a conflict without user approval. Initialize the validation ledgers only when absent.
9. **Verify with fresh evidence.** Follow the completion levels below. Before any completion/status claim, run the relevant authorized check against the current artifact, inspect its full output and exit status, and report the command and result. If a check cannot run, state why and keep the corresponding status pending; do not infer success from an earlier run or from a different completion level.

Read [references/bootstrap-contract.md](references/bootstrap-contract.md) when selecting a base, handling generated code, or deciding completion evidence.

## Completion levels

| Available evidence | Allowed status | Required proof |
|---|---|---|
| No hardware | `Clean Build Verified / Hardware Pending` | Fresh clean build with the selected toolchain; no missing generated inputs or developer-only paths |
| Hardware available | `Download Verified` | Successful programming plus tool output tied to the intended image/target |
| Hardware available | `Boot Verified` | Observable, repeatable proof such as reaching `main`, startup log, heartbeat pin/LED, or a running RTOS task |

Never infer `Boot Verified` from a successful download. If hardware is expected but unavailable, record the exact pending test.

## Required project artifacts

- Project `AGENTS.md`: confirmed stack, architecture, generated-code ownership, protected resources, authoritative build/test/debug commands.
- `docs/issues.md`: create from `assets/issues.template.md` when absent.
- `docs/hardware-validation.md`: create from `assets/hardware-validation.template.md` when absent.

Do not create README, changelog, or process documents merely for completeness. Reuse existing canonical files when present.

## Stop and ask

Stop before:

- choosing or changing the technology stack;
- using a mismatched board/example;
- downloading external dependencies without permission;
- deciding a repository-wide dependency layout;
- imposing or changing project architecture/dependency direction;
- editing generator-owned code outside supported regions;
- deleting or overriding existing project `AGENTS.md` rules;
- claiming hardware success without observable evidence.

## Common shortcuts to reject

| Shortcut | Required response |
|---|---|
| “Use whatever latest SDK you remember.” | Pin an exact installed or user-approved official version. |
| “Generate now; board revision can come later.” | Stop at the missing target fact; do not create a false product baseline. |
| “Copy every dependency into the repo.” | Recommend an ecosystem-appropriate layout and wait for confirmation. |
| “The download tool says success, so it boots.” | Report `Download Verified`; obtain an observable boot signal. |
| “Edit generated files because it is faster.” | Protect regeneration; request approval for an unavoidable direct edit. |

## Final report

Report:

- selected official base and pinned versions;
- dependency source and layout;
- generated-code ownership and any approved exception;
- exact build/download/boot commands run and their results;
- `Clean Build Verified`, `Download Verified`, `Boot Verified`, and `Hardware Pending` separately;
- remaining assumptions and next hardware evidence.
