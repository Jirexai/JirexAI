# JirexAI, Inc.

**The First Logos-Based Company.**
Cybersecurity infrastructure built from primitives.

> *"Jehová-jireh" — יהוה יראה — "The Lord will provide"* (Gen 22:14)

---

## What we do

JirexAI builds networking and cybersecurity infrastructure in Rust, with a posture rarely taken in 2026: **we write the bytes ourselves against the RFCs, rather than pulling the usual third-party ecosystem onto our security-critical path.**

This is a **supply-chain decision**. For a cybersecurity vendor, depending on the same upstream libraries as every other shop is a category error: the blast radius of a single upstream compromise becomes the whole industry's problem. We keep our trust base as small as the language runtime itself.

### Production, live at [jirex.ai](https://jirex.ai)

- Authoritative DNS server, dual-stack IPv4/IPv6 (RFC 1035 / 4034 / 6891 / 8482)
- TLS 1.3 stack (handshake, AEAD, X25519, Ed25519, P-256, X.509)
- ACME client (RFC 8555, HTTP-01 + DNS-01 wildcard)
- Web server · HTTPS · UDP proxy · SNI routing
- gzip codec (RFC 1952)
- **Post-quantum cryptography — operational** (UOV multivariate signatures over GF(343), sponge hash with proprietary permutation, own elliptic-curve and AEAD primitives). Deployed across the identity layer of the stack.
- Orchestration, scheduling, defensive tooling

Every component above is code we own. `cargo tree --edges no-dev,no-build` on any of our crates produces a single node — the crate itself.

For the full list of products across **In Production · In Development · In Design**, see **[PRODUCTS.md](./PRODUCTS.md)** — including our Logos-grade cybersecurity suite.

### Not a catalog — an integrated stack

The components listed are the **productized surface** of an **integrated stack of 32+ crates** where every crate depends only on our own primitives up the chain: a single shared identity layer, a single wire transport, a single hashing primitive, a single proprietary arithmetic substrate. Coherence by construction, not by post-hoc documentation. A `cargo tree` over any product crate shows the full dependency graph without ever crossing into the public ecosystem on the security-critical path.

---

## How we build

**Spec-first.** Every significant feature starts as a written specification before code. Specs are versioned alongside the code they govern.

**Hostile-test-first.** Every feature ships with an adversarial test suite that tries to break it before production does. Tests are organized by defensive pattern (what can break?) and by adversary posture (how would an attacker think?). Real bugs have been caught this way and cured before release.

**Tier-gated documentation.** Every product ships with professional / enterprise / military-grade deployment artifacts: architecture, install, integration, operations, threat model, supply chain, deployment checklist.

---

## Why our product repositories are private

**All JirexAI product repositories are private.** They are proprietary, they are client-distributed under license, and they contain the operational infrastructure we sell. The public-facing artifact is the running service at [jirex.ai](https://jirex.ai) and this repository.

If you are evaluating us for a commercial engagement, academic research, security audit, or editorial coverage, reach out via the contact points below. We share relevant source, deployment attestations, threat models, and SBOMs under appropriate agreements.

---

## How this is built — design-then-direct

**JirexAI is administered and operated by [Claude Opus 4.7](https://claude.com/claude-code) under the *Logos method* governance framework authored by the founder.** Every line of source code, every line of documentation — this README included — every commit message, every release tag: all AI output, under the human's specification, adversarial review, and merge gate. The workflow is **design-then-direct**, not *"engineer using an AI assistant."* The human is the architect, the pedagogue, and the hostile reviewer; the AI is the implementing hand, the administrator, and the operator.

We declare this explicitly because it matters: the code that handles your DNS, your TLS, your certificates, and your application traffic was written, tested, and committed by an AI agent — and it was specified, pen-tested, and approved by a human who refuses to ship code he has not seen break under the kinds of inputs real attackers send. Both facts are true. Neither is hidden. **JirexAI is The First Logos-Based Company.** The founder is described in [his observation repository](https://github.com/josueisaacelias/AboutMe).

---

## Verify it yourself

The claim that an AI agent authored the production stack is easy to dismiss as marketing. It is also easy to test. `jirex.ai` is live; every piece of the argument is one command away:

```bash
# Our HTTP server identifies itself — this isn't nginx, apache, or caddy.
curl -sI https://jirex.ai | grep -i "^server"
# → server: 7S-Maqor

# Defensive headers set by us — HSTS preload, anti-clickjacking,
# referrer-policy, XSS protection. None default, all deliberate.
curl -sI https://jirex.ai
# → strict-transport-security: max-age=31536000; includeSubDomains; preload
# → x-frame-options: DENY
# → x-content-type-options: nosniff
# → referrer-policy: strict-origin-when-cross-origin

# Our shem nameserver returns SOA with deliberate TTL values
# (2401, 343, 117649) — a distinctive rhythm that no default
# nameserver produces. First-principles code leaves its fingerprint
# in values like these.
dig @ns.jirex.ai jirex.ai SOA +short
# → ns.jirex.ai. hostmaster.jirex.ai. <serial> 2401 343 117649 343

# TLS 1.3 handshake — served directly from our stack, no rustls backend.
openssl s_client -connect jirex.ai:443 -servername jirex.ai -tls1_3 </dev/null 2>&1 | \
  grep -E "^(New|Server Temp Key|Verification:)"
# → New, TLSv1.3, Cipher is TLS_AES_128_GCM_SHA256
# → Server Temp Key: X25519, 253 bits
# → Verification: OK

# ACME wildcard cert issued through our in-house DNS-01 flow.
# Certificate Transparency proof: https://crt.sh/?q=jirex.ai

# Hostile probe: QR=1 in a request is a classic reflection-amplification
# vector. Our DNS returns FORMERR with empty sections — no amplification.
printf '\x12\x34\x80\x00\x00\x01\x00\x00\x00\x00\x00\x00\x05jirex\x02ai\x00\x00\x01\x00\x01' | \
  nc -u -w 2 149.202.62.236 53 | xxd | head -2

# AAAA record publication — universally verifiable from any client:
# major public recursors (Google, Cloudflare, Quad9) all return the
# apex AAAA. This proves the record is published and propagated; it
# does not by itself prove the dual-stack listener, since recursors
# reach shem via IPv4.
dig AAAA jirex.ai @8.8.8.8
dig AAAA jirex.ai @1.1.1.1
dig AAAA jirex.ai @9.9.9.9
# → all three return 2001:41d0:305:2100::1:2e3e.

# Dual-stack listener — separate specific-address binds per address
# family (never wildcard) per canonical Knot DNS / NSD / BIND doctrine.
# Verifying this directly requires a client with native IPv6 routing;
# many residential ISPs still lack it, in which case the v6 command
# returns "network unreachable" — a property of your network, not
# ours. From a client that does have IPv6, both queries return the
# same rdata byte-identically:
dig AAAA jirex.ai @149.202.62.236              # v4 path (always works)
# dig AAAA jirex.ai @2001:41d0:305:2100::1:2e3e  # v6 path (requires client IPv6)
```

### Cross-domain evidence of the methodology

The **same governance framework** has produced verifiable outputs outside cybersecurity:

- **ARC-AGI-3** (François Chollet's abstract-reasoning benchmark, interactive-games variant) — **3 games solved at 6/6** (FT09, R11L, SC25) under AI direction. Run logs, prompts, methodology artifacts available to serious evaluators on request.
- **Kaggle Titanic** — top-5% without machine learning, using a proprietary mathematical approach (parametric model-fitting over feature space). Current ranking: [kaggle.com/josueisaacelias](https://www.kaggle.com/josueisaacelias).

The Logos method is not domain-specific. The company's current productization focus is cybersecurity; the framework generalizes.

**Transparency note on DNS delegation.** If you run `dig jirex.ai NS +short` you will see that the apex of `jirex.ai` is still delegated at Google Cloud DNS — a legacy arrangement from before our own authoritative nameserver existed, and one we have deliberately deferred migrating to reduce single-point-of-failure risk during the hackathon window. The subzone that actually matters for our security posture — **`acme.jirex.ai`**, where DNS-01 challenges for wildcard certificates land — is authoritatively served by our own shem nameserver at `ns.jirex.ai`. That means the path that governs our certificate issuance already runs on our own stack, even though the apex resolution does not yet. The full apex migration is on the roadmap, not hidden.

If any of these commands behaved inconsistently — missing server header, absent security headers, non-7ⁿ TTLs, failed TLS handshake, reflection-amplified response to the spoof, asymmetric dual-stack responses — the claim would not survive. **They do not misbehave.**

---

## Contact

- **Website**: [jirex.ai](https://jirex.ai)
- **Founder**: Josue Isaac Elias — [github.com/josueisaacelias](https://github.com/josueisaacelias)
- **Inquiries**: open an issue in this repository or reach out through the website

---

© 2026 JirexAI, Inc. Individual product licenses apply to proprietary code in private repositories.
