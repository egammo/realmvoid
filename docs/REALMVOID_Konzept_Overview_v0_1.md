# REALMVOID Konzept-Übersicht v0.1

> Kurze, nicht-technische Übersicht über REALMVOID als „Game-Universums-OS“  
> für dich selbst und andere, die in das Projekt einsteigen wollen.

---

## 1. Was ist REALMVOID?

REALMVOID ist kein Spiel und keine Game-Engine, sondern:

- ein **Backend- und Architektur-System** für Spiele,
- gedacht für **Indie-/Coop-/SSMORPG-Stil** (100–500 Spieler pro Welt, nicht 10.000),
- mit Fokus auf:
  - klare Architektur-Bausteine (MABs),
  - saubere Netzwerkgrenzen (AXWire),
  - Wiederverwendbarkeit über verschiedene Engines hinweg.

Man kann REALMVOID sich vorstellen als:

> Ein kleines Betriebssystem für Online-/LAN-Spiele  
> – ohne Grafik, aber mit Welten, Sessions, Protokollen und Bausteinen.

---

## 2. Die wichtigsten Bausteine im Kopf

### 2.1 REALMVOID Core

Der „Kern“ von REALMVOID bietet Dienste, die mehrere Spiele/Welten gemeinsam nutzen:

- **HIVEGATE** – Eingangstor für Clients  
  - nimmt Verbindungen an,  
  - macht Protokoll-Handshake,  
  - fragt das System, wohin der Spieler soll.

- **REGISTRY** – Adressbuch & Routing-Hirn  
  - weiß, welche Spiele (realm_families) es gibt,  
  - welche Welten (realms) und Instanzen (vertices) laufen,  
  - welche Session gerade in welcher Welt ist.

- **VERTEXD** – Welt-Hoster  
  - startet und stoppt konkrete Weltinstanzen (Vertices),  
  - z. B. `agnon_city#1`, `agnon_overworld#3`.

REALMVOID rendert keine Grafik, sondern:
- verwaltet nur Verbindungen, Welten, Instanzen, Sessions und Bundles.

---

### 2.2 Spiele / Universen: `realm_family`

Ein **Spieluniversum** in REALMVOID heißt `realm_family`, z. B.:

- `agnon` – dein erstes Koop-RPG-Universum,
- später vielleicht: `space_trader`, `kart_arena`, …

Jede `realm_family` hat ihre eigenen Welten, Logik und Bundles.

---

### 2.3 Welten und Instanzen: `realm_id` und `vertex_id`

Innerhalb einer `realm_family` gibt es:

- **`realm_id`** – Welt-/Map-Typ, z. B.:
  - `agnon_city` (Startstadt),
  - `agnon_overworld` (Außenwelt),
  - `agnon_dungeon_01` (Dungeon).

- **`vertex_id`** – konkrete Instanz dieser Welt, z. B.:
  - `agnon_city#1`,
  - `agnon_city#2`,
  - `agnon_overworld#1`.

Ein Vertex ist also eine „laufende Welt“ (Prozess), in der Spieler herumrennen.

---

### 2.4 AXWire – das Blut im System

**AXWire** ist das Netzwerkprotokoll von REALMVOID:

- sorgt für Nachrichten zwischen:
  - Clients ↔ HIVEGATE / Vertex-Servern,
  - optional MABs ↔ REALMVOID Core (wenn remote),
- ist binär, leichtgewichtig und auf Spielnachrichten optimiert:
  - Header (Version, Kategorie, Typ, Flags, Session, Sequence),
  - Payload (z. B. JSON, MessagePack oder Binär).

Man kann sich AXWire merken als:

> „Die Leitung, auf der alle Spielmessages fließen.“

---

### 2.5 MABs – Minimal Architectural Blueprints

**MABs** sind kleine Architektur-Bausteine:

- kein großes Framework, sondern einzelne, klar umrissene „Kits“,
- Beispiele:
  - NetCore-MAB – Sessions, Verbindungs-Handling,
  - Game-MAB – Entities, Tick, Events,
  - RPG-MAB – HP, Mana, XP, Level, Items,
  - LAN-MAB – lokale Coop ohne kompletten REALMVOID-Core,
  - Online-MAB – HIVEGATE/REGISTRY-Integration.

Sie können laufen:

- **im Server/Vertex** (als Go-Modul oder DLL),
- **im Client** (als DLL / Modul),
- und sprechen nur mit:
  - ihrem **Host** (Spiel/Engine),
  - optional REALMVOID Core – aber niemals direkt mit anderen MABs.

Wichtig: MABs sprechen über einen kleinen, standardisierten ABI (siehe MAB-ABI-Manifest).

---

### 2.6 Bundles – MAB-Gruppen pro Welt

Damit nicht jedes Mal alles von Hand verkabelt wird, gibt es **MAB-Bundles**:

- **Server-MAB-Bundle**
  - z. B. `agnon_city_server_v1`,
  - enthält MABs, die auf dem Vertex-Server laufen.

- **Client-MAB-Bundle**
  - z. B. `agnon_city_client_v1`,
  - enthält MABs für den Client (Netz, Game-View, UI-Logik, …).

Für jede (`realm_family`, `realm_id`) ist definiert:

- welche **Server-Bundle-ID** der Vertex lädt,
- welche **Client-Bundle-ID** der Client benutzen soll.

---

## 3. REALMVOID vs. AGNØN

- **REALMVOID**:
  - das OS/Backend-Universum,
  - kümmert sich um: Sessions, Realms, Vertices, Bundles, Protokolle,
  - kennt keine konkrete Spielmechanik (kein „Schaden = 20“, keine Items, etc.).

- **AGNØN**:
  - das **erste Spieluniversum**,
  - `realm_family = "agnon"`,
  - definiert:
    - welche Realms es gibt (`agnon_city`, `agnon_overworld`, …),
    - welche Bundles pro Realm laufen,
    - welche Regeln gelten (RPG-System etc.).

AGNØN ist also:

> das erste Auto, das auf der REALMVOID-Autobahn fährt.

---

## 4. Laufzeit-Flows (vereinfacht)

### 4.1 Spieler verbindet sich

1. Client → HIVEGATE:
   - „Ich bin CUUID XYZ, will `realm_family = "agnon"`.“
2. HIVEGATE → REGISTRY:
   - Session anlegen, Realm/Welt bestimmen.
3. REGISTRY → VERTEXD:
   - ggf. passenden Vertex starten (z. B. `agnon_city#1`).
4. Vertex startet:
   - lädt das zugeordnete **Server-MAB-Bundle**.
5. HIVEGATE → Client:
   - WorldRedirect: Endpoint + Tokens.
6. Client:
   - lädt **Client-MAB-Bundle** für `agnon_city`,
   - verbindet sich zum Vertex.

---

## 5. Entwicklungs-Philosophie

- **KISS auf Baustein-Ebene**:
  - kleine, klar definierte MABs,
  - Messages statt riesige Objekt-Hierarchien,
  - einfache, wohldefinierte Protokolle.

- **Stern-Topologie**:
  - MABs sprechen nicht miteinander, sondern über ihren Host/Core,
  - verhindert Framework-Hölle.

- **Zukunft mitdenken, aber Gegenwart liefern**:
  - Scripts/DSL sind vorgesehen, aber nicht Pflicht für v0.x,
  - AGNØN v0.1 ist bewusst klein, aber echt spielbar:
    - City + Overworld,
    - Bewegung, Chat, einfacher Kampf.

---

## 6. Dokumenten-Landkarte

Wenn jemand REALMVOID verstehen will, kann er so einsteigen:

1. **REALMVOID_Manifest_v0_2.md**  
   → Vision, Prinzipien, Nordstern.

2. **REALMVOID_Konzept_Overview_v0_1.md** (dieses Dokument)  
   → grobes Bild, Begriffe, Zusammenhänge.

3. **REALMVOID_Spezifikation_v0_4.md**  
   → technische Details zu Core, AXWire, MABs, Betriebsmodi.

4. **REALMVOID_Realm_Vertex_Model_v0_1.md**  
   → wie Realms/Vertices/Sessions zusammenhängen.

5. **REALMVOID_MAB_Bundles_v0_1.md**  
   → wie Bundles auf Server/Client-Seite organisiert sind.

6. **AGNØN_Manifest_v0_1.md**  
   → erstes konkretes Spieluniversum auf REALMVOID.

7. **REALMVOID_MAB_ABI_Manifest_v0_1.md**  
   → für Leute, die selbst MABs implementieren wollen.

---

## 7. Aktueller Fokus (v0.x)

Für die erste praktische Phase gilt:

- Ein AGNØN-Vertex-Server in Go,
- Ein einfacher Client (Acknex oder C#/Testclient),
- AXWire v0.1 (TCP-basiert, minimal),
- Ein oder zwei Realms (`agnon_city`, `agnon_overworld`),
- Ein kleines Set an MABs/Bundles (NetCore + Game + RPG light).

Alles andere (Scripts, komplexere Cluster, Multi-Spiele-Betrieb)  
ist bewusst als **spätere Erweiterung** geplant.

---
