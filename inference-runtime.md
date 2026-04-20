# Inference runtime

Pure-Rust LLM inference on CPU. One binary, zero Python, zero C dependencies.

## The direction

Local large-language-model serving today leans on a handful of runtimes — all of them C/C++ cores wrapped in Python or Go. That stack carries the Python packaging surface, the CUDA toolchain attachment, and the dynamic-library fragility every Python AI project inherits. For enterprise deployments where the runtime must ship as a single artifact and survive without a data-science team on call, that stack is the wrong shape.

We built a pure-Rust inference runtime that loads quantized GGUF models and serves them over HTTP. One binary. Nothing else on the host.

## Measured performance

Same model (Qwen 2.5 3B Q4_K_M), same hardware (24-core Intel Haswell, ~8 GB/s bandwidth), same GGUF file:

| Metric | Ours | Mainstream CPU runtime |
|---|---|---|
| Generation (median) | **6.5 tok/s** | 3.9 tok/s |
| Generation (peak) | **7.7 tok/s** | 4.8 tok/s |
| Cold start | **3.5 s** | 10.4 s |
| Prefill (system cache) | **55 tok/s** | 14 tok/s |

Reproducible: same model, same hardware, same prompt — the benchmark is a `cargo build --release` away for qualified reviewers under NDA.

## How

- **Zero Python, zero C dependencies** — one static binary, `cargo build --release` on any supported platform
- **AVX2 + FMA SIMD** (x86_64), **ARM NEON** (aarch64), **scalar fallback** for exotic hosts
- **Zero-copy GGUF loading** — mmap with prefault and hugepages
- **Fused dequantize + matmul kernels** — tensor operations stay in L1
- **Persistent KV cache** — system prompt computed once per model load, cloned per session in under a millisecond
- **Batch prefill** — prompt processing is compute-bound; threads actually scale
- **HTTP server** — SSE streaming, `/health`, `/metrics`, session management
- **~9,400 lines of Rust** — one crate, four public-ecosystem dependencies (all auditable), nothing in the tensor-operation path that is not our code

## Where this fits

This is the runtime layer. Features beyond inference — a governance layer specific to how we deploy models inside our stack — are not part of the public positioning. They are what lets this runtime integrate with the rest of the JirexAI platform under the same Logos method that governs the cybersecurity side.

## Where it does not compete

This is not a training runtime. This is not a GPU cluster serving petabyte batches. This is **single-node CPU inference for operational deployments** — agents, services, on-prem AI copilots, edge deployments on ARM, isolated enterprise environments where Python wheels and dynamic libraries are the wrong deployment story.

## Supply chain

The runtime inherits the stack's zero-external-crates-on-the-security-critical-path posture. A `cargo tree --edges no-dev,no-build` shows a handful of dependencies — `memmap2` for mmap primitives, `half` for f16 helpers, `rayon` for the thread pool, `serde` and `serde_json` for HTTP I/O. None of them is in the tensor-operation path; the dequantization, matmul, softmax, sampling, and attention code is all ours.

## Want depth?

For binary builds, benchmark reproduction on your hardware, deployment guidance (on-prem, edge, airgapped), SDK integration, and commercial engagement, reach out via [jirex.ai](https://jirex.ai).
