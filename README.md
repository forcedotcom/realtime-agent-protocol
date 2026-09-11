# Agentforce Live A2A Profile Extension

This repository defines an optional A2A profile for real-time, steerable voice and
multimodal conversations. It standardizes the boundary between a media-facing
**Live layer** and a reasoning/workflow **Reasoner**: transcripts go in; streamed
spoken output and structured conversation directives come out.

The profile is additive to core A2A. A compatible client and server negotiate it
through the Agent Card and the `A2A-Extensions` header; peers that do not implement
it continue to interoperate through ordinary A2A messages, artifacts, and task
states.

## Specification

[`spec.md`](spec.md) is the normative v0.1 draft. It defines:

* the extension URI and Agent Card advertisement;
* extension activation and client/server directive capability negotiation;
* mapping of calls to `contextId` and utterances to A2A tasks;
* streamed text artifacts, control directives, handoffs, interruption, and history
  backfill; and
* core-A2A fallback behavior.

The v0.1 profile is text-in/text-and-directives-out. ASR, TTS, media transport, and
call control remain the Live layer's responsibility.

## What this repository still needs

`spec.md` is the contract, but it is not sufficient for interoperable production
implementations. The following artifacts should be added before declaring a stable
release:

| Artifact | Why it is needed |
| --- | --- |
| JSON Schemas | Machine-validate Agent Card extension parameters, metadata, all directive payloads, interruption, and history updates. Publish versioned schema URLs. |
| Conformance suite | Make the normative requirements executable: negotiation, ordering, task lifecycle, invalid payloads, fallback, and cancel/interrupt cases. |
| Reference fixtures | Provide valid and invalid JSON-RPC/WebSocket transcripts, Agent Cards, and expected event sequences for every directive. |
| Reference implementation(s) | Demonstrate both Live-client and Reasoner-server behavior, including event ordering and reconnection boundaries. |
| Compatibility matrix | State the exact A2A baseline(s), SDK versions, transport support, and `A2A-Extensions` versus `X-A2A-Extensions` behavior. |
| Security profile | Specify authentication, authorization, tenant isolation, PII redaction/retention, rate limits, audit events, and WebSocket/TLS requirements. |
| Error and retry policy | Define stable error codes, retryability, idempotency/replay semantics, timeouts, and delivery guarantees. |
| Escalation design | Finalize the neutral escalation payload and the transfer-outcome handshake without coupling to a specific contact-center routing system. |
| Operational profile | Define observability fields, metrics, trace propagation, limits, backpressure, graceful drain, reconnect, and resubscribe behavior. |
| Governance | Add versioning and extension-evolution rules, change control, owners, contribution guidance, and a license. |

## Suggested repository layout

```text
spec.md                         # normative profile specification
schemas/                        # versioned JSON Schema documents
fixtures/                       # Agent Cards and wire-level examples
conformance/                    # executable interoperability tests
examples/live-client/           # minimal Live-layer client
examples/reasoner-server/       # minimal Reasoner server
docs/compatibility.md           # A2A/SDK/transport support
docs/security.md                # threat model and controls
docs/operations.md              # observability and lifecycle behavior
CHANGELOG.md
CONTRIBUTING.md
LICENSE
```

## Current open design items

The v0.1 draft intentionally leaves media-carrying A2A, graceful connection drain
and replay, and a portable escalation routing/transfer-result contract for later
versions. These need resolution alongside the schemas and conformance suite before
a production interoperability commitment.
