# Video — research

**Status:** In Design

A new way to represent and store video for the archives that matter and the applications that need more than playback.

## The direction

Pixel-grid video — the way every commercial format has stored moving images since the 1970s — was engineered for a specific kind of screen and a specific bandwidth. What it stores is not "the scene." It is a grid of color snapshots, quantized in space and quantized in time. Every downstream operation — upscaling, denoising, compression, inter-frame reasoning — works inside that compromise.

We are exploring a different representation. One where resolution is a decision made at playback, not at capture. Where the file carries motion as first-class information, not as pixel deltas to be re-inferred. Where compression fails gracefully at the edges of things rather than smudging them uniformly.

## What this should enable

- **Resolution-independent archival.** One master file reproduces at any output resolution — 1080p, 4K, 8K, IMAX — without re-encoding, without AI super-resolution, without loss across resolution changes.
- **Native motion awareness.** Tracking, event detection, forensic review, and machine-vision pipelines read the format directly instead of deriving motion from pixel comparisons.
- **Graceful degradation where it matters.** In domains where edge fidelity is the whole point — medical imaging, machine vision, surveillance, satellite imagery, scientific video — the format's lossy modes preserve the parts that carry signal rather than smearing them as modern codecs do.

## What is not specified here

Format internals, compression approach, motion handling, reference-frame strategy, and performance targets are research artifacts. They are available to qualified counterparts under NDA.

## Why this is in research

The scaffolding is close enough to what we already operate in audio that we can reason about the problem confidently, and far enough that the work is not trivial. It is realistic, not imminent — the audio track matures first, what we learn there informs the video track, and the specification becomes code once that foundation is stable.

---

Part of the [JirexAI research program](../PRODUCTS.md#research-program--the-logos-method). For research collaboration or domain-specific applications (archival, scientific imaging, medical video, machine vision), reach out via [jirex.ai](https://jirex.ai).
