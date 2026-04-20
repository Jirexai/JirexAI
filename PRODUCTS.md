# Products — Index

What JirexAI is building, where each piece stands today.

**Status legend:**
- **In Production** — deployed, handling real traffic at [jirex.ai](https://jirex.ai) or client environments.
- **In Development** — coded and under adversarial testing; tests pass; not yet serving production traffic.
- **In Design** — specification written, scope declared, no code yet.

Each category below links to its own page. Research items have one page per item. If you need depth on any specific component today, reach out.

---

## [Core platform](./core-platform.md)

The components that make jirex.ai run end-to-end. Every byte on the security-critical path written against the RFCs, not pulled from the ecosystem.

| Component | Status |
|---|---|
| Authoritative DNS (dual-stack IPv4/IPv6) | In Production |
| Web server · HTTPS · reverse proxy | In Production |
| TLS 1.3 stack | In Production |
| ACME client (HTTP-01 + DNS-01 wildcard) | In Production |
| Foundation library (shared Rust primitives) | In Production |
| gzip codec | In Production |

→ **[Read more](./core-platform.md)**

## [Identity & transport](./identity-and-transport.md)

Post-quantum identity and signed transport for machine-to-machine infrastructure.

| Component | Status |
|---|---|
| Post-quantum cryptography | In Production |
| Per-packet signed transport | In Production |
| Covenant / identity service | In Production |
| PKI / secrets vault | In Production |
| Native agent protocol | In Development |

→ **[Read more](./identity-and-transport.md)**

## [Cybersecurity suite](./cybersecurity-suite.md)

A Logos-grade defensive army for Unix servers, coordinated under a single command. Each soldier is a specialized daemon with zero external crates, designed to replace a category of third-party tooling with a single coherent stack.

| Soldier | Role | Status |
|---|---|---|
| Commander | CLI, watchtower, orchestration | In Development |
| Guardian | Intrusion prevention (replaces fail2ban) | In Development |
| Cherub | File integrity watchdog | In Development |
| Scout | DNS filtering, reputation, threat intelligence | In Development |
| WAF | Web application firewall | In Development |
| Restorer | Backup, DR, compliance, config drift | In Development |
| Scribe | SIEM, logs, audit trail, anomaly detection | In Development |
| Ladder | SSH server + client hardening | In Design |
| Honeypot | Deception technology | In Design |
| Container guard | Container runtime security | In Design |
| Messenger | Email gateway + anti-phishing | In Design |
| Response | Incident triage + automation | In Design |
| Segmenter | Network segmentation + zone policy | In Design |
| Seal | Data loss prevention | In Design |
| Witness | Forensics, evidence collection, chain of custody | In Design |
| Testimony | Multi-factor authentication | In Design |
| Passage | VPN / secure remote access | In Design |
| Pipeline | CI/CD runner with signed webhooks | In Design |

→ **[Read more](./cybersecurity-suite.md)**

## [Data layer](./data-layer.md)

| Component | Status |
|---|---|
| Database engine | In Development |

→ **[Read more](./data-layer.md)**

## [Audio & video](./audio-and-video.md)

Continuous representation for time-series media — replacing discrete sampling with evaluable structure. Audio is further along; video is at specification.

| Component | Status |
|---|---|
| Audio format | In Development |
| Video format | In Design — see [research](./research/video.md) |

→ **[Read more](./audio-and-video.md)**

## [Biometric identity](./biometric-identity.md)

Continuous-representation biometric identification from physiological signals captured by consumer-grade hardware. No machine learning on the matching path.

| Component | Status |
|---|---|
| Cardiac identity | In Development |
| Vocal identity | In Development |
| Bimodal fusion (heart + voice) | In Development |

→ **[Read more](./biometric-identity.md)**

## [Inference runtime](./inference-runtime.md)

Pure-Rust LLM inference on CPU — one binary, zero Python, zero C dependencies. Measurably faster than the mainstream CPU alternative on identical hardware and models.

| Component | Status |
|---|---|
| LLM inference runtime | In Development |

→ **[Read more](./inference-runtime.md)**

## Research program — the Logos method

Longer-horizon work: what does computing look like when it is built, from the substrate up, under the Logos method rather than the conventions inherited from the 1970s? Each item below is a published specification; implementation follows the same spec-first / hostile-test-first discipline as the rest of the stack. One page per research item.

| Research item | Status | Page |
|---|---|---|
| Operating system | In Design | [research/operating-system.md](./research/operating-system.md) |
| Programming language | In Design | [research/programming-language.md](./research/programming-language.md) |
| Analog hardware | In Design | [research/analog-hardware.md](./research/analog-hardware.md) |
| Compression codec | In Design | [research/compression-codec.md](./research/compression-codec.md) |
| Filesystem | In Design | [research/filesystem.md](./research/filesystem.md) |
| Assembler / ISA / VM | In Design | [research/assembler-vm.md](./research/assembler-vm.md) |
| AI-agent browser | In Design | [research/ai-browser.md](./research/ai-browser.md) |
| Video format | In Design | [research/video.md](./research/video.md) |

---

## What is not listed here

Client-specific integrations, internal tooling, operational automation, and products we are not yet ready to disclose publicly. For a full stack review under NDA, reach out via [jirex.ai](https://jirex.ai) or open an issue in this repository.

---

*Last updated: 2026-04-19. This index evolves with the stack; the commit history on this file is the honest record of what moved when.*
