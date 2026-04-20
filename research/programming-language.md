# Programming language — research

**Status:** In Design

A systems programming language designed under the Logos method, compiling to multiple targets including conventional x86/ARM and future analog substrates.

## The thesis

Every mainstream systems language inherits the accumulated hacks of the numeric conventions it was built on: floating-point as default, null as a value, signed integers as default, division that produces decimals, arrays that begin at zero, infinite loops as a legal construct. Most security and correctness bugs in modern software trace back to one of these defaults.

A language under the Logos method eliminates each of them at the grammar level — not by lint, not by convention, but by compiler rejection.

## What's specified

- **No `null` as a value.** Absence is a type discipline (`Option`-style). The compiler rejects any attempt to use absence as a number or pointer.
- **No `zero` as a meaningful value.** Arrays begin at one. "Success" is the normal control flow, not a number. "Empty" is explicit typed absence.
- **No signed integers.** Direction (inflow vs outflow, credit vs debit, forward vs back) is a separate column from magnitude. Double-entry bookkeeping as a language property.
- **No floating point.** Precision is handled by exact-rational types or bounded-field arithmetic. The FPU is never touched.
- **No division operator.** A `.separate(n)` method returns a list of exact integer parts; a `.fraction(b)` method returns an exact rational. Truncated integer division is not available.
- **Bounded loops.** Every loop declares its upper bound at compile time. `while (true) { }` does not parse.
- **Exhaustive pattern matching.** Every enumeration branch must be covered — "default" is a case, not a fallback.

## Three compilation targets

- **Translated target** — compiles to conventional x86/ARM binaries via an LLVM back end. Runs on Linux, macOS, and Windows today. The arithmetic is emulated; correctness holds.
- **Hybrid target** — emits both conventional binary and an alternative IR for the analog substrate research. A hybrid scheduler routes each process to the substrate that fits it best.
- **Native target** — compiles directly to the analog hardware research target (see [research/analog-hardware.md](./analog-hardware.md)). Zero translation overhead.

The same source compiles for all three. Developers choose their target; the language does not change.

## Why now

The language underlies everything else in the research program — the operating system, the filesystem, the assembler/VM, the compression codec all assume a language where `null` and floating point are not available. Specifying the language early keeps the rest of the research program coherent.

---

Part of the [JirexAI research program](../PRODUCTS.md#research-program--the-logos-method). For language-design review, compiler collaboration, or academic access to the specification, reach out via [jirex.ai](https://jirex.ai).
