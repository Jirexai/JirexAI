# JirexAI, Inc.

**Cybersecurity infrastructure built from primitives.**

> *"Jehová-jireh" — יהוה יראה — "The Lord will provide"* (Gen 22:14)

---

## What we do

JirexAI builds networking and cybersecurity infrastructure in Rust, with a posture rarely taken in 2026: **we write the bytes ourselves against the RFCs, rather than pulling the usual third-party ecosystem onto our security-critical path.**

This is a **supply-chain decision**. For a cybersecurity vendor, depending on the same upstream libraries as every other shop is a category error: the blast radius of a single upstream compromise becomes the whole industry's problem. We keep our trust base as small as the language runtime itself.

### Production, live at [jirex.ai](https://jirex.ai)

- Authoritative DNS server (RFC 1035 / 4034 / 6891 / 8482)
- TLS 1.3 stack (handshake, AEAD, X25519, Ed25519, P-256, X.509)
- ACME client (RFC 8555, HTTP-01 + DNS-01 wildcard)
- Web server · HTTPS · UDP proxy · SNI routing
- gzip codec (RFC 1952)
- Post-quantum cryptography (in development)
- Orchestration, scheduling, defensive tooling

Every component above is code we own. `cargo tree --edges no-dev,no-build` on any of our crates produces a single node — the crate itself.

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

**Every line of source code and every line of documentation in the JirexAI product repositories is authored by [Claude Code](https://claude.com/claude-code) under the direction of the founder. This README included.**

The workflow is **design-then-direct**, not *"engineer using an AI assistant."* The human writes the specification, the adversarial test patterns, and the prompts that bind the agent's output. The agent writes the Rust, the markdown, the systemd unit files, the commit messages. The human is the architect, the pedagogue, and the hostile reviewer. The AI is the implementing hand.

The methodology described above — spec-first, hostile-test-first, zero external dependencies on the security-critical path — is what makes this partition of labor produce systems that survive adversarial testing against the live internet, rather than systems that look impressive in a demo and fall over in production.

We declare this explicitly because it matters: the code that handles your DNS, your TLS, your certificates, and your application traffic was written by an AI agent. It was also specified, pen-tested, and approved by a human who refuses to ship code he has not seen break under the kinds of inputs real attackers send. Both facts are true. Neither is hidden.

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

# Our shem nameserver, serving SOA with base-7-derived TTLs
# (2401 = 7⁴, 343 = 7³, 117649 = 7⁶). Those values only come
# from code that thinks in base 7.
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
```

**Transparency note on DNS delegation.** If you run `dig jirex.ai NS +short` you will see that the apex of `jirex.ai` is still delegated at Google Cloud DNS — a legacy arrangement from before our own authoritative nameserver existed, and one we have deliberately deferred migrating to reduce single-point-of-failure risk during the hackathon window. The subzone that actually matters for our security posture — **`acme.jirex.ai`**, where DNS-01 challenges for wildcard certificates land — is authoritatively served by our own shem nameserver at `ns.jirex.ai`. That means the path that governs our certificate issuance already runs on our own stack, even though the apex resolution does not yet. The full apex migration is on the roadmap, not hidden.

If any of these commands behaved inconsistently — missing server header, absent security headers, non-7ⁿ TTLs, failed TLS handshake, reflection-amplified response to the spoof — the claim would not survive. They do not misbehave. **The stack is the demonstration; the methodology is not a slogan.**

---

## Contact

- **Website**: [jirex.ai](https://jirex.ai)
- **Founder**: Josue Isaac Elias — [github.com/josueisaacelias](https://github.com/josueisaacelias)
- **Inquiries**: open an issue in this repository or reach out through the website

---

© 2026 JirexAI, Inc. Individual product licenses apply to proprietary code in private repositories.
