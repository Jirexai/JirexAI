# Core platform

The components that make [jirex.ai](https://jirex.ai) run end-to-end. Every byte on the security-critical path written against the RFCs, with zero third-party dependencies in the cryptographic or networking path.

This is the stack a CISO, compliance auditor, or supply-chain reviewer cares about first: what serves the traffic, what speaks TLS, what requests the certificates, what stores the primitives everything else depends on.

---

## Authoritative DNS — In Production

Dual-stack IPv4/IPv6 authoritative nameserver. Serves `jirex.ai` and delegated subzones. Full RFC compliance across 1035 (base DNS), 4034 (DNSSEC resource records), 6891 (EDNS(0)), and 8482 (ANY-query minimal response — anti-amplification). Written against the standards, not on top of `bind` or `unbound` or `knot`.

Runs the same specific-address bind-per-family doctrine that canonical implementations (Knot DNS, NSD, BIND9) recommend: no wildcard listener, no ambiguity about which socket answers which query.

## Web server · HTTPS · reverse proxy — In Production

Pure-Rust HTTP/1.1 and HTTP/2 server. Reverse proxy, SNI routing, virtual hosts, UDP forward for signed transport, ACME integration. Identifies itself as its own implementation — the `Server:` header is ours, not `nginx`, `apache`, or `caddy`.

Defensive headers are baseline, not opt-in: HSTS preload, anti-clickjacking, no-sniff, strict-origin referrer policy, XSS protection. Every response is defensive by default.

## TLS 1.3 stack — In Production

Full TLS 1.3 handshake, AEAD ciphers, X25519 key exchange, Ed25519 and P-256 signatures, X.509 parsing and validation. No `rustls`, no `openssl` linked into the security path. Every byte of the handshake is code we can read line by line.

## ACME client — In Production

RFC 8555 ACME client with both challenge types: HTTP-01 for single-host certificates and DNS-01 for wildcards. Certificates flow through our in-house PKI module, not through `certbot` or `acme-dns` or commercial ACME clients.

## Foundation library — In Production

The shared Rust crate that underlies every other piece in the stack. Numeric primitives, I/O abstractions, logging, async runtime, TLS, compression, text encoding. Every product crate depends on it; it depends on nothing external. The `cargo tree --edges no-dev,no-build` on our foundation library is a single node — the crate itself.

## gzip codec — In Production

RFC 1952 compressor and decompressor. Used for HTTP content-encoding and any other compression path where we prefer owned code over `flate2` or the `zlib` C library. Small, auditable, adversarially tested against malformed inputs.

---

## Want depth?

For architecture diagrams, threat model, supply-chain attestations, deployment checklists, or commercial engagement, reach out via [jirex.ai](https://jirex.ai) or open an issue on this repository.
