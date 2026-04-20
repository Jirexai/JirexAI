# Operating system — research

**Status:** In Design

A kernel and userspace built under the Logos method from the substrate up.

## The thesis

Modern operating systems inherit categories of vulnerability from their language — NULL-terminated strings produce buffer overflows, signed integers produce underflows, IEEE 754 introduces precision errors, NULL pointers produce dereferences. These are not bugs in the OS; they are bugs in the *language and number system the OS is written in*.

In April 2026 an AI agent autonomously compromised one of the world's most hardened operating systems in four hours. The codebase had been audited for decades; the vulnerabilities were in the substrate, not the audit.

An OS written under the Logos method eliminates these categories by design, not by audit.

## What's specified

- **No floating point in the kernel.** Fractional precision is handled by an exact rational type; the kernel never rounds.
- **Cycle-based scheduling.** Time is divided into ordered cycles with a mandatory rest step — the scheduler cannot "skip rest to service urgent work", because the rest step is when the system consolidates, garbage-collects, and verifies.
- **Length-prefixed everything.** No null terminators. Buffer overflow by null-termination is structurally impossible.
- **Capability-based permissions.** Access control is not a bitmask — it is an algebraic model where permissions compose under well-defined operations.
- **Bounded enumerations.** Kernel enumerations are capped at small, auditable sizes; exceeding the cap forces hierarchical decomposition, not a flat list.
- **Biblical-handler pattern for external input.** Every external input handler must exhaustively enumerate its cases — common, rare, and a default — at compile time. "Uncommon" paths are the ones AI adversaries exploit; this discipline makes them first-class.

## Why now

Linux, BSDs, and Windows cannot retrofit these properties — too much of their behavior depends on the language assumptions. The research program is not a distribution of an existing kernel; it is a ground-up design. That is why it is Phase 0 — specification written, ground prepared, code has not begun.

## What is not here

Drivers, userspace ergonomics, compatibility shims, package ecosystems — all of that belongs to implementation, which begins when the substrate research (language, hardware, assembler) is farther along.

---

Part of the [JirexAI research program](../PRODUCTS.md#research-program--the-logos-method). For research collaboration, academic partnership, or methodology review under NDA, reach out via [jirex.ai](https://jirex.ai).
