# Embedded Stage Review Rubric

Load this reference after the user fixes the stage scope. Apply only sections relevant to that stage.

## Architecture and boundaries

- Project-confirmed module responsibilities and dependency direction
- Bottom-up vs top-down calls, callbacks, and hidden global coupling
- Public API ownership, duplicated policy, and cross-layer leakage
- Feature interactions that were harmless in isolation but unsafe together
- Unrequested abstractions, compatibility breaks, or scope creep

## Execution and concurrency

- ISR/callback/thread/task context for each shared object
- Atomicity, critical sections, locks, queues, semaphores, event flags
- Priority inversion, starvation, missed wakeups, lost notifications
- Initialization, shutdown, restart, and callback-after-destroy paths
- Concrete race interleavings and memory-order/cache implications

## Data and memory lifetime

- Ownership transfer, reference counts, pool return, double free/reuse
- Static/stack/heap budgets and stack depth
- DMA buffer addressability, alignment, length, cache clean/invalidate
- Ring/FIFO wrap, partial frames, endian/width conversion
- Persistent records, versioning, CRC, power-loss atomicity

## Hardware-facing behavior

- Pin/clock/peripheral configuration against project evidence
- Interrupt/DMA request mapping and error flags
- Timeouts, retries, bus recovery, watchdog and brownout behavior
- Assumptions that require a board, instrument, or vendor example
- Difference between software build evidence and hardware proof

## Error and recovery paths

- Errors propagated rather than collapsed or ignored
- Bounded waits and retry limits
- Cleanup after partial initialization/transfer/write
- Recovery without corrupting state or violating ownership
- Diagnostic evidence sufficient to distinguish root causes

## Resources and performance

- CPU/ISR latency, queue depth, sample/storage/transport throughput
- Backpressure and overload policy
- Flash wear, write amplification, storage exhaustion
- Static memory, stack, bandwidth, and worst-case timing claims

## Verification integrity

- Tests are executed, not merely built or discovered
- Tests exercise the real seam and failure mode
- Host/native simulation limitations are stated
- Target build matches the intended configuration
- Hardware debt is complete and has executable procedures
- Completion claims match fresh raw evidence

## Finding threshold

Record an issue only when it has evidence, an identifiable impact, and an actionable closure condition. Put missing information into a hardware/design-evidence item rather than inventing a defect.
