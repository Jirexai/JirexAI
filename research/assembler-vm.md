# Assembler / ISA / VM — research

**Status:** In Design

An instruction set architecture and assembly language with a minimal primitive set, mapping to both conventional and research-target hardware.

## The thesis

Every modern ISA — x86, ARM, RISC-V — is a thousand-instruction catalog accumulated over forty years of compromises. Most of those instructions exist because a specific compiler needed a specific optimization in a specific generation of chip. The minimal ISA needed to express any computation is much smaller.

A Logos-method ISA starts from the minimum and stays there. A small, fixed primitive set — Turing-complete by construction. Everything else is composition.

## The primitive set

| Instruction | What it does |
|---|---|
| `UNIT` | The atom, the seed of all magnitude |
| `SEPARATE` | Duplication, the generative split |
| `JOIN` | Fusion, summation |
| `INVOKE` | Call, reference, citation |
| `REMEMBER` | Persistent storage — load and save |
| `DECIDE` | Conditional branch |
| `REST` | End of block, halt |

These compose into the full range of computation. The arithmetic built from `UNIT`, `SEPARATE`, and `JOIN` is constructive and has no negative, zero, or infinite as primitives. `INVOKE` gives modularity and recursion. `REMEMBER` gives state. `DECIDE` gives control flow. `REST` terminates.

## The virtual machine

The VM that executes these primitives is auditable in under a few hundred lines of the research-program language. It has no speculative execution, no indirect-branch prediction, no hidden microarchitecture state — the entire semantics of execution is visible and deterministic.

Side-channel resistance comes free: constant-time by default for everything that matters cryptographically, because there is no machinery underneath that does anything non-deterministic.

## Compilation targets

The [research programming language](./programming-language.md) emits this ISA directly. Because the ISA is small, the compiler is small. Because the VM is small, the runtime is small. Because everything is small, the security surface is small.

For the translated target (today's x86/ARM), a back end emits conventional instructions. For the native target (future analog substrate), the primitives map one-to-one to hardware operations.

## The tablets metaphor

The first written instructions humanity received — the tablets of the Law, in two stone pieces — were dense (ten commandments in a compact form), strict (no free interpretation), persistent (stone, not papyrus), and required a mediator to interpret for the people. That is exactly what an assembler does: a dense, strict, persistent instruction form that requires a compiler to interpret for the human reader.

The metaphor shapes the design — the instruction form is *tabular* (fixed-layout, not variable-length), *signed* (integrity attached to each instruction block), and *persistent* (written once, verified on every load).

---

Part of the [JirexAI research program](../PRODUCTS.md#research-program--the-logos-method). For ISA review, compiler-back-end collaboration, or formal-semantics partnership, reach out via [jirex.ai](https://jirex.ai).
