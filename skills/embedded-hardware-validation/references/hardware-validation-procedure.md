# Hardware Validation Procedure

Load this reference before modifying the supplied example or product integration.

## Prepare a comparable experiment

Record:

- exact board/MCU/revision and power configuration;
- vendor example path and immutable pristine reference/hash;
- SDK, generator, compiler, debugger/programmer versions;
- peripheral instance, pins, clocks, interrupt/DMA mapping;
- connected device/revision and physical wiring;
- stimulus, expected observation, and instruments/log channels.

Keep the pristine example immutable. Adapt and execute only a disposable copy or separate worktree. Keep example and product observations comparable. Change one variable at a time.

## Vendor-example adaptation log

For each unavoidable change capture:

| Field | Content |
|---|---|
| File/configuration | Exact location |
| Original assumption | Board/clock/pin/tool expected by example |
| Actual hardware fact | Evidence from schematic/board/tool |
| Minimal adaptation | Exact change |
| Why unavoidable | Why the unmodified example cannot run |
| Validation | Observation after the change |

Do not add product abstractions or cleanup to the example.

## Comparison surfaces

Inspect only surfaces relevant to the current symptom:

- power, reset, boot straps, oscillator and clock enable/source;
- pin mux, electrical mode, alternate function, chip select;
- peripheral mode, width, endian, speed, timing, FIFO thresholds;
- interrupt vector/priority/enable/flags;
- DMA/DMAMUX request, direction, increments, width, count, alignment;
- cacheability, memory region, clean/invalidate/barriers;
- RTOS synchronization, callback context, timeout and error cleanup;
- protocol dummy cycles, framing, CRC and known identity/data values.

Capture register dumps, logic-analyzer traces, logs, or memory views only when they distinguish hypotheses. Avoid “log everything.”

## Failure routing

| Observation | Next action |
|---|---|
| Vendor example fails | Diagnose example assumptions, environment, wiring, power, toolchain, or hardware. Do not touch product. |
| Example passes; product passes | Record/close debt. Do not port anything. |
| Example passes; product fails | Differential diagnosis; port the minimum evidence-backed product change. |
| Product behavior cannot be compared | Define the missing seam/test; keep debt open. |
| Unrelated software defect found | Record in `docs/issues.md`; leave code unchanged. |

## Evidence hierarchy

Use the strongest available evidence appropriate to the claim:

1. Exact repeated observable behavior under controlled stimulus
2. Instrument capture or debugger/register/memory evidence
3. Target logs tied to a specific execution
4. Successful programming verification
5. Target build
6. Host/native test or simulation

Lower levels cannot prove claims that require higher levels. State what remains unproven.

## Closing a debt item

Include the procedure, actual result, hardware/software versions, raw artifact path or concise observation, repetitions/stress performed, and date. If the result contradicts the design, do not quietly update expectations; return to design confirmation.
