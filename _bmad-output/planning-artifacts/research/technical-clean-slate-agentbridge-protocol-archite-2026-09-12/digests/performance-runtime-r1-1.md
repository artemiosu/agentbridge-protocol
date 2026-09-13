# Digest: performance, transport, encoding, and runtime

Accessed: 2026-09-12

Decision signal: public evidence provisionally favors a **native semantic core with optional, fail-closed transport/encoding bridges**. Transport, encoding, and reference-language choices must remain replaceable until representative benchmarks exist.

## Protocol versus implementation decisions

| Protocol layer | Implementation layer |
| --- | --- |
| Framing, limits, canonical signed representation | Parser/library, allocations, zero-copy techniques |
| Idempotency, retry eligibility, deduplication, 0-RTT policy | Retry scheduler, queues, worker pools |
| Stream cancellation/error semantics and recovery | Async runtime, task scheduling, buffer reuse |
| Backpressure visibility and flow-control contract | Window-update heuristics and buffer sizes |
| Compression negotiation/context separation/limits | Codec implementation, level, dictionary, hardware acceleration |
| H2/H3 bindings and fallback behavior | Congestion controller, packetization, socket/runtime tuning |
| Cross-SDK conformance semantics | Rust, Go, TypeScript/Node, Python implementation |

## Findings

1. QUIC+TLS has a full 1-RTT handshake and resumed 0-RTT, but early data is replayable. Side-effecting AgentBridge operations should reject 0-RTT unless their replay semantics are explicitly safe; disabling early data is the strongest defense.
   - Source: IETF/RFC Editor, RFC 9001 — https://www.rfc-editor.org/rfc/rfc9001.html
   - Status: Standards Track, 2021-05
   - Confidence/class: high; protocol fact

2. HTTP/3 removes TCP-level cross-stream head-of-line blocking, but congestion control remains connection-wide, control/QPACK dependencies remain, and blocked UDP can prevent establishment. HTTP/3 clients are advised to fall back to TCP-based HTTP.
   - Source: IETF/RFC Editor, RFC 9114 — https://www.rfc-editor.org/rfc/rfc9114.html
   - Status: Standards Track, 2022-06
   - Confidence/class: high; protocol fact/deployment limitation

3. QUIC flow control does not solve application backpressure automatically. Length-prefixed messages larger than available credit can deadlock when receivers wait for a full message; interdependent streams can also deadlock. Incremental consumption, bounded partial-message state and cancellation semantics belong in the application protocol.
   - Source: IETF/RFC Editor, RFC 9308 — https://www.rfc-editor.org/rfc/rfc9308.html
   - Status: Informational, 2022
   - Confidence/class: high; protocol failure mode

4. CBOR supports binary strings and deterministic encoding through shortest representations, definite lengths and ordered encoded keys. Determinism must be explicitly profiled before CBOR is a signable wire representation.
   - Source: IETF/RFC Editor, RFC 8949 — https://www.rfc-editor.org/rfc/rfc8949.html
   - Status: Standards Track, 2020-12
   - Confidence/class: high; encoding standard

5. JCS creates repeatable JSON signing bytes using I-JSON constraints, ECMAScript serialization and recursive property sorting. This establishes interoperability, not demonstrated performance superiority.
   - Source: RFC Editor, RFC 8785 — https://www.rfc-editor.org/rfc/rfc8785.html
   - Status: Informational, 2020-06
   - Confidence/class: high for behavior; no measured cost retrieved

6. Raw Protocol Buffers bytes are unsuitable as permanent cross-language signature/hash input: official documentation says deterministic serialization is not canonical and may vary by schema, build, library or application.
   - Source: Protocol Buffers / Google, “Proto Serialization Is Not Canonical” — https://protobuf.dev/programming-guides/serialization-not-canonical/
   - Status: current official documentation
   - Confidence/class: high; format constraint

7. Two independent 2026 studies report binary-encoding advantages over JSON on their own workloads but identify different strengths. One reports up to 80% size reduction with CBOR and up to 13.8% loading improvement for large tested objects; another finds Cap’n Proto smallest and Protobuf strong for encoding/decoding in LoRaWAN/MQTT-SN. The transferable finding is workload dependence, not a universal winner.
   - Sources:
     - IEEE TNSM, “A Leaner and Faster Web” — https://arxiv.org/abs/2512.12067
     - AIP Conference Proceedings, “Optimizing data transmission efficiency in IoT systems” — https://doi.org/10.1063/5.0322487
   - Status: peer-reviewed, 2026
   - Confidence/class: medium and low-medium for transfer to agent RPC; benchmark

8. Compression requires protocol-level resource/security rules. HTTP zstd bounds decoder windows at 8 MB, while HTTP/3 forbids sharing a compression context between confidential and attacker-controlled data. Compression should be opt-in, thresholded, context-separated, and fail closed on unsupported resource requirements.
   - Sources: IETF/RFC Editor, RFC 9659 — https://www.rfc-editor.org/rfc/rfc9659.html; RFC 9114 — https://www.rfc-editor.org/rfc/rfc9114.html
   - Status: Informational 2024-09; Standards Track 2022-06
   - Confidence/class: high; resource/security rules

9. Rust async is credible for a high-concurrency core but performance depends on runtime choice. Official documentation notes compatibility constraints, blocking/future failure modes, and maintenance burden; language reputation is insufficient evidence.
   - Source: Rust Project, “The State of Asynchronous Rust” — https://rust-lang.github.io/async-book/01_getting_started/03_state_of_async_rust.html
   - Status: current official documentation
   - Confidence/class: high for tradeoffs; no cross-language ranking

10. Go is also credible, with workload-dependent GC CPU, memory and latency tradeoffs including short stop-the-world transitions, GC scheduling delays, allocation assists and a soft memory limit. Bursts, queues and large messages must be benchmarked.
    - Source: Go Project, “A Guide to the Go Garbage Collector” — https://go.dev/doc/gc-guide
    - Status: current official documentation
    - Confidence/class: high for runtime behavior; no cross-language ranking

11. A language comparison should use identical cross-language workers and report unconstrained and saturated latency/QPS plus per-core scalability. The gRPC harness demonstrates the method for Go, Node and Python secure Protobuf workloads; it is methodological evidence, not a current language ranking.
    - Source: gRPC/CNCF, “Benchmarking” — https://grpc.io/docs/guides/benchmarking/
    - Status: living documentation, materially documented 2022
    - Confidence/class: medium; benchmark-method lead

## Architecture implications

- HTTP/3 is an optional binding until it wins representative loss/RTT tests and proves TCP fallback.
- Prohibit 0-RTT for externally visible effects unless the action is explicitly replay-safe.
- JSON/JCS, deterministic CBOR and schema-driven binary encodings must compete; no winner is established.
- Rust and Go are reference-core finalists. TypeScript/Node and Python remain first-class interop/SDK targets.
- A profile/composition candidate should add no steady-state round trip; profile material should be cached or provisioned.

## Gate 0 benchmark matrix

| Dimension | Required cases |
| --- | --- |
| Transport | H2+TLS/TCP; H3+QUIC; warm/cold; resumed without early data; 0-RTT only for replay-safe controls |
| Composition | Preprovisioned profile; once-per-connection negotiation; per-call control; missing/stale/mismatched profile |
| Network | RTT 0.2/5/30/100/250 ms; loss 0/0.1/1/3/5%; reorder; constrained MTU; UDP blocked; migration/NAT rebinding |
| RPC | Unary; request/response/bidirectional streams; fan-out; long idle; mid-message cancellation |
| Payload | 128 B through 4 MiB; flat/nested; key-count extremes; repeated strings; binary; unknown fields |
| Encoding | Minified JSON; JCS JSON; preferred/deterministic CBOR; MessagePack; Protobuf |
| Signing | Parse/canonicalize/hash/sign/verify separately; valid/invalid; reordering; unknown fields; Unicode/numeric edges; batch |
| Compression | None/gzip/zstd; thresholds; dictionary; precompressed; secret+attacker input; oversized window/expansion bomb |
| Backpressure | Slow consumer; bounded buffers; message over credit; interdependent streams; ignored credit; cancel while blocked |
| Recovery | Fail before send/application, after effect, after headers, mid-stream; retry policy; dedup hit/miss; amplification |
| Implementations | Rust with pinned runtime; Go; Node/TypeScript; Python; identical semantics, fixtures, crypto and transport settings |
| Metrics | p50/p95/p99/p99.9; handshake RTT/packets; QPS/core; CPU; RSS; allocations; wire bytes; queue; fallback; recovery |
| Correctness | Golden-vector equality; malformed/deep/oversized rejection; cancellation/error equivalence; version/unknown fields |
| Reproducibility | Pinned toolchain/library/hardware/kernel/cipher/congestion settings; warm-up; samples; confidence intervals; raw data |

Policy gates proposed by the researcher: 100% golden-vector/cross-language conformance; no bounded-flow deadlock; configured memory caps under hostile input; no side effect through replayable 0-RTT; working TCP fallback for H3; performance optimization only with a preregistered material win and no unacceptable p99/RSS/compatibility/failure regression.

## Searches with no useful evidence

- No recent identical-method operator comparison of H2 and H3 for RPC/agent traffic.
- No current benchmark jointly compares JSON, deterministic CBOR, MessagePack and Protobuf while isolating canonicalization/signing.
- No current identical secure-workload comparison covers Rust, Go, Node and Python.
- No performance evidence for a permanent agent-protocol composition mechanism.
- MessagePack canonical signing evidence was insufficient.
- Language microbenchmarks and vendor “fastest” claims were excluded.
