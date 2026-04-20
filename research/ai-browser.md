# AI-agent browser — research

**Status:** In Design

A pure-Rust console browser engine designed for AI-directed traversal rather than for human clicking.

## The thesis

Every production browser — Chromium, Firefox, Safari, WebKit — is built around a human-centered interaction model: pixels, clicks, mouse hovers, viewports, visual layouts. When an AI agent needs to read and interact with the web, it either drives one of these human-centered browsers via automation (Playwright, Puppeteer) or it parses raw HTML with a library (Beautiful Soup, `reqwest`) and misses everything the page wanted to render.

Both paths are compromises. The automation path pays for a rendering engine the AI does not need; the scraping path misses interactive state. Neither is purpose-built for the agent.

An AI-agent browser asks: what does a browser look like when the "viewer" is not human?

## What's specified

### Zero external dependencies

The browser is built on the JirexAI foundation library and nothing else. No `chromium-embedded`, no `webkit`, no HTML parser from crates.io. Every parser, every networking call, every rendering primitive is owned code.

This is the same supply-chain discipline that applies to the core platform: a cybersecurity-oriented AI agent cannot depend on a browser engine whose CVE stream it does not control.

### Semantic over visual

The browser's internal representation of a page is its document semantic tree, not its pixel layout. The AI reads meaning, structure, and interactive affordances directly; the expensive rendering pipeline that turns that tree into pixels is absent entirely.

### Console-native, structured-output

The "viewport" is a structured data object, not a framebuffer. The AI queries the page the way a human queries a JSON document: what are the forms, what are the links, what is the article text, what are the interactive states. The browser presents its findings as typed records.

### Driven by the agent protocol

The browser accepts commands over the native agent protocol (see [identity-and-transport.md](../identity-and-transport.md)) rather than over a browser-automation surface. The protocol was designed for AI-to-infrastructure calls; the browser is one of its natural consumers.

## Why this is in research

The engine requires mature HTML parsing, CSS interpretation (at least enough for dynamic content), JavaScript evaluation (hardest piece), and network handling — each a substantial piece. The specification defines the architecture and the interaction surface; implementation will follow the usual spec-first / hostile-test-first discipline once earlier-stage research items have matured.

The AI-agent use case also intersects with the larger AI-agent work JirexAI has in flight but does not disclose publicly. Once that work is ready to share, this browser will likely gain deployment paths faster than its current Phase 0 status suggests.

---

Part of the [JirexAI research program](../PRODUCTS.md#research-program--the-logos-method). For collaboration on AI-first browsing patterns, automated evaluation of web content, or semantic-web-oriented agents, reach out via [jirex.ai](https://jirex.ai).
