# Cybersecurity suite

A Logos-grade defensive army for Unix servers. Coordinated under a single command, each soldier is a specialized daemon designed to replace a category of mainstream third-party tooling with a coherent, zero-external-dependency alternative.

The thesis: most enterprise security stacks are a collage of vendors — fail2ban here, CrowdStrike there, ClamAV for another layer, Splunk for logs, OSSEC for integrity, a WAF from a CDN vendor, a honeypot someone forked from GitHub in 2017. Each has its own supply chain, its own update cadence, its own opinion about how Linux works. The blast radius of a single upstream compromise cascades through every organization running the same collage.

JirexAI's suite replaces that collage with a single integrated army under one command structure. Every soldier speaks the same internal protocol, trusts the same identity root, logs to the same audit layer, and updates through the same signed channel.

---

## Commander — In Development

CLI, watchtower, orchestration across the suite. The single control plane operators talk to; coordinates deployment, policy distribution, cross-soldier signaling, and operator-facing status.

## Guardian — In Development

Intrusion prevention. Analyzes in-flight traffic and connection patterns, applies policy, issues bans. The direct replacement for `fail2ban` and its cousins — with the governance model and supply chain we control end-to-end.

## Cherub — In Development

File integrity watchdog. Continuously verifies that critical files on the host have not been tampered with, out-of-band from the daemons they protect. Where `OSSEC` or `tripwire` sit in the typical stack, Cherub sits — with signed baseline, signed alerts, and no network-exposed configuration surface.

## Scout — In Development

DNS-layer defense. Filters queries by reputation and threat intelligence, flags anomalies, and participates in the stack's DNS visibility pipeline. Replaces the commercial DNS-security appliance for infrastructure that does not want to hand its DNS traffic to a third party.

## WAF — In Development

Web application firewall. Enforces request schemas, blocks injection patterns, and participates in the stack's traffic-shaping rules. Sits inline in the HTTPS path; collaborates directly with the web server rather than bolting on as a separate reverse-proxy hop.

## Restorer — In Development

Backup, disaster recovery, compliance, and configuration drift detection. Knows what state the host *should* be in; restores it when it drifts; provides the artifacts that satisfy audit frameworks. Replaces the sprawl of `rsync` scripts, `borg`, `restic`, `tripwire`, and vendor compliance agents with a single daemon.

## Scribe — In Development

SIEM, log aggregation, audit trail, anomaly detection, metrics. Receives structured events from every soldier in the suite and from the core platform; indexes, correlates, and alerts. Replaces `Splunk` / `Elastic` / commercial SIEMs for infrastructure that wants its security signal in-house.

---

## In design

The following soldiers are specified and queued for implementation. Priority and sequencing are driven by client deployments and by the maturity of dependencies in the core platform.

### Ladder

SSH server and client hardening daemon. Not a replacement for `sshd` — a control layer that enforces key policy, session limits, and audit hooks on top of it. Will eventually absorb SSH serving itself.

### Honeypot

Deception technology. Presents believable targets to attackers, captures their techniques, and feeds signals back to Guardian and Scribe.

### Container guard

Container runtime security. Policy enforcement, behavioral anomaly detection, image integrity at launch and at runtime.

### Messenger

Email gateway and anti-phishing. Inbound mail filtering, outbound DKIM/SPF/DMARC enforcement, quarantine and forensic handling of flagged messages.

### Response

Incident triage and automation. Given a signal from Guardian, Scribe, or any other soldier, executes the documented response runbook — quarantine, rotation, escalation, or rollback — with human-in-the-loop gating for destructive actions.

### Segmenter

Network segmentation and zone policy enforcement. Partitions the host's network surface into isolated zones; enforces inter-zone traffic rules; revokes zone access on compromise.

### Seal

Data loss prevention. Pattern-based and content-based egress filtering; enforcement on files, on API responses, and on email.

### Witness

Forensics, evidence collection, chain of custody. When an incident is declared, Witness takes the immutable snapshot — memory, disk, network state — and signs the artifact for later legal and technical review.

### Testimony

Multi-factor authentication. Not a replacement for any single MFA vendor — a provider-agnostic MFA broker that integrates with the covenant service and with any number of downstream authenticators.

### Passage

Secure remote access. VPN-class connectivity for operators, with identity tied to the covenant service and policy tied to Segmenter.

### Pipeline

CI/CD runner with signed webhooks. Receives build requests from signed sources, executes pipelines in isolation, signs outputs. The long-term replacement for the third-party runner appliances that currently handle our own CI.

---

## Want depth?

For threat models, deployment topology, integration with existing tooling, SBOM review, or commercial engagement around the full suite or specific soldiers, reach out via [jirex.ai](https://jirex.ai).
