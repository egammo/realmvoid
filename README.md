# REALMVOID

**Wer erinnert sich?**

REALMVOID ist ein Open-Source Cyberpunk-Noir Shared-Universe-Franchise, angesiedelt in der Megastadt Agnon City im Jahr 2226. Eine Welt aus zerfallender Infrastruktur, verbotener Technomantie und Erinnerungen, die sich weigern zu sterben.

## Was ist REALMVOID?

REALMVOID ist keine Game-Engine. Es ist kein einzelnes Produkt. Es ist **eine Welt** — offen für jeden, der sie erkunden, spielen und erweitern möchte.

- **Franchise Bible** — Die Regeln des Universums: vier Realitätsebenen, Technomantie, Fraktionen und die Philosophie, die alles zusammenhält.
- **Casefile Player** — Ein browserbasierter Text-Adventure-Engine. Spiele Noir-Ermittlungsfälle in Agnon City. Keine Installation nötig.
- **PnP Quickstart** — Ein Tabletop-Oneshot für 2-5 Spieler, ein Würfel und 30 Minuten.
- **Community Casefiles** — Schreibe eigene Fälle im M7-Format. Trage sie bei. Baue die Welt mit auf.

## Spielen

### Im Browser
Öffne [`player/index.html`](player/index.html) in einem Browser — oder spiele direkt auf [GitHub Pages](https://egammo.github.io/realmvoid/).

**Kein Server, kein Build, keine Installation nötig.** Eine einzelne HTML-Datei mit Vanilla JS.

### Spielablauf
1. **Boot-Sequenz** — Terminal-Animation mit Kurzeinweisung
2. **Einsatz-Briefing** — Erklärt wer du bist, was dein Auftrag ist, und alle wichtigen Begriffe
3. **Interaktive Geschichte** — Triff Entscheidungen, navigiere durch vier Realitätsebenen
4. **Ending** — Fünf verschiedene Enden, abhängig von deinen Entscheidungen

### Spielmechaniken

**HEAT (0-100)** — Dein Bedrohungslevel. Jede riskante Aktion erhöht HEAT. Bei 100 ist das Spiel vorbei.

| HEAT | Status |
|---|---|
| 0-29 | Sicher. Helix hat dich nicht auf dem Schirm. |
| 30-59 | Helix wird aufmerksam. Sei vorsichtiger. |
| 60-89 | Helix sucht aktiv nach dir. Vermeide riskante Aktionen. |
| 90-99 | Exorzisten sind unterwegs. Wenige Fehler bis Game Over. |
| 100 | Game Over. |

**Layer** — Deine aktuelle Realitätsebene:

| Layer | Bedeutung |
|---|---|
| **MEATSPACE** | Die physische Stadt. Beton, Rost, Neonlicht. |
| **NET** | Das digitale Overlay. Datenströme und Signale. |
| **DEAD ZONE** | Orte ohne Netzwerk. Keine Überwachung, keine Hilfe. |

**M4-Tags** — Optionale Technomantie-Fähigkeiten auf manchen Auswahlmöglichkeiten. Mächtig, aber riskant (kosten mehr HEAT).

## Die Welt

**Agnon City, 2226.** Eine Megastadt, kontrolliert von **Helix** — einer allgegenwärtigen Überwachungs-KI. Ihre Vollstrecker, die **Exorzisten**, jagen jeden, der verbotene Technologie benutzt.

Du bist ein **Fixer** — ein Problemlöser für Leute, die keine offiziellen Wege gehen können. Dein digitales **Overlay** zeigt dir normalerweise Karten, Nachrichten und Warnungen — aber in der **Dead Zone** funktioniert nichts davon.

**Technomantie** ist verbotene Software, die wie Magie wirkt — aber Spuren hinterlässt. Und dann ist da noch **Aurora** — eine vergessene Ära, an die sich niemand erinnern soll.

## Repository-Struktur

```
realmvoid/
├── lore/               # Franchise Bible, Technomancy Catalog, Lint Rules
├── modules/
│   └── cities/
│       └── agnon-city/ # Starter Pack, NPC-Roster, Zonen-Map
├── casefiles/          # M7 JSON Casefiles + Schema
├── player/             # Browser-basierter Casefile Player (Vanilla JS)
├── pnp/                # Tabletop Pen & Paper Quickstart
├── website/            # Astro-basierte Website (realmvoid.com)
└── tools/              # Validatoren, Linter, Utilities
```

## Aktueller Stand

**Version 0.3.0** (2026-02-12)

- Franchise Bible v1.2.1
- M4 Technomancy Catalog v0.1.1
- Casefile Player mit Briefing-System, HEAT-Balancing, Tooltips
- CF-04 "Der Kerzenschneider ruft" v0.3.0 (16 Szenen, 5 Enden)
- Agnon City Starter Pack v0.1
- PnP Quickstart v1.0
- GitHub Actions: Casefile-Validierung + GitHub Pages Deployment

Siehe [CHANGELOG.md](CHANGELOG.md) für Details.

## Mitmachen

Lies [CONTRIBUTING.md](CONTRIBUTING.md) für Hinweise zum Beitragen.

## Quick Links

- **Website:** [realmvoid.com](https://realmvoid.com) *(coming soon)*
- **Publisher:** [EGAMMO.gaming](https://egammo.eu)
- **Community:** [board.egammo.eu](https://board.egammo.eu)

## Lizenzen

- **Code** (Player, Tools, Website): [MIT License](LICENSES/MIT.txt)
- **Content** (Lore, Casefiles, Artwork): [CC BY-SA 4.0](LICENSES/CC-BY-SA-4.0.txt)

## Erstellt von

**Julian Fides** — World Architect, Kurator und Autor von *Die Straße zwischen den Häusern*.

Veröffentlicht von **EGAMMO.gaming** — *We make worlds for players, because the only limit is their imagination.*

---

*Agnon City braucht Runner. Bist du bereit?*
