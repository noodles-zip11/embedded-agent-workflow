# Embedded Agent Workflow

An opinionated personal Agent Skills bundle for embedded C development. It captures the workflow I use to bootstrap projects, define feature boundaries, require independent review, and separate software evidence from target-hardware proof.

> Status: personal portfolio snapshot. The Skills have been structurally checked, but a complete STM32F407 end-to-end hardware run has not yet been performed.

## Skills

- `embedded-project-bootstrap` - lands a user-chosen MCU, RTOS, SDK, and toolchain on an official starting point.
- `embedded-feature-dev` - gates a bounded feature through design, explicit implementation authorization, verification, and independent review.
- `embedded-stage-review` - performs a read-only stage review and records evidence-backed findings.
- `embedded-hardware-validation` - establishes a vendor-example baseline before comparing and minimally correcting the product integration.

All four Skills are explicit-only. Invoke them by name, for example `$embedded-feature-dev`.

## Installation

Clone this repository and copy all four folders from `skills/` into the Skills directory supported by your agent. For this Codex setup, the personal directory is `~/.codex/skills/`. The bundle is designed to work together; hardware validation may route a material design change back through `$embedded-feature-dev` in a separate conversation and worktree.

`embedded-feature-dev` and `embedded-hardware-validation` require Git worktree and independent-agent support. `embedded-stage-review` can use independent agents when its review axes are separable. The Skills reference `verification-before-completion`, `diagnosing-bugs`, and conditionally `code-review`; install compatible versions separately or expect the relevant gate to remain pending.

## Scope

This repository contains workflow instructions and templates only. It does not bundle vendor SDKs, generated firmware, board examples, or hardware tools. CubeMX may be used for an STM32 project, but it is not a package dependency.

## License

MIT

---

这是一套源自个人嵌入式项目实践的 Agent Skills 工作流备份，重点是用户门禁、独立审查、官方例程基线，以及软件验证与真机证据分离。
