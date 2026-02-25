# REALMVOID Realm & Vertex Model v0.1

> Dieses Dokument beschreibt, wie REALMVOID Spiele, Welten und Instanzen logisch modelliert:  
> `realm_family`, `realm_id` und `vertex_id` – plus das Zusammenspiel mit HIVEGATE, REGISTRY und VERTEXD.

---

## 1. Begriffe & Ziele

REALMVOID soll mehrere **unterschiedliche Spiele** (Universen) gleichzeitig hosten können.  
Dafür werden drei Ebenen unterschieden:

- **realm_family** – welches Spiel / welches Universum?
- **realm_id** – welche Welt / Map innerhalb dieses Spiels?
- **vertex_id** – welche konkrete Instanz dieser Welt?

Ziel:  
Jeder, der diese drei Begriffe kennt, kann verstehen, wie:

- ein Client beim richtigen Spiel landet,
- REALMVOID die passende Welt-Instanz auswählt oder startet,
- ein Vertex-Server weiß, welche Logik er laden muss.

---

## 2. realm_family – welches Spiel?

**realm_family** identifiziert das *Spiel* oder die *Spiel-Familie*.

Beispiele:

- `agnon`
- `space_trader`
- `kart_arena`

Eigenschaften:

- Wird beim **Login/Handshake** vom Client an HIVEGATE übergeben:
  - „Ich will AGNØN spielen, nicht irgendein beliebiges Spiel.“
- Steht in der REGISTRY-DB:
  - als Attribut bei Realms und Vertices,
  - als Attribut bei Sessions („diese Session gehört zu AGNØN“).

Regel:

- Jede Session gehört genau **einer** `realm_family`.
- Jede `realm_id` gehört genau **einer** `realm_family`.
- Jeder `vertex_id` gehört genau **einer** `realm_family`.

---

## 3. realm_id – welche Welt / Map innerhalb des Spiels?

**realm_id** beschreibt einen logischen Welt-/Map-Typ innerhalb einer Familie.

Beispiele (alle gehören zur Familie `agnon`):

- `agnon_city` – Startstadt
- `agnon_overworld` – Oberwelt
- `agnon_dungeon_01` – erster Dungeon

Eigenschaften:

- `realm_id` ist ein **logischer Typ**, keine einzelne Instanz.
- Der Client kann (muss aber nicht) beim Connect bereits einen `realm_id` wünschen:
  - Wenn leer: das Spiel/REALMVOID wählt einen **Default-Startrealm** (z. B. `agnon_city`).
  - Wenn gesetzt: wird geprüft, ob der Spieler dort sein darf (z. B. Zugangsvoraussetzungen).

Regel in der REGISTRY:

- `realm_id` ist **immer** zusammen mit `realm_family` definiert.
- Kombination (`realm_family`, `realm_id`) ist eindeutig.

---

## 4. vertex_id – welche konkrete Instanz?

**vertex_id** steht für eine **konkrete laufende Weltinstanz** (Prozess).

Beispiele:

- `agnon_city#1`
- `agnon_city#2`
- `agnon_overworld#1`

Eigenschaften:

- Eine `realm_id` kann viele `vertex_id`s haben:
  - z. B. viele Stadt-Instanzen für unterschiedliche Spielergruppen.
- Jeder `vertex_id`:
  - läuft auf einem bestimmten VERTEXD-Host,
  - hat einen Netzwerk-Endpunkt (z. B. `tcp://world01.egammo.local:5001`),
  - hat Kapazitätsinfos (max CCU).

Regel:

- `vertex_id` ist **immer** eindeutig.
- Zu jedem `vertex_id` gehört genau ein (`realm_family`, `realm_id`)-Paar.

---

## 5. Rollen von HIVEGATE, REGISTRY und VERTEXD

### 5.1 HIVEGATE – Eingangstor

Aufgaben:

- Nimmt initiale Verbindungen vom Client an.
- Führt den Handshake durch:
  - Protokollversion,
  - Annahme/Bekanntheit von `realm_family`,
  - Authentifizierung (optional).
- Erfragt bei REGISTRY:
  - `session_id` für den Client,
  - wohin (welcher Vertex) der Client geschickt werden soll.
- Schickt dem Client bei Weltwechsel:
  - „WorldRedirect“ (Endpoint, Tokens, Ziel-Vertex).

Wichtige Inputs vom Client an HIVEGATE:

- Pflicht: `realm_family`
- Optional (je nach Use-Case):
  - `realm_id` (wenn der Client gezielt in eine bestimmte Welt will),
  - sonst wird Spiel-Default genutzt.

### 5.2 REGISTRY – Verzeichnis & Routing-Hirn

Aufgaben:

- Verwalten von:
  - `realm_families`,
  - `realm_ids`,
  - `vertex_ids`,
  - Clients/Sessions.
- Buchhalten:
  - welche Vertices laufen wo,
  - welcher Client/Session an welchem Vertex hängt.

Typische Aufgaben:

- **Session-Anlage**:
  - `cuuid` → neue `session_id` + Zuordnung zu `realm_family`.
- **Vertex-Auswahl**:
  - „Für `realm_family=agnon`, `realm_id=agnon_city`:
    - gibt es einen passenden Vertex mit Platz?
    - wenn nein: VERTEXD bitten, einen zu starten.“
- **Weltwechsel (Realm-Transfer)**:
  - Session von `vertex_id A` auf `vertex_id B` umhängen.
  - HIVEGATE/Vertex über Ziel-Vertex informieren.

### 5.3 VERTEXD & Vertex-Prozesse

VERTEXD:

- Läuft auf einem Host.
- Kann mehrere Vertex-Prozesse starten/stoppen.
- Bekommt von REGISTRY Aufträge:
  - „Starte Vertex für (`realm_family`, `realm_id`).“

Vertex-Prozess:

- Lädt beim Start:
  - das passende **Server-MAB-Bundle** für diese (`realm_family`, `realm_id`).
- Öffnet einen Netzwerk-Endpunkt.
- Meldet REGISTRY:
  - „Ich bin `vertex_id = XYZ`, endpoint = ..., capacity = ..., realm_family = ..., realm_id = ...“
- Betreut laufende Sessions/Spieler.

---

## 6. Flow-Beispiele

### 6.1 Erster Login (Start in Standardwelt)

Annahme:

- Spiel: `realm_family = "agnon"`
- Standardstart: `realm_id = "agnon_city"`

Schritte:

1. Client → HIVEGATE:
   - „Hallo, ich will `realm_family = "agnon"` spielen.“
2. HIVEGATE → REGISTRY:
   - „Neuer Client mit CUUID, gib mir `session_id` für `agnon`.“
3. REGISTRY:
   - legt `session_id = S1234` an,
   - notiert `realm_family = "agnon"`.
4. Client → HIVEGATE:
   - „EnterWorld“ ohne explizites `realm_id` → Default: `agnon_city`.
5. HIVEGATE → REGISTRY:
   - „Für `agnon`/`agnon_city`: Vertex gesucht.“
6. REGISTRY → VERTEXD (falls keiner läuft):
   - „Starte Vertex für (`agnon`, `agnon_city`).“
7. VERTEXD:
   - startet Prozess `vertex(agnon_city#1)`,
   - meldet Endpoint + Kapazität.
8. REGISTRY:
   - trägt `vertex_id = "agnon_city#1"` ein,
   - hängt `session_id S1234` an diesen Vertex.
9. HIVEGATE → Client:
   - „WorldRedirect zu `endpoint = world01:5001`, mit `session_id S1234` + Token.“
10. Client:
    - verbindet sich mit `agnon_city#1` direkt.

---

### 6.2 Weltwechsel `agnon_city` → `agnon_overworld`

1. Im Vertex `agnon_city#1`:
   - interne Logik erkennt: „Spieler will in `agnon_overworld` wechseln.“
2. Vertex → REGISTRY:
   - „Für `realm_family=agnon`, `realm_id=agnon_overworld`:  
      gib mir Ziel-Vertex (existierend oder neu).“
3. REGISTRY:
   - identifiziert/erzeugt `vertex_id = "agnon_overworld#1"`.
   - hängt `session_id S1234` von `agnon_city#1` auf `agnon_overworld#1` um.
4. Vertex `agnon_city#1` → Client:
   - „ZoneTransfer → endpoint von `agnon_overworld#1` + Session/Token.“
5. Client:
   - baut Verbindung zu `agnon_overworld#1` auf,
   - setzt Spiel fort.

---

## 7. Symmetrie zu MAB-Bundles (Ausblick)

Dieses Dokument ist OS-zentriert, aber es bereitet bereits eine wichtige Info vor:

- Ein Vertex kennt (`realm_family`, `realm_id`) →  
  **damit weiß er, welches Server-MAB-Bundle zu laden ist.**
- Der Client kennt ebenfalls (`realm_family`, wahrscheinlich `realm_id`) →  
  **damit weiß er, welches Client-MAB-Bundle zu laden ist.**

Damit werden später:

- Server-MAB-Bundles (für Logik, Zonen, Netzwerk),
- Client-MAB-Bundles (für Logik, UI, Prediction)

klar einem (`realm_family`, `realm_id`)-Kontext zugeordnet.

Das Gegenstück dazu wird in `REALMVOID_MAB_Bundles_v0_1` beschrieben.
