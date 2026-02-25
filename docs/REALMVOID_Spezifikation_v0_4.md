# REALMVOID_Spezifikation_v0_4

> Technische Spezifikation (Arbeitsstand) von REALMVOID nach Manifest v0.2  
> und MAB-ABI v0.1.  
> Fokus dieser Version: Klarheit über Komponenten, Topologie und Schnittstellen,  
> ohne in Implementierungsdetails einzelner Spiele abzutauchen.

---

## 1. Überblick

REALMVOID ist ein **Backend- und Architektur-System** für indieskalige Spiele:

- Kernziele:
  - Wiederverwendbare Architektur‑Bausteine (MABs),
  - stabile Message‑Protokolle (AXWire),
  - klare Trennung zwischen Engine/Präsentation und Logik/Backend.

- Grenzen:
  - Kein Rendering,
  - kein Editor,
  - keine AAA‑Massenskalierung.

---

## 2. Komponenten-Überblick

### 2.1 REALMVOID Core

**Core‑Services** (konzeptionell):

- **HIVEGATE**
  - Externer Einstiegspunkt für Clients.
  - Funktion:
    - Erstkontakt,
    - Protokoll‑Handshake,
    - Authentifizierung (Basic/Token‑basiert).

- **REGISTRY**
  - „Adressbuch“ des Universums:
    - verwaltet Clients/Sessions,
    - kennt Realms und Vertices,
    - weiß, welche Vertex‑Instanz welche Welt bedient.

- **VERTEXD (Vertex‑Daemon)**
  - Hostet eine oder mehrere Vertex‑Instanzen (Spielwelten).
  - Jede Vertex‑Instanz:
    - betreut typischerweise 100–500 CCU,
    - führt Gameplay‑Logik aus (direkt in Go oder über MABs).

Weitere mögliche Services (später):

- PATCH/CONTENT‑Server,
- METRICS‑Collector.

### 2.2 MABs (Minimal Architectural Blueprints)

MABs existieren sowohl:

- als **Go‑Packages** (für Core/Server/Go‑Hosts),
- als **DLLs/Shared Libraries mit C‑ABI** (für C/C++/C#‑Hosts).

Sie kapseln wiederverwendbare Architektur‑Muster.

Beispiele:

- **NetCore-MAB**
  - Session‑Handling,
  - Basis‑Routing,
  - ggf. einfache Replikationsmuster.

- **Game-MAB**
  - Entity‑Verwaltung,
  - Events,
  - Tick‑Modell.

- **RPG-MAB**
  - Charakter‑Stats,
  - Items/Inventar,
  - Skills/XP/Level.

- **LAN-MAB**
  - Discovery,
  - Host/Join,
  - Peer‑Verwaltung ohne zentralen Core.

- **Online-MAB**
  - Anbindung an HIVEGATE/REGISTRY,
  - Realm‑/Vertex‑Auswahl,
  - Session‑Lifecycle in einem REALMVOID‑Cluster.

### 2.3 Hosts / Engines

Hosts sind:

- konkrete Spiele (Client/Server),
- Tools (Admin, Simulation),
- Test‑Programme.

Rollen:

- Sie halten die Game‑Loop (Render‑/Update‑Loop),
- sie binden MABs ein (per Go‑Package oder C‑ABI),
- sie sprechen ggf. über AXWire mit REALMVOID‑Core‑Services.

---

## 3. Topologie

### 3.1 Stern-Topologie (konzeptionell)

Zentrales Prinzip:

- REALMVOID Core in der Mitte,
- MABs und Hosts sternförmig daran angeschlossen,
- keine direkten MAB↔MAB‑Abhängigkeiten.

```text
[ Engine/Host ] ←→ [ MAB ] ←→ (optional) [ REALMVOID Core ]

[ anderer Host ] ←→ [ anderer MAB ] ←→ (optional) [ REALMVOID Core ]
```

Varianten:

- **Pure Local**: Host ↔ MAB (ohne Core).
- **LAN‑Spiel**: Host ↔ MAB (LAN‑MAB), die MABs sprechen ggf. peer‑to‑peer über AXWire.
- **Online**: Host ↔ MAB ↔ REALMVOID Core (HIVEGATE/REGISTRY/VERTEXD) über AXWire.

Damit bleiben die Bausteine entkoppelt, während das System insgesamt strukturiert skaliert.

---

## 4. AXWire – Protokollrahmen

AXWire beschreibt:

- die äußere Hülle für Nachrichten („auf der Leitung“),
- eine empfohlene interne Message‑Struktur („im Code“).

### 4.1 Header (Minimum v0.1)

Ein AXWire‑Header enthält mindestens:

- `version` – Protokollversion (z. B. 0x0001)
- `category` – grobe Kategorie (z. B. System, Gameplay, Chat)
- `msg_type` – konkreter Messagetyp innerhalb der Kategorie
- `flags` – Bitfeld (z. B. encrypted, compressed, reliable)
- `session_id` – Zuordnung zu einer Session/Verbindung
- `sequence` – Sequenznummer (für Ordering/Reliability)

Implementierungsdetails (Byte‑Reihenfolge, Größen) werden in einer separaten AXWire‑Spec präzisiert.

### 4.2 Payload

Die Nutzlast kann sein:

- JSON,
- MessagePack,
- Binärformat.

Vereinbarung:

- Je MAB / je Service wird genau festgelegt:
  - welche `category`+`msg_type`‑Kombinationen existieren,
  - wie das Payload‑Format aussieht,
  - welche Semantik die Nachricht hat.

### 4.3 Transport

AXWire kann über verschiedene Transports getragen werden:

- UDP,
- TCP,
- ggf. WebSocket (später).

Für v0.x:

- Fokus auf **TCP** (einfachere Debugbarkeit) und/oder UDP mit sehr dünner Reliability‑Schicht.

---

## 5. MAB-ABI (Kurzfassung)

Der MAB‑ABI ist in `REALMVOID_MAB_ABI_Manifest_v0_1.md` ausführlich beschrieben.  
Diese Spezifikation fasst nur die Kernpunkte zusammen.

Ein Host interagiert mit einem MAB über genau fünf konzeptionelle Operationen:

1. `CreateInstance(config) → handle`
2. `DestroyInstance(handle)`
3. `Tick(handle, dt_seconds)`
4. `SendMessage(handle, kind, payload)`
5. `PollMessage(handle) → (hasMessage, kind, payload)`

Wichtige Eigenschaften:

- Alle Interaktion läuft über Messages (Commands/Events).
- Pro Instanz wird die ABI sequentiell benutzt (Host serialisiert Aufrufe).
- Der ABI selbst kennt keine Engine‑Typen und keine konkreten Game‑Klassen.

MAB‑spezifische Dokumente (z. B. „RPG-MAB Spec“) definieren:

- zulässige `kind`‑Werte,
- Payload‑Schemas,
- Fehlernachrichten etc.

---

## 6. Datenmodelle (konzeptionell)

Einige Schlüssel‑IDs und Konzepte im REALMVOID‑Universum:

- **CUUID** – Client‑ID (z. B. Gerät/Installation)
- **Session-ID** – Laufende Sitzung eines Clients
- **Realm** – logisches Spiel‑Universum (z. B. „AGNØN“)
- **Vertex** – konkrete Instanz eines Realms (Shard/Welt)
- **VUUID** – eindeutige Vertex‑ID
- **Entity-ID** – eindeutige ID für In‑Game‑Entitäten innerhalb einer Welt

Diese IDs werden:

- in AXWire‑Nachrichten mitgeführt (wo sinnvoll),
- in REGISTRY/CORE‑Diensten verwaltet.

Eine tiefere, formale Beschreibung folgt in einer eigenen „Datenmodell‑Spec“.

---

## 7. Betriebsmodi & Scope v0.x

### 7.1 Konzeptionelle Modi

Siehe Manifest v0.2:

- Single / Local
- LAN / Direct
- Online / Cluster

### 7.2 v0.x Fokus

Um nicht zu überfrachten, fokussiert sich v0.x auf:

- **einen Online‑ähnlichen Modus**:
  - ein Go‑Vertex‑Server für AGNØN,
  - ein einfacher Client (Acknex/C#/Go),
  - AXWire‑Lite v0.1.

Technische Einschränkungen in v0.x:

- Wahrscheinlich nur **ein Realm / wenige Vertices**.
- Persistenz ggf. minimal (Dateien/In‑Memory).
- MAB‑Ökosystem reduziert auf:
  - NetCore‑Logik,
  - einfache Game/RPG‑Logik.

Spätere Versionen können LAN‑Only‑Szenarien und vollwertige Cluster nachziehen.

---

## 8. Sicherheits- und Robustheitsaspekte (nur angerissen)

REALMVOID ist kein Security‑Framework, aber:

- AXWire sollte frühzeitig:
  - Integritäts‑Checks (z. B. Checksums, MAC) unterstützen,
  - klare Trennung zwischen „trusted“ (Server/Backend) und „untrusted“ (Client) haben.

- MABs sollten:
  - defensiv mit inkompatiblen/fehlerhaften Messages umgehen,
  - Fehler klar signalisieren, statt panisch zu crashen.

Weitere Details gehören in eine eigene „Security & Hardening“-Schrift.

---

## 9. Weiteres Vorgehen

Für die nächste Spezifikations‑Iteration (v0_5+) sind geplant:

- eine dedizierte **AXWire v0.1‑Spec** (Headerlayout, Endianness, Beispielmessages),
- eine **NetCore-MAB‑Spec** (Session/Connect/Disconnect‑Flow),
- eine erste **AGNØN‑v0.x‑Featureliste** als Referenz‑Implementierung im REALMVOID‑Universum.

Dieses Dokument (`v0_4`) dient als aktuelle, technischere Gesamtübersicht,  
auf die sich spätere Detail‑Specs beziehen.
