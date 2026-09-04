# Feature Design Contract

Load this reference before writing or materially updating a feature design.

## Required decisions

### Goal and scenario

- What observable product/system behavior changes?
- Give one concrete end-to-end example the user can understand.
- What evidence will distinguish success from a plausible-looking partial result?

### Scope

- Software implemented now
- Hardware work deferred
- Explicit non-goals
- Prerequisite feature/baseline

### Architecture

- Module responsibilities and single owner for each behavior
- Allowed dependency/call direction
- Public and internal interfaces
- Data flow, ownership transfer, memory lifetime
- ISR, callback, thread/task, and process contexts
- Synchronization, queues, DMA buffers, cache behavior

### Boundaries

- Timing, capacity, stack/static memory, flash, bandwidth, retry limits
- Error, timeout, cancellation, restart, power-loss, and recovery behavior
- API/protocol/persistent-format compatibility
- Generated/vendor-code ownership
- Security/safety implications where relevant

### Verification

- Automated software checks
- Target build(s)
- Independent Reviewer inputs
- Hardware validation debt, equipment, procedure, and expected observation
- Assumptions and remaining risks

## Approval states

Use explicit states in the document:

- `Draft — boundaries unresolved`
- `Final design — awaiting implementation authorization`
- `Implementation authorized`
- `Design change pending reconfirmation`

Do not mark `Implementation authorized` merely because the user discussed or approved the design. Require a separate explicit instruction to start implementation.

## Material-change test

A change is material when it alters any externally observable behavior, module owner, allowed dependency, public interface, protocol/persistent data, hardware resource, concurrency model, resource bound, or acceptance criterion. Material changes return to user confirmation. Naming, private helper shape, and equivalent local algorithms usually do not.
