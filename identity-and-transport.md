# Identity & transport

Post-quantum identity and cryptographically signed transport for machine-to-machine infrastructure.

The thesis: for AI-agent and infrastructure-to-infrastructure communication, the 1990s web assumptions (TCP, HTTP headers, session tokens over TLS, RSA or ECDSA at the identity layer) are both unnecessarily verbose and increasingly fragile against quantum adversaries. JirexAI's identity and transport layer replaces that entire substrate with owned post-quantum primitives, a signed UDP wire format, and a covenant model for agent trust.

---

## Post-quantum cryptography — In Production

UOV multivariate signatures over GF(343) — an NIST-standardized post-quantum signature family chosen for short signatures and fast verification. Sponge-hash construction with a proprietary permutation. Elliptic-curve and AEAD primitives written in-house rather than pulled from `ring`, `rustcrypto`, or `openssl`.

Deployed across the identity layer: every signed artifact in the stack is signed with our PQ primitives, not with RSA or Ed25519. This is the most consequential choice we've made — once quantum advantage arrives, nothing on our critical path needs re-issuance.

## Per-packet signed transport — In Production

UDP-based transport where **every datagram is individually signed** with a post-quantum signature. Replaces TCP for internal infrastructure traffic: no session state, no TLS handshake tax, no connection teardown — each packet authenticates itself and is accepted or rejected on its own merits.

Base layer of the stack's agent-to-agent communication. Integrates tightly with the covenant service below.

## Covenant / identity service — In Production

Cryptographic key registration and root-of-trust service for cooperating agents. When a new agent joins the ecosystem, it negotiates a *covenant* with the root — a post-quantum-signed identity record that governs subsequent communication. Replaces the TLS client-certificate ceremony and the session-token dance with a single signed artifact.

Also handles *brain distribution* — the mechanism by which agents synchronize shared operational context and protocols. Each update is a signed, versioned delta.

## PKI / secrets vault — In Production

Certificate authority, secret storage, and key-lifecycle management. Every X.509 certificate served by our TLS stack is issued through this layer; every symmetric secret used by the stack lives behind its authorization gates. Rotation, revocation, and audit trails are first-class.

## Native agent protocol — In Development

A 46-byte binary wire format for AI-agent-to-infrastructure calls. The same intent that HTTP expresses in 400+ bytes of text headers fits in 46 bytes of structured binary. Every frame carries an integrity tag; malformed or unauthenticated frames close the connection silently — the protocol presents a *wall*, not an error page, to unauthorized traffic.

Designed for AI-first call patterns (many small parallel queries, persistent connections, cursor-based streaming) rather than for human-clicked page loads.

---

## Want depth?

For cryptographic reviews, threat models, key-ceremony documentation, or commercial engagement around our identity stack, reach out via [jirex.ai](https://jirex.ai).
