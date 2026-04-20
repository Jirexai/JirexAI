# Analog hardware — research

**Status:** In Design

Hardware substrates where computation emerges from continuous physical phenomena rather than forced-binary digital abstraction.

## The thesis

Binary logic is a *hack* on top of continuous physics. A transistor's voltage is naturally a continuum; the digital logic gate forces it to be either on or off by defining a "forbidden zone" in the middle. Every bit of modern computing wastes the majority of the transistor's dynamic range in exchange for the simplicity of two-state arithmetic.

Nature is not binary. Neurons do not fire 0 or 1 — they fire with variable amplitude and frequency. Photons have continuous intensity, angular momentum, and polarization states. Memristors have multiple stable resistance levels. Qudits have arbitrary numbers of coherent quantum states. The Logos method asks: what if the substrate carried more than two states per wire, *natively*?

## Candidate substrates

### Photonic

Optical modulators can produce multiple distinguishable intensity levels in a single fiber. Already used commercially for PAM-4 and higher-order modulation. No electromagnetic interference between parallel channels. Speed limited by the speed of light, not by electron drift.

### Memristive

Non-volatile components whose resistance depends on history. HP Labs demonstrated multi-level memristors in 2008; Intel, Samsung, and Crossbar Inc. have had active programs. Stable states without power. In-memory computation (no separate CPU / RAM bus).

### Multi-level qudit (quantum)

Atoms with multiple hyperfine levels. Photons with multiple orbital-angular-momentum modes. Superconducting circuits with multiple energy levels. A qudit of dimension *d* carries `log₂(d)` times the information of a qubit per coherent state — error correction over the larger alphabet has algebraic structure native to our research primitives.

### Neuromorphic

Chips that model neurons with continuous activation rather than firing binary spikes. Intel Loihi, IBM TrueNorth have demonstrated the pattern. Extremely energy-efficient compared to GPU inference.

### Molecular

Chemical concentration gradients as state. DNA has four bases; biological signaling routinely uses multiple simultaneous concentration channels. Slower than electronics, but applicable to biological and medical-adjacent computing.

## Roadmap

1. **FPGA simulation.** Emulate the alternative logic on commodity FPGAs. Benchmark against binary FPGA implementations of the same algorithms.
2. **FPGA + DAC/ADC.** Real multi-level voltage wires feeding the FPGA. The physical substrate now carries the alternative states; the logic still runs on silicon.
3. **Prototype ASIC.** Memristive or photonic ASIC prototype. Native multi-level logic, no translation.
4. **Full substrate.** Compiled output from the [programming language](./programming-language.md) runs natively on the ASIC. Binary compatibility layer serves legacy traffic.

## Why this matters

When the cryptographic primitives in the core platform assume field arithmetic that the hardware does not execute natively, every operation pays a translation tax. When the hardware *is* the field, that tax disappears — and so do entire classes of side-channel leaks that exist because the hardware is silently re-encoding.

---

Part of the [JirexAI research program](../PRODUCTS.md#research-program--the-logos-method). For research collaboration, FPGA access, or industry partnership, reach out via [jirex.ai](https://jirex.ai).
