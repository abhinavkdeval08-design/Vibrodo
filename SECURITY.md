# Security & Privacy Policy

Vibrodo is engineered with a **Security-by-Design** and **Zero-Egress Local Media** philosophy.

---

## 1. Data & Media Privacy

- **Zero-Egress Local Audio:** Scanning and indexing local audio files (`FLAC`, `WAV`, `ALAC`, `MP3`) happens strictly on-device. Personal audio files and local file paths are never uploaded to Vibrodo servers.
- **Minimal Account Footprint:** Cloud accounts store only essential authentication credentials, synced track identifiers, and focus session preferences. We do not sell or monetize user listening behavior.

---

## 2. Application & Infrastructure Security

- **Authentication & Password Hygiene:** Passwords are salted and hashed with `bcrypt` prior to persistence. Session tokens are signed via short-lived JSON Web Tokens (`JWT`) over TLS.
- **Edge Rate Limiting & Isolation:** Public API gateways enforce per-client rate limiting and strict payload validation to protect backend services and external quotas.
- **Private Core Repositories:** Production source code, environment configurations, and proprietary synchronization engines are isolated in private repositories with strict secret-scanning rules.

---

## 3. Reporting a Vulnerability

If you discover a security issue or privacy concern in Vibrodo's mobile client, backend APIs, or realtime room gateway, please report it responsibly before public disclosure:

- **Security Contact:** `abhinavceovibrodo@gmail.com`
- **Subject Line:** `[SECURITY] Vulnerability Report`

We acknowledge valid reports within **48 hours** and coordinate remediation prior to any public advisory.

---

<sub>Back to [README.md](./README.md)</sub>
