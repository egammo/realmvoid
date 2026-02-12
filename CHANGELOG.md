# Changelog

All notable changes to REALMVOID will be documented in this file.

## [0.3.0] - 2026-02-12

### Changed
- **Casefile CF-04 v0.3.0** — Alle Szenen-Texte komplett neu geschrieben: konkret, verständlich, mit Noir-Atmosphäre aber ohne kryptische Metaphern. Jede Szene erklärt jetzt klar was passiert, wo man ist und warum.
- **HEAT-Balancing** — Alle heat_delta-Werte halbiert. Vorsichtiger Pfad endet bei ~54 HEAT statt 120+. Rücksichtsloses Spielen führt weiterhin zu Game Over.
- **HEAT-Warnungen** — Von kryptischen Codes ("AUDIT-PROTOKOLL AKTIV") zu verständlichen Sätzen ("Helix sucht aktiv nach dir. Vermeide riskante Aktionen!").
- **Ending-Texte** — Klarer formuliert mit konkreten Konsequenzen statt reiner Poesie.

### Added
- **Briefing-Screen** — Neuer Einsatz-Briefing-Bildschirm zwischen Boot-Sequenz und Spiel. Erklärt Rolle (Fixer), Auftraggeber (Kroll), Ziel und alle Schlüsselbegriffe (Helix, Exorzisten, Overlay, Dead Zone, Technomantie, Aurora, HEAT).
- **Ending-Status-Badges** — Jedes Ende zeigt jetzt eine klare Bewertung: MISSION ABGESCHLOSSEN (grün), MISSION ABGEBROCHEN (cyan), GESCHEITERT (rot), MISSION AUFGEGEBEN, KERN VERKAUFT.
- **HEAT-Tooltip** — Klickbare Erklärung auf der HEAT-Leiste. Wird beim Spielstart automatisch 8 Sekunden lang angezeigt.
- **M4-Technomantie-Tooltips** — Hover-Erklärungen für M4-Tags auf Auswahlbuttons (z.B. "Totenwache — RAM-Dump, Signale aus toten Systemen lesen").
- **Boot-Screen Tutorial** — Kurzeinweisung zu HEAT, Layern und M4-Tags vor dem Spielstart.

### Fixed
- **heat_range-Bug** — Spieler-gewählte Endings zeigen jetzt immer das korrekte Ende. Vorher konnte es passieren, dass "Kern an Kroll übergeben" das "Kern behalten"-Ende anzeigte, weil der HEAT-Wert außerhalb der heat_range lag.
- **HEAT >= 100 Priorität** — Game-Over-Check hat jetzt Vorrang vor der Ending-ID-Suche.
- **aftermath heat_range erweitert** — end_deliver von "0-40" auf "0-70", end_walk_away von "0-10" auf "0-40" (als Metadaten beibehalten).

## [0.1.0] - 2026-02-11

### Added
- Initial repository structure
- README with project overview
- Dual licensing: MIT (Code) + CC BY-SA 4.0 (Content)
- Contributing guidelines
- Directory structure for lore, casefiles, player, pnp, website, tools
- Franchise Bible v1.2.1
- M4 Technomancy Catalog v0.1.1
- Agnon City Starter Pack v0.1
- M7 Casefile JSON Schema v0.1
- CF-04 "Der Kerzenschneider ruft" (Pilot Casefile)
- Casefile Player Engine v0.1 (Vanilla JS, single HTML file)
- PnP Quickstart v1.0
- GitHub Actions: Casefile-Validierung + GitHub Pages Deployment
