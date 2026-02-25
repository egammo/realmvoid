# REALMVOID Pirelli-Prinzip v0.1

> **"Power is nothing without control"** – Pirelli (1994)  
> **"Clarity is the foundation of both"** – REALMVOID

---

## Executive Summary

REALMVOID ist mehr als eine technische Architektur – es ist ein **philosophisches System** für den Bau verteilter Spielwelten. Das Pirelli-Prinzip beschreibt die drei fundamentalen Säulen, die REALMVOID zu einem kohärenten Universum machen:

```yaml
Power:    Verschiedene Tick-Raten für verschiedene Bedürfnisse
          → Client 60 Hz, Network 20 Hz, Server variabel
          
Control:  Memory-TCP Pattern für garantierte Ordnung
          → Verlustfrei, geordnet, konsistent
          
Clarity:  Command/Query-Pattern für klare Contracts
          → Birne erwarten, Birne bekommen
```

**Die Synthese:**
```
Power + Control + Clarity = REALMVOID
```

Keine dieser Säulen funktioniert isoliert. Erst ihre **Kombination** erschafft ein System, das gleichzeitig schnell, verlässlich und verständlich ist.

---

## Die Orchester-Metapher

REALMVOID ist wie ein Orchester:

```yaml
Musiker (MABs):
  - Jeder spielt sein Instrument (RPG, Network, Combat)
  - Jeder hat sein eigenes Tempo (60 Hz, 20 Hz, 5 Hz)
  - Jeder spielt seine Stimme (Commands, Queries, Events)

Dirigent (REALMVOID Core):
  - Gibt den Takt vor (Tick-Koordination)
  - Verteilt die Partituren (Bundles)
  - Koordiniert die Einsätze (Session-Routing)

Taktstock (Memory-TCP):
  - Verbindet alle (Message Channels)
  - Garantiert Synchronität (Ordering)
  - Kein Musiker geht verloren (Verlustfrei)

Partitur (Command/Query):
  - Klare Notation (Message-Types)
  - Jeder weiß was zu tun ist (Contracts)
  - Keine Improvisation wo Präzision zählt

Publikum (Clients):
  - Hört das Gesamtwerk (60 FPS smooth)
  - Sieht nicht die Einzelteile (Abstraktion)
  - Erlebt die Harmonie (Interpolation)
```

Ein Orchester funktioniert nicht, wenn:
- Musiker beliebig schnell spielen (ohne Tick-Koordination),
- Noten durcheinander kommen (ohne Ordering),
- niemand weiß welches Stück gespielt wird (ohne klare Contracts).

REALMVOID orchestriert verteilte Spielwelten mit denselben Prinzipien.

---

## Säule 1: Power durch Tick-Raten

### Das Problem

Ein verteiltes Spiel hat fundamental unterschiedliche Geschwindigkeitsanforderungen:

```yaml
Rendering:
  - Braucht: 60+ FPS für smooth visuals
  - Toleranz: 0 (Stutter = spürbar)
  - Beispiel: Animation, Kamera, Particle Effects

Network:
  - Braucht: 20 Hz für Position-Updates
  - Toleranz: Hoch (Interpolation kaschiert)
  - Beispiel: Player Movement, Entity Sync

Game Logic:
  - Braucht: 20-30 Hz für Validierung
  - Toleranz: Mittel (Client predictiert)
  - Beispiel: Combat, Collision Detection

AI:
  - Braucht: 5-10 Hz für Decisions
  - Toleranz: Sehr hoch (NPCs nicht kritisch)
  - Beispiel: Pathfinding, Behavior Trees

Database:
  - Braucht: On-Demand
  - Toleranz: Sekunden OK
  - Beispiel: Save, Load, Persistence
```

**Anti-Pattern:** Alles mit derselben Rate ticken
- 60 Hz für alles → CPU-Verschwendung, Bandwidth explodiert
- 20 Hz für alles → Client fühlt sich sluggish
- Variable Rates ohne Koordination → Chaos

**REALMVOID-Pattern:** Hierarchische Tick-Raten

```
Client Rendering (60 Hz)
  ↓ buffered/sampled
  ├→ Local MAB Processing (60 Hz)
  ├→ Client Prediction (60 Hz)
  └→ Network Send (20 Hz throttle)
       ↓ AXWire TCP
       ↓
Server Game Logic (20 Hz)
  ├→ Physics (30 Hz wenn nötig)
  ├→ AI (5-10 Hz)
  └→ Database (async on-demand)
```

### Die Power

```go
// Client: 60 FPS Rendering
func ClientLoop() {
    renderTick := time.NewTicker(time.Second / 60)
    networkThrottle := time.NewTicker(time.Second / 20)
    
    for {
        select {
        case <-renderTick.C:
            // POWER: 60 Hz smooth visuals
            input := processInput()
            predictMovement(input, 0.016)
            interpolateEntities(0.016)
            render()
            
        case <-networkThrottle.C:
            // EFFICIENCY: Nur 20 Hz über Netzwerk
            sendBatchedCommands()
        }
    }
}

// Server: 20 Hz Game Logic
func ServerLoop() {
    gameTick := time.NewTicker(time.Second / 20)
    
    for {
        select {
        case <-gameTick.C:
            // CONTROL: 20 Hz validation ausreichend
            processCommands()
            updatePhysics(0.05)
            broadcastState()
            
        case msg := <-asyncMessages:
            // INSTANT: Kritische Messages nicht warten
            handleImmediate(msg)
        }
    }
}
```

### Die Interpolation

Zwischen 20 Hz Network-Updates (50ms) hat Client 3-4 Render-Frames (16ms):

```javascript
// Client empfängt Position nur 20x/Sekunde
let lastServerPos = {x: 0, y: 0, time: 0};
let targetServerPos = {x: 0, y: 0, time: 0};

function onNetworkUpdate(pos, timestamp) {
    lastServerPos = targetServerPos;
    targetServerPos = {x: pos.x, y: pos.y, time: timestamp};
}

// Aber rendert 60x/Sekunde
function render(currentTime) {
    // Interpolation zwischen zwei 20 Hz Updates
    const elapsed = currentTime - lastServerPos.time;
    const duration = targetServerPos.time - lastServerPos.time;
    const t = Math.min(elapsed / duration, 1.0);
    
    const displayPos = {
        x: lerp(lastServerPos.x, targetServerPos.x, t),
        y: lerp(lastServerPos.y, targetServerPos.y, t)
    };
    
    // POWER: 60 FPS smooth trotz 20 Hz Network!
    drawEntity(displayPos);
}
```

**Ergebnis:** Client hat Power (60 FPS smooth), Network ist effizient (20 Hz), Server ist skalierbar (20-30 Hz).

---

## Säule 2: Control durch Memory-TCP

### Das Problem

In verteilten Systemen mit verschiedenen Tick-Raten entstehen Race-Conditions:

```yaml
Scenario: Zwei Tasks verarbeiten Commands unterschiedlich schnell

Timeline:
  T0:   Task A startet "Command 1" (schnell)
  T5:   Task A startet "Command 2" (schnell)
  T0:   Task B startet "Command 3" (langsam)
  T10:  Command 1 fertig
  T15:  Command 2 fertig
  T50:  Command 3 fertig

Ohne Ordering:
  Empfänger sieht: Cmd1 → Cmd2 → Cmd3 ✅ (Glück gehabt)
  
Aber bei anderer Timing:
  Empfänger sieht: Cmd1 → Cmd3 → Cmd2 ❌ (Out-of-Order!)
  
  Wenn Cmd3 von Cmd2 abhängt → State inkonsistent!
```

**Anti-Pattern:** "Fire and forget"
- Messages ohne Sequence Numbers
- Keine Ordering-Garantien
- "Es wird schon passen" – bis es nicht passt

**REALMVOID-Pattern:** Memory-TCP Ordering

### Die Implementierung

```go
// Memory-TCP: TCP-Garantien ohne Network-Overhead

type OrderedChannel struct {
    incoming     chan Message
    buffer       map[uint64]Message  // Out-of-Order Buffer
    nextExpected uint64
    sendSeq      uint64
}

// Senden: Auto-Sequence
func (c *OrderedChannel) Send(msg Message) {
    msg.Sequence = atomic.AddUint64(&c.sendSeq, 1)
    c.incoming <- msg
}

// Empfangen: Ordering-Garantie
func (c *OrderedChannel) Receive() Message {
    for {
        msg := <-c.incoming
        
        if msg.Sequence == c.nextExpected {
            // Perfekt: richtige Reihenfolge
            c.nextExpected++
            return msg
            
        } else if msg.Sequence > c.nextExpected {
            // Zu früh: buffern
            c.buffer[msg.Sequence] = msg
            
            // Check: sind jetzt Folge-Messages verfügbar?
            for {
                if next, ok := c.buffer[c.nextExpected]; ok {
                    delete(c.buffer, c.nextExpected)
                    c.nextExpected++
                    return next
                }
                break
            }
        }
        // msg.Sequence < nextExpected: Duplikat, ignore
    }
}
```

### Die Control

```yaml
Garantien:
  ✅ Keine verlorenen Messages (buffered channels)
  ✅ Keine Duplikate (Sequence Check)
  ✅ Immer korrekte Reihenfolge (Ordering Buffer)
  ✅ Minimal Latency (in-memory, kein Network)

Performance:
  - Overhead: Nur uint64 Sequence + Map-Lookup
  - Latency: <1µs (vs 20-100ms Network TCP)
  - Throughput: Millionen msgs/sec möglich
```

### Anwendung im Stack

```yaml
Client-Side:
  Input Handler → Memory-TCP → MAB → Memory-TCP → Network Sender
  ↑ 60 Hz              instant           instant         20 Hz ↓
  
Network:
  Client → AXWire TCP (ordered!) → Server
  
Server-Side:
  Network Receiver → Memory-TCP → Game Logic → Memory-TCP → MABs
         20 Hz              instant      20 Hz         instant
```

**Überall wo Components asynchron laufen: Memory-TCP für Ordering.**

---

## Säule 3: Clarity durch Command/Query

### Das Problem

Ohne klare Semantik entsteht Chaos:

```yaml
Bad API (unklar):
  send("move", data)
  → Verändert das Position?
  → Gibt es ein Response?
  → Ist das ein Request oder Notification?
  → Muss ich warten?

Worse API (magisch):
  entity.update()
  → Was genau passiert?
  → Macht es Network-Call?
  → Ist es idempotent?
  → Kann es fehlschlagen?
```

**Anti-Pattern:** Gemischte Verantwortlichkeiten
- Funktionen die lesen UND schreiben
- Unclear side-effects
- "Ich weiß nicht was diese Funktion tut"

**REALMVOID-Pattern:** CQRS light

### Die Semantik

```yaml
Command (verändern):
  - Intention: State ändern
  - Side-Effects: Ja, das ist der Zweck
  - Idempotent: Nein (normalerweise)
  - Return: Success/Failure (Result)
  - Analog: HTTP POST

Query (lesen):
  - Intention: Information holen
  - Side-Effects: Niemals!
  - Idempotent: Ja, immer
  - Return: Data
  - Analog: HTTP GET

Event (benachrichtigen):
  - Intention: "X ist passiert"
  - Side-Effects: Nein (ist Vergangenheit)
  - Idempotent: Ja (kann mehrmals ankommen)
  - Return: Nichts (one-way)
  - Analog: Pub/Sub Message
```

### Die Implementation

```go
// Command: Verändert State
type Command struct {
    Type     string      // "move", "attack", "use_item"
    Payload  interface{}
    Sequence uint64
}

// Query: Liest State
type Query struct {
    Type     string      // "get_state", "list_entities"
    Payload  interface{}
    ReplyTo  chan Response  // Wo soll Antwort hin?
}

// Event: Notification
type Event struct {
    Type      string     // "entity_died", "loot_dropped"
    Payload   interface{}
    Timestamp time.Time
}

// Handler unterscheiden klar
func (v *Vertex) Handle(msg Message) {
    switch msg := msg.(type) {
    case Command:
        // Verändere State, gib Result zurück
        result := v.handleCommand(msg)
        sendResponse(result)
        
    case Query:
        // Lese State, keine Änderungen!
        data := v.handleQuery(msg)
        msg.ReplyTo <- Response{Data: data}
        
    case Event:
        // Reagiere auf Vergangenheit
        v.handleEvent(msg)
        // Kein Response!
    }
}
```

### Das "Birne"-Prinzip

```yaml
Regel: "Erwarte ich eine Birne, bekomme ich eine Birne"

Gut ✅:
  Command "CreateVertex":
    Request:  { realm_family: "agnon", realm_id: "city" }
    Response: { vertex_id: "agnon_city#1", endpoint: "..." }
    → Birne erwartet, Birne bekommen
  
  Query "GetVertexStatus":
    Request:  { vertex_id: "agnon_city#1" }
    Response: { status: "running", ccu: 120 }
    → Birne erwartet, Birne bekommen

Schlecht ❌:
  Command "DoSomething":
    Request:  { stuff: "whatever" }
    Response: Sometimes X, sometimes Y, sometimes nothing
    → Apfel? Birne? Garnichts? Wer weiß!
```

### Die Clarity

```go
// Client weiß genau was er tut
func (c *Client) MovePlayer(dx, dy float32) {
    // Command: Ich will State ändern
    cmd := Command{
        Type: "move",
        Payload: MovePayload{
            Velocity: Vector2{dx, dy},
        },
    }
    
    // Predict lokal (Power!)
    c.predictPosition(dx, dy)
    
    // Send to server
    c.sendCommand(cmd)
    
    // Erwarte: Server validiert, sendet State-Update zurück
    // Klar: Ich weiß was passiert
}

func (c *Client) QueryEntityInfo(entityID uint64) EntityInfo {
    // Query: Ich will nur lesen
    query := Query{
        Type: "get_entity",
        Payload: entityID,
        ReplyTo: make(chan Response),
    }
    
    c.sendQuery(query)
    resp := <-query.ReplyTo
    
    // Erwarte: EntityInfo, keine Side-Effects
    // Klar: State hat sich nicht verändert
    return resp.Data.(EntityInfo)
}
```

---

## Die Synthese: Wie die Säulen zusammenspielen

### Szenario: Player bewegt sich

```yaml
1. Input (Client 60 Hz):
   ┌─────────────────────────────────────┐
   │ User drückt 'W'                     │
   │ ProcessInput() → Movement Command   │
   │ Sequence: 1001                      │
   └─────────┬───────────────────────────┘
             │ Memory-TCP (Clarity: Command)
             ↓
2. Local Processing (Client 60 Hz):
   ┌─────────────────────────────────────┐
   │ MAB empfängt Command 1001 (ordered!)│
   │ Predict: pos += velocity * dt       │
   │ Event: "predicted_pos" zurück       │
   └─────────┬───────────────────────────┘
             │ Memory-TCP (Power: instant)
             ↓
3. Rendering (Client 60 Hz):
   ┌─────────────────────────────────────┐
   │ Render predicted position           │
   │ Smooth 60 FPS                       │
   └─────────┬───────────────────────────┘
             │
4. Network Send (20 Hz throttle):
   ┌─────────────────────────────────────┐
   │ Batch Commands [1000-1005]          │
   │ AXWire TCP → Server                 │
   └─────────┬───────────────────────────┘
             │ AXWire (Control: TCP ordered)
             ↓
5. Server Receive (20 Hz):
   ┌─────────────────────────────────────┐
   │ Commands [1000-1005] (in order!)    │
   │ Memory-TCP → Game Logic             │
   └─────────┬───────────────────────────┘
             │ Memory-TCP (Control: ordered)
             ↓
6. Server Validation (20 Hz):
   ┌─────────────────────────────────────┐
   │ Process Command 1001:               │
   │ - Check speed (no speedhack)        │
   │ - Check collision (no wall-clip)    │
   │ - Apply if valid                    │
   └─────────┬───────────────────────────┘
             │ Memory-TCP (Clarity: Result)
             ↓
7. Server Broadcast (20 Hz):
   ┌─────────────────────────────────────┐
   │ State Update: { pos: (10, 5) }      │
   │ AXWire TCP → All Clients            │
   └─────────┬───────────────────────────┘
             │ AXWire (Control: TCP ordered)
             ↓
8. Client Reconciliation (async):
   ┌─────────────────────────────────────┐
   │ Server pos vs Predicted pos:        │
   │ Diff < 0.01? → OK, ignore           │
   │ Diff > 0.01? → Correction           │
   └─────────────────────────────────────┘

Power:   Client 60 Hz smooth, Server nur 20 Hz
Control: Alle Messages ordered, keine Lost
Clarity: Command clear, Validation clear, Result clear
```

### Szenario: Realm Transfer (Weltwechsel)

```yaml
1. Trigger (In Vertex A):
   ┌─────────────────────────────────────┐
   │ Player betritt Zone-Portal          │
   │ Game Logic: "Transfer needed"       │
   └─────────┬───────────────────────────┘
             │ Memory-TCP (Clarity: Command)
             ↓
2. Vertex A → REGISTRY:
   ┌─────────────────────────────────────┐
   │ Command: "TransferSession"          │
   │ { session_id, target_realm_id }     │
   └─────────┬───────────────────────────┘
             │ AXWire TCP (Control: ordered)
             ↓
3. REGISTRY Processing:
   ┌─────────────────────────────────────┐
   │ Query: "FindVertex" (target_realm)  │
   │ → vertex_id = "agnon_overworld#1"   │
   │                                     │
   │ Command: "UpdateSession"            │
   │ → session now at Vertex B           │
   └─────────┬───────────────────────────┘
             │ Memory-TCP (Control: ordered tasks)
             ↓
4. REGISTRY → Vertex A:
   ┌─────────────────────────────────────┐
   │ Response: { target_endpoint }       │
   └─────────┬───────────────────────────┘
             │ AXWire TCP
             ↓
5. Vertex A → Client:
   ┌─────────────────────────────────────┐
   │ Event: "ZoneTransfer"               │
   │ { endpoint, token }                 │
   └─────────┬───────────────────────────┘
             │ AXWire TCP
             ↓
6. Client Reconnect:
   ┌─────────────────────────────────────┐
   │ Disconnect from Vertex A            │
   │ Connect to Vertex B                 │
   │ Handshake mit Token                 │
   └─────────┬───────────────────────────┘
             │
7. Continue Playing:
   ┌─────────────────────────────────────┐
   │ Client in new Vertex                │
   │ Back to 60 Hz gameplay              │
   └─────────────────────────────────────┘

Power:   Async transfer, kein Blocking
Control: Session-State konsistent über Transfer
Clarity: Jeder Schritt hat klaren Contract
```

---

## Die Stern-Topologie als Orchester-Prinzip

```
          [RPG-MAB]        [Combat-MAB]
               \               /
                \             /
        [Engine] → [CORE] ← [Vertex]
                /             \
               /               \
          [Net-MAB]        [LAN-MAB]
```

### Warum Stern statt Chain?

```yaml
Chain (Anti-Pattern):
  RPG → Combat → Network → Core
  
  Problem:
    - RPG hängt von Combat ab
    - Combat hängt von Network ab
    - Änderung in Network bricht RPG
    - Dependency Hell

Stern (REALMVOID):
  RPG → Core
  Combat → Core  
  Network → Core
  
  Vorteil:
    - MABs kennen sich nicht
    - Jeder spricht nur mit Core
    - Austauschbar
    - Testbar isoliert
```

### Memory-TCP im Stern

```go
// Core koordiniert, MABs kommunizieren über Core

type Core struct {
    mabs map[string]*OrderedChannel
}

// RPG-MAB sendet Event
func (rpg *RPGMab) OnLevelUp(entity Entity) {
    event := Event{
        Type: "level_up",
        Payload: LevelUpData{entity.ID, newLevel},
    }
    
    // Geht nur an Core, nicht an andere MABs!
    rpg.toCore.Send(event)
}

// Core verteilt
func (c *Core) DistributeEvent(evt Event) {
    // Alle interessierten MABs bekommen es (ordered!)
    if evt.Type == "level_up" {
        c.mabs["combat"].Send(evt)  // Combat updated Stats
        c.mabs["ui"].Send(evt)      // UI zeigt Notification
        // RPG-MAB bekommt es NICHT zurück
    }
}
```

**Ergebnis:** Loose Coupling + Guaranteed Delivery + Clear Contracts

---

## Design-Patterns und Anti-Patterns

### Pattern: Async Command, Sync Query (wenn nötig)

```go
// Command: Fire and continue
func (c *Client) Attack(targetID uint64) {
    cmd := Command{Type: "attack", Payload: targetID}
    c.sendCommand(cmd)
    // Kein Warten! Server sendet später "damage_dealt" Event
}

// Query: Warte auf Antwort (wenn wirklich nötig)
func (c *Client) GetInventory() []Item {
    query := Query{
        Type: "get_inventory",
        ReplyTo: make(chan Response),
    }
    c.sendQuery(query)
    
    // Warte (aber nur für Queries!)
    resp := <-query.ReplyTo
    return resp.Data.([]Item)
}
```

### Pattern: Client Prediction + Server Authority

```go
// Client: Predict (Power)
func (c *Client) UseSkill(skillID int) {
    // Sofort lokal ausführen (60 FPS smooth)
    c.playAnimation("cast")
    c.startCooldown(skillID)
    
    // Command an Server
    cmd := Command{Type: "use_skill", Payload: skillID}
    c.sendCommand(cmd)
    
    // Erwarte: Server validiert und sendet Result
}

// Server: Validate (Control)
func (s *Server) HandleUseSkill(cmd Command) {
    skill := cmd.Payload.(int)
    
    // Validierung (Control!)
    if !s.hasMana(skill) {
        return Event{Type: "skill_failed", Reason: "no_mana"}
    }
    if s.isOnCooldown(skill) {
        return Event{Type: "skill_failed", Reason: "cooldown"}
    }
    
    // Apply
    s.applyCost(skill)
    damage := s.calculateDamage(skill)
    
    return Event{Type: "skill_used", Damage: damage}
}
```

### Anti-Pattern: Blocking auf Game-Thread

```go
// ❌ SCHLECHT: Block game loop
func BadQuery() {
    result := database.Query("SELECT * FROM ...") // BLOCKS!
    render(result)
}

// ✅ GUT: Async mit Callback
func GoodQuery() {
    go func() {
        result := database.Query("SELECT * FROM ...")
        resultChannel.Send(result)  // Memory-TCP!
    }()
}

func GameLoop() {
    select {
    case result := <-resultChannel:
        render(result)
    default:
        renderPlaceholder()  // Zeig Loading...
    }
}
```

### Anti-Pattern: Tight Coupling zwischen MABs

```go
// ❌ SCHLECHT: RPG-MAB ruft Combat-MAB direkt
func (rpg *RPGMab) OnLevelUp() {
    combatMab.UpdateStats(newStats)  // Direct Call!
}

// ✅ GUT: Über Core mit Memory-TCP
func (rpg *RPGMab) OnLevelUp() {
    event := Event{Type: "level_up", Payload: newStats}
    rpg.toCore.Send(event)  // Core distribuiert!
}
```

---

## Die Checkliste für neue Features

Bei jedem neuen Feature diese Fragen stellen:

### 1. Clarity: Welche Semantik?

```yaml
Fragen:
  - Ist es Command, Query oder Event?
  - Was ist Input? Was ist Output?
  - Was ist die "Birne" die ich erwarte?
  - Gibt es Side-Effects?

Wenn unklar:
  → Design überarbeiten
  → Vielleicht in mehrere Messages splitten
```

### 2. Control: Wo ist Ordering wichtig?

```yaml
Fragen:
  - Können Messages Out-of-Order ankommen?
  - Ist Reihenfolge kritisch für Konsistenz?
  - Brauche ich Reliability (keine Verluste)?
  - Ist es Time-Critical?

Wenn ja:
  → Memory-TCP für interne Kommunikation
  → AXWire Reliable Channel für Network
```

### 3. Power: Welche Tick-Rate?

```yaml
Fragen:
  - Braucht es 60 Hz (UX-critical)?
  - Reicht 20 Hz (most gameplay)?
  - Kann es 5 Hz sein (background tasks)?
  - Ist es async/on-demand?

Dann:
  → Client: 60 Hz wo UX zählt
  → Network: 20 Hz Standard
  → Server: 20-30 Hz Validation
  → Background: 5 Hz oder async
```

### 4. Der Pirelli-Test

```yaml
Power ohne Control:
  Symptom: Client fühlt sich smooth, aber Cheating möglich
  Fix: Server-Validation hinzufügen

Control ohne Power:
  Symptom: Alles korrekt, aber fühlt sich sluggish an
  Fix: Client-Prediction hinzufügen

Power + Control ohne Clarity:
  Symptom: Funktioniert, aber niemand versteht es
  Fix: Messages umbenennen, Contracts dokumentieren
```

---

## Praktische Beispiele

### Beispiel 1: Chat-System

```yaml
Anforderung: Global Chat mit History

Analyse:
  - Clarity: Chat = Command (send) + Event (receive)
  - Control: Reihenfolge wichtig (sonst Chaos)
  - Power: Nicht zeitkritisch (5 Hz OK)

Design:
  Command "SendChat":
    Client → Server (AXWire Reliable)
    { message, timestamp, seq }
    
  Server Processing (async, nicht auf Tick warten):
    - Validierung (Rate-Limit, Profanity Filter)
    - Store in Redis (History)
    - Broadcast als Event
    
  Event "ChatMessage":
    Server → All Clients (ordered!)
    { sender, message, timestamp }
    
  Client Receiving:
    Memory-TCP → UI-Thread
    Append to Chat-Log (ordered!)

Ergebnis:
  ✅ Messages kommen in Order an (Control)
  ✅ Kein Block auf Game-Thread (Power)
  ✅ Klare Semantik (Clarity)
```

### Beispiel 2: Loot-Drop

```yaml
Anforderung: Monster stirbt, loot spawned

Analyse:
  - Clarity: Monster-Death = Event, Loot-Spawn = Command
  - Control: Server-Authority (prevent duping)
  - Power: Nicht zeitkritisch

Design:
  Event "MonsterDied":
    Server Combat-MAB → Server Core
    { monster_id, killer_id, position }
    Memory-TCP (instant, ordered)
    
  Server Core distributes:
    → Server RPG-MAB: Calculate Loot
    → Server World-MAB: Spawn Loot Entity
    Memory-TCP zu jedem (ordered!)
    
  Command "SpawnLoot":
    Server World-MAB → Clients
    { loot_id, position, items[] }
    AXWire Reliable (ordered)
    
  Client receiving:
    Memory-TCP → Game-MAB
    Spawn Visual Entity (60 FPS)
    Play Animation

Ergebnis:
  ✅ Server entscheidet Loot (Control)
  ✅ Client zeigt smooth (Power)
  ✅ Klare Event-Chain (Clarity)
```

### Beispiel 3: Skill-Cooldown

```yaml
Anforderung: Skill mit 10s Cooldown

Analyse:
  - Clarity: UseSkill = Command, Server validiert
  - Control: Server-Authority (prevent cooldown hacks)
  - Power: Client muss sofort reagieren (UX)

Design:
  Client UseSkill:
    1. Check local cooldown (instant feedback)
       → Wenn auf CD: Play "not ready" sound
       → Return early
       
    2. Predict:
       → Start animation (60 FPS smooth)
       → Start local cooldown timer
       
    3. Send Command (20 Hz batched):
       { skill_id, target_id, timestamp }
       
  Server Validation:
    1. Check server-side cooldown
    2. Check mana/resources
    3. If OK:
       → Apply skill
       → Start server-side CD
       → Broadcast Event "skill_used"
    4. If NOT OK:
       → Send Event "skill_failed"
       
  Client Reconciliation:
    Event "skill_used":
      → Update cooldown (server-authoritative)
      → Play VFX/SFX
      
    Event "skill_failed":
      → Reset local cooldown (was prediction)
      → Play error feedback

Ergebnis:
  ✅ Instant feedback (Power)
  ✅ Server prevents hacks (Control)
  ✅ Clear failure handling (Clarity)
```

---

## Warum das für Indies perfekt ist

### AAA-Studios

```yaml
Resourcen:
  - 100+ Entwickler
  - Millionen Budget
  - Dedicated Anti-Cheat Team
  - Custom Engine

Können sich leisten:
  - Vollständige Server-Authority
  - Alles validiert
  - 60 Hz Server (für Shooter)
  - Komplexe Lag-Compensation
```

### Indies (REALMVOID-Zielgruppe)

```yaml
Resourcen:
  - 1-5 Entwickler
  - Begrenztes Budget
  - Kein Security-Team
  - Off-the-shelf Tools

Brauchen:
  - Smart Tradeoffs
  - Klare Patterns
  - Maintainable Code
  - Focus auf Critical Paths
```

### REALMVOID gibt Indies:

```yaml
Power:
  - 60 FPS Client smooth gameplay
  - 20 Hz Network (4x weniger als 60 Hz)
  - Skaliert auf 500 CCU pro Vertex
  → AAA-Feeling ohne AAA-Infrastructure

Control:
  - Memory-TCP: Konsistenz garantiert
  - Server-Authority: Wo es zählt
  - Tolerance Bands: Wo es egal ist
  → Keine Duping-Exploits, aber pragmatisch

Clarity:
  - Command/Query: Jeder versteht Code
  - Clear Contracts: Onboarding leicht
  - Dokumentierbar: Maintainable
  → Ein Dev-Team kann es handhaben
```

---

## Zusammenfassung: Das Orchester

```
┌─────────────────────────────────────────────────────────┐
│                    REALMVOID UNIVERSUM                  │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Dirigent (Core):                                       │
│    - Koordiniert Tick-Raten                             │
│    - Verteilt Messages                                  │
│    - Gibt Takt vor                                      │
│                                                         │
│  Taktstock (Memory-TCP):                                │
│    - Verbindet alle Components                          │
│    - Garantiert Ordering                                │
│    - Verlustfrei                                        │
│                                                         │
│  Partitur (Command/Query):                              │
│    - Klare Notation                                     │
│    - Jeder weiß seine Rolle                             │
│    - Keine Improvisation wo Präzision zählt             │
│                                                         │
│  Musiker (MABs):                                        │
│    - RPG-MAB: 20 Hz (steady rhythm)                     │
│    - Combat-MAB: 30 Hz (precise timing)                 │
│    - Net-MAB: 20 Hz (efficient)                         │
│    - Renderer: 60 Hz (smooth performance)               │
│                                                         │
│  Publikum (Clients):                                    │
│    - Hört harmonisches Gesamtwerk                       │
│    - 60 FPS smooth experience                           │
│    - Sieht nicht die Komplexität dahinter               │
│                                                         │
└─────────────────────────────────────────────────────────┘

Power + Control + Clarity = Harmonie
```

### Die drei Säulen im Orchester

```yaml
Power (Verschiedene Tempi):
  - Jeder Musiker spielt in seinem Tempo
  - Schnelle Passages (60 Hz Rendering)
  - Langsame Passages (5 Hz AI)
  - Alles synchronisiert zum Takt

Control (Präzision):
  - Kein Musiker spielt zu früh
  - Keiner wird ausgelassen
  - Alle spielen zusammen, nicht durcheinander
  - Der Dirigent sorgt für Ordnung

Clarity (Notation):
  - Jede Note hat klare Bedeutung
  - Jeder weiß wann sein Einsatz ist
  - Keine Missverständnisse
  - Reproduzierbar
```

---

## Appendix: Quick Reference

### Memory-TCP Verwendung

```go
// Erstellen
channel := NewOrderedChannel(bufferSize)

// Senden (auto-sequence)
channel.Send(Message{Type: "cmd", Data: x})

// Empfangen (ordered!)
msg := channel.Receive()
```

### Command/Query Pattern

```go
// Command
type Command struct {
    Type    string
    Payload interface{}
    Seq     uint64
}

// Query
type Query struct {
    Type    string
    Payload interface{}
    ReplyTo chan Response
}

// Event
type Event struct {
    Type      string
    Payload   interface{}
    Timestamp time.Time
}
```

### Tick-Raten Guidelines

```yaml
60 Hz:
  - Client Rendering
  - Input Processing
  - Client Prediction
  - Animation

20-30 Hz:
  - Network Updates
  - Server Game Logic
  - Physics (wenn nötig)
  - Combat Resolution

5-10 Hz:
  - AI Decisions
  - Pathfinding
  - Background Tasks

On-Demand:
  - Database Ops
  - File I/O
  - Web API Calls
```

### Das REALMVOID-Pirelli-Prinzip als technische Volkssprache

Das REALMVOID-Pirelli-Prinzip beschreibt, wie das Universum auf der technischen Ebene funktioniert:
- **Power**: unterschiedliche Tick-Raten für unterschiedliche Bedürfnisse (Rendering, Network, Logic),
- **Control**: Memory-TCP als Taktstock für Ordnung und Zuverlässigkeit,
- **Clarity**: Commands, Queries und Events als klare „Wortarten“ mit sauberen Contracts.

Zusammen mit der Idee von REALMVOID als „Volkssprache der VORTEXe“ entsteht ein vollständiges Bild:
- Die **Volkssprache** definiert, *was* gesagt werden kann (Semantik, Vokabular, Rollen).
- Das **Pirelli-Prinzip** definiert, *wie* es gesagt und koordiniert wird (Tempo, Ordnung, Klarheit).

So wird REALMVOID zu einem Orchester aus MABs und VORTEXen, die alle dieselbe Sprache sprechen – nur in unterschiedlichen Tempi, aber mit demselben Taktstock und derselben Partitur.

---

**Version:** v0.1  
**Datum:** 2024-11-22  
**Status:** Philosophisches Kerndokument

> "In einem Orchester spielt jeder Musiker seine eigene Stimme in seinem eigenen Tempo,  
> aber alle folgen demselben Taktstock und derselben Partitur.  
> REALMVOID ist dieses Orchester für verteilte Spielwelten."

**Power + Control + Clarity = REALMVOID**
