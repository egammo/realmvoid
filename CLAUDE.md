# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## REALMVOID — Das Ökosystem

Dachordner für alle drei Kernprojekte. Kein Code hier — nur die Struktur.

```
REALMVOID/
├── AXWire/          → Protokoll-Spec (sprachunabhängig)
├── realmvoid-os/    → Infrastruktur (Relay, Registry, Vertex)
└── agnon-city/      → Erstes Referenzspiel (AGNON City 2226)
```

## Hierarchie

```
AXWire        →  "Was ist die Sprache?"      (Spec)
realmvoid-os  →  "Wie ist das Netz gebaut?"  (Infrastruktur)
agnon-city    →  "Was läuft darauf?"         (Anwendung)
```

## GitHub (Stand 2026-02-26)

Alle Repos eingerichtet und gepusht:

| Projekt | GitHub | Lokal (eigenes .git) |
|---|---|---|
| REALMVOID Codex | `egammo/realmvoid` | `REALMVOID/` selbst |
| AXWire | `egammo/AXWire` | `REALMVOID/AXWire/` |
| realmvoid-os | `egammo/realmvoid-os` | `REALMVOID/realmvoid-os/` |
| agnon-city | `egammo/agnon-city` | `REALMVOID/agnon-city/` |

Sub-Repos sind in `.gitignore` von `REALMVOID/` ausgeschlossen.

## Projektstand (Feb 2026)

| Projekt | Status |
|---|---|
| AXWire | Spec v1.0 fertig (Draft), auf GitHub |
| realmvoid-os | Phase 0 abgeschlossen, Phase 1 (Go) ausstehend |
| agnon-city | Konzept & Docs fertig, kein Code |

## Verwandtes

| Ordner | Was es ist |
|---|---|
| `../REALMVOID-Legacy/` | Altes REALMVOID-Projekt (AGNON-HIVE Basis, archiviert) |
| `../NEOVOID/` | Kreatives Open-Source Shared Universe (Cyberpunk-Noir, eigenständig) |
