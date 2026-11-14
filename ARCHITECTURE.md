# Vibrodo System Architecture

High-level architectural overview of the **Vibrodo Hybrid Audio & Focus Ecosystem**, detailing how on-device media indexing, cloud audio delivery, and real-time room synchronization fit together.

---

## Architectural Topology

```text
  MOBILE CLIENT                    BACKEND SERVICES              INFRA
 ┌──────────────────────┐  HTTPS  ┌─────────────────────┐
 │ Cockpit & Queue UI   │<───────>│ REST API Gateway    │      ┌─────────┐
 │   ^            |     │         │ Auth & Rate Limit ──┼─────>│ MongoDB │
 │   |            v     │         └─────────────────────┘      └─────────┘
 │ Local Index  Player  │   WSS   ┌─────────────────────┐      ┌─────────┐
 │              Engine  │<───────>│ Realtime Sync Hub ──┼─────>│Edge CDN │
 └──────────────────────┘         └─────────────────────┘      └─────────┘
```

---

## 1. Hybrid Source Model

Vibrodo abstracts two fundamentally different audio origins behind a single normalized queue interface:

| Source Layer | Responsibility | Storage & Delivery |
| :--- | :--- | :--- |
| **Cloud Catalog** | Curated mood soundscapes, focus tracks, and artwork metadata | Delivered via Edge Audio CDN with backend metadata caching |
| **Local Index** | Lossless & compressed personal audio (`FLAC`, `WAV`, `ALAC`, `MP3`) | Indexed asynchronously on-device via native media libraries |

Once queued, the playback engine treats cloud streams and local files identically—preserving gapless queue transitions, shuffle/repeat rules, and lock-screen controls regardless of source.

---

## 2. Client Architecture

- **State & Persistence Layer:** Modular state stores manage playback queues, active Pomodoro cycles, and mood preferences, backed by local storage hydration for instant cold starts and offline resilience.
- **Deterministic Playback Engine:** Modeled as an explicit finite state machine (`idle` -> `loading` -> `playing` -> `paused` -> `buffering`) rather than loose boolean flags. Every user action or OS audio interruption triggers a single legal state transition, eliminating desync and stuck-buffer states.
- **Zero-Egress Local Indexing:** Device audio scanning and metadata extraction happen entirely on the client. Personal audio files are never uploaded or transmitted externally.

---

## 3. Realtime Room Synchronization ("Vibing Now")

To enable multiple listeners across varying mobile networks to hear the same moment of a track simultaneously:

1. **Authoritative Server Clock:** Device system clocks drift. All room state mutations (`PLAY`, `PAUSE`, `SEEK`, `TRACK_CHANGE`) are timestamped by the server and broadcast over persistent WebSocket connections.
2. **Target Position Projection:** Clients compute expected playback position from the authoritative event timestamp and network round-trip latency estimates.
3. **Bounded Drift Reconciliation:** Clients periodically audit local playback offset against the projected timeline and apply non-intrusive position corrections only when drift exceeds perceptual thresholds.

*(Note: Proprietary drift-estimation heuristics, adaptive buffering parameters, and multi-layer audio mixing pipelines reside in private repositories.)*

---

## 4. Key Engineering Trade-offs

| Decision | Rationale |
| :--- | :--- |
| **Local-first match priority** | If a queued track exists locally on-device, local playback wins—saving mobile data and guaranteeing instant start. |
| **State machine over boolean flags** | Prevents race conditions between rapid track skips, background transitions, and network jitter. |
| **Edge CDN + cached catalog metadata** | Decouples client playback from third-party API rate limits and ensures predictable stream startup. |

---

<sub>Back to [README.md](./README.md)</sub>
