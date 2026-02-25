# REALMVOID Codex

> Federated Game-Universe OS — Architecture Specification & Design Papers

REALMVOID is a federated infrastructure for persistent, sovereign game worlds. It defines how independent game realms communicate, authenticate, and federate — without central control.

This repository is the **conceptual core**: manifests, principles, and specifications that all REALMVOID implementations must follow.

## Ecosystem

| Repo | What it is |
|---|---|
| **egammo/realmvoid** ← *you are here* | Codex — architecture papers, philosophy, MAB spec |
| [egammo/axwire](https://github.com/egammo/axwire) | AXWire — binary transport protocol spec |
| [egammo/realmvoid-os](https://github.com/egammo/realmvoid-os) | Reference implementation (Go) |
| [egammo/agnon-city](https://github.com/egammo/agnon-city) | First game world running on REALMVOID |

## Core Concepts

**MAB (Minimal Architectural Blueprint)** — Every REALMVOID component exposes exactly 5 functions: `Create`, `Destroy`, `Tick`, `Send`, `Poll`. No more, no less.

**Pirelli Principle** — Power + Control + Clarity. Every architectural decision is measured against these three axes.

**Realm / Vertex Model** — A Realm is a persistent world. A Vertex is a node in the federation. Realms live on Vertices. Vertices talk via AXWire.

**Star Topology / OR-Circuit** — Relay nodes route between realms. No relay is a gatekeeper — traffic can always find an alternate path.

**Player addressing** — `player@relay-domain` — like email, but for game identities.

## Documents

| Document | Content |
|---|---|
| [`docs/REALMVOID_Manifest_v0_2.md`](docs/REALMVOID_Manifest_v0_2.md) | Architecture manifest — the full vision |
| [`docs/REALMVOID_Pirelli_Prinzip_v0_1.md`](docs/REALMVOID_Pirelli_Prinzip_v0_1.md) | Pirelli Principle — core philosophy |
| [`docs/REALMVOID_Spezifikation_v0_4.md`](docs/REALMVOID_Spezifikation_v0_4.md) | Master specification v0.4 |
| [`docs/REALMVOID_MAB_ABI_Manifest_v0_1.md`](docs/REALMVOID_MAB_ABI_Manifest_v0_1.md) | MAB-ABI contract definition |
| [`docs/REALMVOID_Realm_Vertex_Model_v0_1.md`](docs/REALMVOID_Realm_Vertex_Model_v0_1.md) | Realm / Vertex model |
| [`docs/REALMVOID_Konzept_Overview_v0_1.md`](docs/REALMVOID_Konzept_Overview_v0_1.md) | Concept overview (historical) |

## License

MIT — implement freely, in any language.

---

*Built by [EGAMMO.gaming](https://egammo.eu)*
