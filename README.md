# Umbra Studio

Software engineering practice of **Miles Lewis** — Carrollton, TX. I design multi-agent
development systems and ship the production software they produce: live web platforms, a C++
multiplayer game service, and a from-scratch Unity client, all built and operated solo.

> **Why the repos are private:** client ownership, live-service security, and commercial code.
> Publishing a live game server's source hands exploiters a map; publishing a payment platform's
> auth internals does the same. I'm happy to screen-share any codebase live — walkthroughs on
> request: [miles@umbrastudio.io](mailto:miles@umbrastudio.io)

---

## Systems

### Chaos Pirates — live multiplayer game platform · [chaospirates.com](https://chaospirates.com)
Inherited C++ four-daemon server cluster (five world processes, 31 maps) plus a from-scratch
Unity desktop/mobile client — ~150,000 lines of original C# implementing the server's immutable
600-opcode binary protocol.

- Removed an unauthenticated RCE path in inherited server code; parameterized 48 client-reachable SQL call sites
- Root-caused a compound outage (use-after-free in live config reload, zombie crash handler, process-existence health check) and replaced it with a loop-counter liveness probe per world
- Both ends of a signed update chain: server-side release tooling signs 31,000-file patch manifests with RSA-3072-PSS over BLAKE2s-256; the client verifies with a PSS implementation written from RFC 8017 because Unity's Mono rejects the padding mode
- Eight cryptographic primitives implemented against platform limits, each verified against published test vectors; CI runs 12 golden round-trip suites over 11 proprietary binary formats on every push

### Resolve Change — training business platform · [resolvechange.com](https://resolvechange.com)
Self-hosted production system on EC2 (nginx, PM2 under systemd) running a live personal-training
business since 2020 — 274 HTTP handlers, 64-model schema, billing, scheduling, client programs.

- Central authorization module whose ownership predicate *is* the lookup query — closed 8 IDOR vulnerabilities as a class, not one at a time
- Every LLM-generated meal plan gated behind deterministic verification: macros recomputed from the food database, hallucinated items structurally excluded, trainer approval before anything is written
- Replaced a third-party booking vendor with an owned scheduler: webhook-driven two-way Google Calendar sync, HMAC-tokened client self-reschedule, full per-event audit log

### Umbra Studio platform · [umbrastudio.io](https://umbrastudio.io)
Internal business platform — Next.js, Neon Postgres, 47 tables, 27 tRPC routers, 206 procedures.
Tool-using Claude agent (24 tools) under a bounded loop with per-tool error containment, and LLM
cost governance: seven providers priced across five billing units, pre-call budget authorization,
per-call accounting.

Also the origin of a decision I stand behind: I removed an autonomous multi-agent outreach
system after it fabricated claims about real prospects — deleted 11,197 lines across 77 files,
suppressed the affected cohort, repaired 617 records, and replaced autonomy with
typed-confirmation gates and draft review.

### CareBase AI — multi-tenant RAG platform for home care operations · [carebaseai.vercel.app](https://carebaseai.vercel.app)
Section-aware chunking that never splits a care procedure mid-step, intent-conditioned reranking
with neighbor-chunk stitching, permission-scoped retrieval at the vector layer, per-tenant OAuth
document sync (SharePoint / Google Drive) with credentials encrypted at rest. 148 automated
tests; an LLM evaluation harness scoring responses against a golden query set.

### Eidos Fit — AI fitness platform · [eidosfit.com](https://eidosfit.com)
Flutter client against Firebase Cloud Functions. A durable-execution state machine on Firestore
survives the 540-second serverless ceiling by checkpointing week-by-week and resuming from the
last completed week — and the model's video form-analysis scores are discarded and recomputed
server-side from issue severities.

### Client delivery · [anarchyresearch.com](https://anarchyresearch.com)
Live commerce and point-of-sale platform: dual-path payment-processor outage detection designed
to fail open, hosted checkout with HMAC-signed callbacks, card data removed from application
servers.

---

## How I work

Multi-agent development with capability-based tool grants — read-only research and audit roles
structurally denied write access, integration and deployment reserved to the orchestrator. The
workflow is version-controlled policy, not convention: committed agent definitions, an
issue-driven two-role pipeline, model attribution in commit trailers. Verification leans on
determinism and byte-identity — pinned encryption parameters so identical sources compile to
byte-identical artifacts, and an acceptance rule that changed behavior must be observed, not
inferred.

---

**Contact:** [miles@umbrastudio.io](mailto:miles@umbrastudio.io) ·
[linkedin.com/in/mileslewisa](https://linkedin.com/in/mileslewisa) · (575) 637-5411
