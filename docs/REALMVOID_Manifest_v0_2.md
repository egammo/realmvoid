# REALMVOID Manifest v0.2

> **REALMVOID** ist ein *indieskaliges Spiele‑Universums‑OS* – kein Game‑Engine‑Ersatz, kein „mach alles für mich“-Framework.  
> Es liefert die **technische Infrastruktur**, in der viele Spiele und Welten leben können.

## REALMVOID als Volkssprache der VORTEXe

REALMVOID versteht sich nicht nur als technisches Framework, sondern als gemeinsame „Volkssprache“ eines ganzen Universums.  
Die REALMVOID-API (inkl. AXWire) definiert die grundlegende Art und Weise, wie Entitäten denken, sprechen und einander wahrnehmen können – unabhängig davon, aus welchem Realm, MAB oder VORTEX sie stammen.

### REALMVOID-API & AXWire: die Volkssprache

Die REALMVOID-API bildet die gemeinsame Semantik, AXWire den Transport dieser Semantik. Zusammen fungieren sie als „volkssprachlich / zum Volk gehörig“ im Sinne von **diutisc**:

- Jede Entität, die REALMVOID-API spricht, gehört zum „Volk“ des REALMVOID-Universums.
- AXWire stellt sicher, dass diese Sprache überall verstanden werden kann – über Prozess-, Maschinen- und Realmgrenzen hinweg.
- Neue Features erscheinen als Erweiterungen des gemeinsamen Wortschatzes, nicht als Fremdsprache.

REALMVOID definiert damit nicht nur Schnittstellen, sondern eine **linguistische Schicht für Maschinen**: ein Vokabular für Zustände, Ereignisse, Identitäten und Relationen.

### MABs & VORTEXe: Völker und Dialekte

MABs, Realms und VORTEXe repräsentieren die „Völker“ des REALMVOID-Universums:

- Jedes MAB kann seine eigene Kultur, Regeln und innere Logik haben.
- Jeder VORTEX kann als eigenes „Volk“ mit spezifischen Dialekten, Konventionen und Sichtweisen auftreten.
- Trotzdem bleiben alle Teil des gleichen Ganzen, solange sie REALMVOID-API & AXWire sprechen.

Die Unterschiede zwischen MABs und VORTEXen entsprechen Dialekten:  
lokale Regeln, spezielle Typen oder Events – aber immer rückführbar auf die gemeinsame REALMVOID-Semantik.

### Verschwundene Völker & Archiv-Sprache

REALMVOID berücksichtigt auch „verschwundene Völker“:

- Deaktivierte oder verlassene MABs und VORTEXe bleiben als Schemas, Snapshots und Protokolle im Archiv erhalten.
- Alte Versionen der REALMVOID-API werden nicht gelöscht, sondern historisiert – wie tote Sprachen, die weiterhin lesbar bleiben.
- Kompatibilitätsschichten fungieren als Übersetzer zwischen alten und neuen Dialekten.

So wird das REALMVOID-Universum im Kern zu einem **„Land derjenigen, die REALMVOID sprechen“** –  
ein Raum, in dem selbst vergangene Völker noch verstanden und in neue Kontexte übersetzt werden können.

Diese Version v0.2 präzisiert vor allem:

- die **Stern‑Topologie** (REALMVOID Core in der Mitte, MABs als unabhängige Äste),
- die Rolle von **Go** als Referenz‑Implementierung,
- die Rolle von **Go, C/C++ und C#** als Zielsprachen für Devs,
- und die Trennung zwischen **Langzeit‑Vision** und **v0.x‑Umsetzung**.

---

## 1. Zweck & Zielgruppe

REALMVOID adressiert ein wiederkehrendes Problem:

> *„Ich will Spiele bauen, deren Logik und Architektur  
> länger überlebt als die aktuelle Engine oder das Netzwerk‑Setup.“*

REALMVOID ist gedacht für:

- **Indie‑Devs und kleine Teams**
- typische Last: **100–500 CCU pro Welt/Vertex**, nicht Millionen
- Menschen, die **Kontrolle und Klarheit** bevorzugen statt magischer AAA‑Frameworks

---

## 2. Mentales Modell

Stell dir REALMVOID wie eine **Stern‑Verkabelung** vor:

- In der Mitte sitzt der **REALMVOID Core** (die „Spinne im Netz“).
- An den Ästen hängen **MABs** (*Minimal Architectural Blueprints*), jeweils mit einem klaren Zweck.
- Außen sitzen die **Host‑Umgebungen** (Game‑Engines, Tools, Services in Go, C/C++, C#).

ASCII‑Bild:

```text
           [ Game‑MAB ]       [ RPG‑MAB ]
                 \               /
                  \             /
     [ Engine ] ←→ [ REALMVOID CORE ] ←→ [ LAN‑MAB ]
                  /             \
                 /               \
         [ Online‑MAB ]      [ GFX‑MAB ]
```

Wichtige Punkte:

- MABs **kennen sich untereinander nicht**.  
  Sie reden **nur** mit ihrem Host (z. B. Engine) und optional mit REALMVOID Core / AXWire.
- Es gibt **keine Reihenschaltung** („RPG‑MAB hängt von Game‑MAB hängt von Net‑MAB…“),  
  sondern eine **OR‑Topologie**: jeder MAB kann allein existieren.
- AXWire fungiert als **gemeinsame Sprache** für Nachrichten – lokal (Memory‑TCP) und übers Netz.

---

## 3. Kernprinzipien

1. **KISS auf Baustein‑Ebene**
   - Jeder Baustein (MAB, Service, Protokollteil) hat einen kleinen, klaren Auftrag.
   - Komplexität entsteht durch Kombination, nicht durch aufgeblähte Einzelteile.

2. **Nachrichtenbasierte Architektur**
   - Kommunikation läuft über Messages (AXWire), nicht über direkte Funktionsaufrufe.
   - Gilt für:
     - Engine ↔ MAB,
     - MAB ↔ REALMVOID Core,
     - Client ↔ Server,
     - interne Services.

3. **Stern‑Topologie**
   - REALMVOID Core ist Hub, MABs sind Speichen.
   - MABs sind unabhängig, optional und austauschbar.

4. **Engine‑agnostisch**
   - REALMVOID kennt keine Render‑APIs.
   - Integration in Acknex, Godot, Unity, eigene Engines läuft über **Engine‑Adapter** + MAB‑APIs.

5. **Sprach‑agnostisch auf der Außenseite**
   - **Interne Referenz:** Go
   - **Offizielle Dev‑Zielsprachen:** Go, C/C++ und C#
   - Integration über:
     - Netzwerkprotokoll (AXWire),
     - und/oder C‑ABI (DLLs/Shared Libraries nach MAB‑ABI Manifest).

6. **Offline‑freundlich, Online‑fähig**
   - REALMVOID muss sinnvoll funktionieren:
     - rein lokal (Singleplayer),
     - im LAN (kleine Coop‑Szenarien),
     - im Online‑Betrieb (indie‑MMO‑Style).
   - Ideal: dieselbe API, nur andere Konfiguration („mode = single/lan/online“).

7. **Config‑first, Script‑second**
   - Grundverhalten wird über Daten/Config gesteuert.
   - Skripte erweitern, ersetzen aber nicht die Grundarchitektur.

8. **Beobachtbar, nicht magisch**
   - Basis‑Metriken und Logging sind vorgesehen (Sessions, Messages/s, Tick‑Zeiten).
   - System soll sich wie ein sauber verdrahteter Schaltschrank anfühlen, nicht wie Voodoo.

9. **Stabile Verträge & Versionierung**
   - Wire‑Protokoll, MAB‑ABI und Kern‑Konzepte werden versioniert.
   - Breaking Changes führen zu neuen Versionen statt zu stillen Inkompatibilitäten.

---

## 4. Akteure & Grenzen

### 4.1 REALMVOID Core (Go)

Der Core ist der **zentrale Dienstverbund**, typischerweise als Go‑Services/Binaries:

- **Identity & Auth**
  - CUUID, K_client, Verifikations‑Flows, Account‑Status.

- **Registry & Universe**
  - Verwaltet:
    - Clients & Sessions,
    - Realms (logische Spiele‑Universen),
    - Vertices (Instanzen/Shards),
    - laufende Dienste.

- **Vertex‑Prozesse**
  - Vertex‑Server (z. B. AGNØN‑Instanzen) mit je 100–500 CCU.
  - Führen Gameplay‑Logik aus (direkt in Go oder über angebundene MABs).

- **Netzwerk‑Frontends**
  - HIVEGATE / GATEWAY: Einstieg für Clients.
  - Weitere Spezial‑Services (Patch/Content, später).

Der Core spricht intern und extern über **AXWire**.

### 4.2 MABs (Minimal Architectural Blueprints)

MABs sind **Kits** mit jeweils einem klaren Zweck. Beispiele:

- **Game‑MAB** – generische Game‑Loop/Entity/State‑Mechanik.
- **RPG‑MAB** – Charakterwerte, Items, Skills, grundlegender Kampf/XP.
- **LAN‑MAB** – Host/Join/Discovery für LAN‑Coop.
- **Online‑MAB** – Talks to HIVEGATE/REGISTRY, Session‑/Realm‑Handling.
- **GFX‑MAB** – standardisierte Visual‑Events (Spawn, Animation, VFX/SFX, UI‑Hooks).

Alle MABs:

- reden **nur** über Nachrichten (AXWire‑Struktur, lokales Message‑API),
- halten ihre **eigene Konfiguration**,
- sind technisch unabhängig voneinander,
- nutzen ein gemeinsames **MAB‑ABI** nach dem separaten Manifest.

### 4.3 Hosts / Engines / Tools

Hosts sind alles, was MABs einbettet oder mit REALMVOID redet, z. B.:

- Spiele in:
  - Go (direkt mit MAB‑Packages),
  - C/C++ (über C‑ABI DLLs),
  - C# (über P/Invoke auf C‑ABI DLLs).
- Tools:
  - Admin‑UIs,
  - Simulations‑Clients,
  - Bots / Test‑Clients.

Der Host:

- initialisiert MAB‑Instanzen,
- tickt sie,
- schickt Messages hinein,
- holt Events/State heraus.

---

## 5. AXWire – gemeinsame Sprache

**AXWire** ist das verbindende Protokoll:

- **Header**:
  - Protokollversion,
  - Kategorie, Messagetyp,
  - Flags (z. B. encrypted, compressed, reliable),
  - Session‑ID, Sequence‑Nummer.

- **Payload**:
  - frei wählbar (z. B. MessagePack, JSON, Binärstrukturen),
  - optional: Compress‑then‑Encrypt.

AXWire wird eingesetzt:

- zwischen Client ↔ Server (UDP/TCP),
- zwischen MAB ↔ Core (z. B. wenn MABs remote sind),
- lokal als strukturiertes Message‑Schema (Memory‑TCP‑Pattern).

---

## 6. Betriebsmodi (Konzept)

REALMVOID kennt konzeptionell drei Modi.  
Implementierung kann nacheinander erfolgen – Fokus in v0.x liegt auf einem minimalen, aber echten Online‑Szenario.

1. **Single / Local**
   - MABs laufen im selben Prozess wie das Spiel.
   - Kein externer REALMVOID‑Cluster nötig.
   - AXWire wird ggf. nur lokal genutzt (Memory‑TCP).

2. **LAN / Direct**
   - Spiele sprechen direkt untereinander (Host/Join) via AXWire.
   - LAN‑MAB liefert Discovery/Session‑Basics.
   - REALMVOID‑Core ist optional.

3. **Online / Cluster**
   - Spiele verbinden sich über AXWire mit HIVEGATE.
   - REGISTRY weist Vertices/Realms zu.
   - Gameplay läuft in Vertex‑Prozessen.

**Wichtig:**  
Auf Dev‑Seite sollte die API idealerweise identisch bleiben – nur Konfiguration und Infrastruktur ändern sich.

---

## 7. Zielsprachen & Referenz‑Implementierung

- **Go**
  - Primäre Implementierungssprache für:
    - REALMVOID Core‑Services,
    - Referenz‑MABs (Game, RPG, Net etc.),
    - AXWire‑Tooling.
  - Devs können in Go direkt mit Packages arbeiten.

- **C / C++**
  - über:
    - AXWire‑Netzwerk (direkte Protokollimplementierung),
    - C‑ABI DLLs (MAB‑Implementierungen in Go, konsumiert aus C/C++).

- **C#**
  - über:
    - AXWire‑Netzwerk (Sockets),
    - P/Invoke auf C‑ABI DLLs.

Andere Sprachen können sich an AXWire + MAB‑ABI orientieren, sind aber nicht primär im Fokus.

---

## 8. Script‑Layer (Langzeit‑Option)

Der Script‑Layer ist **kein Teil des v0.x‑Scopes**, aber als Designrichtung festgehalten:

- **Ziel:**
  - kleine, eingebettete Logik‑Bausteine,
  - eventgetrieben (z. B. `OnHit`, `OnLevelUp`, `OnItemUse`),
  - optional auf Client und Server ausführbar.

- **Eigenschaften:**
  - deterministisch,
  - sandboxed,
  - vollständig über Messages/State angebunden.

- **Mögliche Technologien:**
  - Lua‑Embed (z. B. gopher‑lua),
  - oder ein minimaler, eigener DSL‑Interpreter (Conditions + Actions).

- **Pflicht:**  
  - REALMVOID und Grund‑MABs müssen **ohne** Script vollständig nutzbar sein.
  - Script ist eine Komfort‑/Power‑Option, kein Zwang.

---

## 9. Umsetzungs‑Phasen (v0.x Realität)

Dieses Manifest beschreibt das **Gesamtbild**.  
Die tatsächliche Umsetzung erfolgt bewusst schrittweise:

- **Phase 0 – AGNØN‑Vertex v0.x**
  - Ein Go‑Server (Vertex) mit:
    - einfacher AXWire‑Variante,
    - grundlegender Session/Movement‑Logik,
    - einem Test‑Client (z. B. Acknex/C#).
  - Kein Script, kein großes MAB‑Ökosystem, kein Multi‑Mode.

- **Phase 1 – Herauslösen von Kern‑MABs**
  - Identifikation und Definition von:
    - NetCore‑MAB,
    - ggf. RPG‑Core‑MAB.
  - Saubere Anwendung des MAB‑ABI Manifests.

- **Phase 2 – Engine‑Adapter & Kits**
  - Erstellung erster „Kits“ (z. B. „Single‑RPG“, „Coop‑LAN“).
  - SDKs/Beispiele für Acknex, Godot, etc.

- **Phase 3 – Script‑Layer & Luxus**
  - Falls Bedarf und Kapazität vorhanden:
    - Einbettung eines Script‑Layers,
    - weitere spezialisierte MABs und Tools.

Damit bleibt der **große Godzilla‑Plan** erhalten,  
aber die Implementation beginnt mit einem kleinen, beherrschbaren Kaiju.

---

## 10. Nicht‑Ziele

REALMVOID strebt ausdrücklich **nicht** an:

- eine komplette 3D/2D‑Engine zu sein,
- AAA‑Backend für Millionen gleichzeitige Spieler zu sein,
- einen „alles auto‑magisch“-Editor zu liefern.

Stattdessen:

- **REALMVOID** ist die **elektrische Infrastruktur**:
  - Protokoll (AXWire),
  - Core‑Services,
  - MAB‑Kits,
- auf der Indies ihre eigenen, klar definierten Welten aufbauen können.
