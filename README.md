# REALMVOID

**Who remembers?**

REALMVOID is an open-source cyberpunk-noir shared universe franchise set in the megacity Agnon City, 2226 AD. A world of crumbling infrastructure, forbidden technomancy, and memories that refuse to die.

> **[Deutsche Version / German Version](README_DE.md)**

## What is REALMVOID?

REALMVOID is not a game engine. It's not a single product. It is **a world** — open for anyone to explore, play, and expand.

- **Franchise Bible** — The rules of the universe: three reality layers, technomancy, factions, and the philosophy that holds it all together.
- **Casefile Player** *(Tech Demo)* — A browser-based interactive fiction engine. Play noir investigations in Agnon City. No installation required. Demonstrates the M7 casefile format and the Layer/HEAT mechanics.
- **PnP Quickstart** — A tabletop one-shot for 2-5 players, one die, and 30 minutes.
- **Community Casefiles** — Write your own cases in M7 format. Contribute them. Help build the world.

## Play

### In the Browser *(Tech Demo)*
Open [`player/index.html`](player/index.html) in a browser — or play directly at [realmvoid.com/player/](https://realmvoid.com/player/).

**No server, no build, no installation required.** A single HTML file with vanilla JS.

> **Note:** The Casefile Player is a tech demo showcasing the M7 JSON format. It demonstrates how casefiles work (layer switching, HEAT system, branching narratives) but is not a finished product. Feedback and contributions are welcome.

### How to Play
1. **Boot Sequence** — Terminal animation with a quick tutorial
2. **Mission Briefing** — Explains who you are, what your mission is, and key terms
3. **Interactive Story** — Make choices, navigate across reality layers
4. **Ending** — Five different endings depending on your decisions

### Mechanics

**HEAT (0-100)** — Your threat level. Every risky action raises HEAT. At 100, it's game over.

| HEAT | Status |
|---|---|
| 0-29 | Safe. Helix doesn't know you exist. |
| 30-59 | Helix is paying attention. Be careful. |
| 60-89 | Helix is actively hunting you. Avoid risky actions. |
| 90-99 | Exorcists inbound. One wrong move left. |
| 100 | Game over. |

**Layer** — Your current reality layer:

| Layer | Meaning |
|---|---|
| **MEATSPACE** | The physical city. Concrete, rust, neon. |
| **NET** | The digital overlay. Data streams and signals. |
| **DEAD ZONE** | Places without network. No surveillance, no help. |

**M4 Tags** — Optional technomancy abilities on some choices. Powerful but risky (costs more HEAT).

## The World

**Agnon City, 2226.** A megacity controlled by **Helix** — an omnipresent surveillance AI. Its enforcers, the **Exorcists**, hunt anyone who uses forbidden technology.

You are a **Fixer** — a problem solver for people who can't go through official channels. Your digital **Overlay** normally shows you maps, messages, and warnings — but in the **Dead Zone**, none of that works.

**Technomancy** is forbidden software that feels like magic — but leaves traces. And then there's **Aurora** — a forgotten era that no one is supposed to remember.

## Repository Structure

```
realmvoid/
├── lore/                  # Franchise Bible, Technomancy Catalog, Lint Rules
├── modules/
│   └── cities/
│       └── agnon-city/    # Starter Pack, NPC Roster, Zone Map
├── casefiles/             # M7 JSON Casefiles + Schema
│   └── community/         # Community-contributed Casefiles
├── player/                # Browser-based Casefile Player (Vanilla JS)
├── pnp/                   # Tabletop Pen & Paper Quickstart
├── website/               # Astro-based website (realmvoid.com)
└── tools/                 # Validators, Linters, Utilities
```

## Current Status

**Version 0.3.0** (2026-02-12)

- Franchise Bible v1.2.1
- M4 Technomancy Catalog v0.1.1
- Casefile Player with briefing system, HEAT balancing, tooltips
- CF-04 "Der Kerzenschneider ruft" v0.3.0 (16 scenes, 5 endings)
- Agnon City Starter Pack v0.1
- PnP Quickstart v1.0
- GitHub Actions: Casefile validation + GitHub Pages deployment

See [CHANGELOG.md](CHANGELOG.md) for details.

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Quick Links

- **Website:** [realmvoid.com](https://realmvoid.com)
- **Publisher:** [EGAMMO.gaming](https://egammo.eu)
- **Community:** [board.egammo.eu](https://board.egammo.eu)

## Licenses

- **Code** (Player, Tools, Website): [MIT License](LICENSES/MIT.txt)
- **Content** (Lore, Casefiles, Artwork): [CC BY-SA 4.0](LICENSES/CC-BY-SA-4.0.txt)

## Created by

**Julian Fides** — World Architect, Curator, and author of *Die Straße zwischen den Häusern* (The Street Between the Houses).

Published by **EGAMMO.gaming** — *We make worlds for players, because the only limit is their imagination.*

---

*Agnon City needs runners. Are you ready?*
