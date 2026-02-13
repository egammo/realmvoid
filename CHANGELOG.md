# Changelog

All notable changes to REALMVOID will be documented in this file.

## [0.3.0] - 2026-02-12

### Changed
- **Casefile CF-04 v0.3.0** — All scene texts completely rewritten: concrete, understandable, with noir atmosphere but without cryptic metaphors. Each scene now clearly explains what happens, where you are, and why.
- **HEAT Balancing** — All heat_delta values halved. Cautious path now ends at ~54 HEAT instead of 120+. Reckless play still leads to game over.
- **HEAT Warnings** — Changed from cryptic codes ("AUDIT-PROTOKOLL AKTIV") to understandable sentences ("Helix is actively hunting you. Avoid risky actions!").
- **Ending Texts** — Rewritten with concrete consequences instead of pure poetry.

### Added
- **Briefing Screen** — New mission briefing screen between boot sequence and game. Explains your role (Fixer), employer (Kroll), objective, and all key terms (Helix, Exorcists, Overlay, Dead Zone, Technomancy, Aurora, HEAT).
- **Ending Status Badges** — Each ending now shows a clear assessment: MISSION COMPLETE (green), MISSION ABORTED (cyan), FAILED (red), MISSION ABANDONED, CORE SOLD.
- **HEAT Tooltip** — Clickable explanation on the HEAT bar. Automatically shown for 8 seconds at game start.
- **M4 Technomancy Tooltips** — Hover explanations for M4 tags on choice buttons (e.g. "Dead Watch — RAM dump, reading signals from dead systems").
- **Boot Screen Tutorial** — Quick tutorial covering HEAT, layers, and M4 tags before game start.

### Fixed
- **heat_range Bug** — Player-chosen endings now always show the correct ending. Previously, "deliver core to Kroll" could display the "keep core" ending because HEAT was outside the heat_range.
- **HEAT >= 100 Priority** — Game over check now takes priority over ending ID lookup.
- **aftermath heat_range expanded** — end_deliver from "0-40" to "0-70", end_walk_away from "0-10" to "0-40" (kept as metadata).

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
- GitHub Actions: Casefile validation + GitHub Pages deployment
