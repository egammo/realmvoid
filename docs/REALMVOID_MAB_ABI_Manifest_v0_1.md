# REALMVOID MAB‑ABI Manifest v0.1

> Dieses Dokument definiert den **Minimalen Architekturellen Blueprint‑ABI**  
> („MAB‑ABI“) von REALMVOID – also die **Schnittstelle**, über die MABs aus  
> verschiedenen Sprachen und Engines angesteuert werden können.

Ziel:  
Jede Person, die dieses Dokument liest, soll in **Go, C/C++ oder C#** (oder jeder anderen Sprache) eine Host‑Umgebung bauen können, die MABs konsistent anspricht – auch ohne die Go‑Referenzimplementierung zu kennen.

---

## 1. Rollen & Begriffe

- **Host**  
  Eine Laufzeitumgebung, die MABs lädt und benutzt.  
  Beispiele:
  - ein Spiel in C++ oder C#,
  - ein Go‑Service,
  - ein Test‑/Admin‑Tool.

- **MAB (Minimal Architectural Blueprint)**  
  Ein eigenständiges Modul mit:
  - klarem Zweck (z. B. RPG‑Regeln, LAN‑Sessions),
  - eigener interner Logik,
  - eigener Konfiguration,
  - definierter Message‑API nach außen.

- **MAB‑Instanz**  
  Eine konkrete, zur Laufzeit erzeugte Instanz eines MABs,  
  mit eigenem Zustand (z. B. eine konkrete RPG‑Welt, eine Session, eine Lobby).

- **Message**  
  Atomare Einheit der Kommunikation zwischen Host und MAB:
  - Richtung Host → MAB: „Commands / Inputs“
  - Richtung MAB → Host: „Events / Outputs / State‑Updates“

- **Tick / Frame**  
  Ein Zeit‑Schritt, in dem ein MAB intern seinen Zustand aktualisiert.

---

## 2. Designziele des MAB‑ABI

1. **Simpel & stabil**
   - Wenige Funktionen, klare Semantik.
   - Lieber etwas generischer als zu viele Spezialfunktionen.

2. **Stern‑Topologie**
   - Host ↔ MAB, nicht MAB ↔ MAB.
   - MABs kommunizieren nicht direkt miteinander.

3. **Nachrichtenorientiert**
   - Der ABI kennt nur generische Messages, keine Engine‑Typen oder Spielklassen.

4. **C‑ABI als Kleinster Gemeinsamer Nenner**
   - Der formale Vertrag wird als C‑ABI gedacht.
   - Andere Sprachen nutzen Wrapper (z. B. P/Invoke in C#, FFI in anderen Sprachen).

5. **Mehrere Instanzen**
   - Ein Host kann mehrere MAB‑Instanzen parallel betreiben (z. B. mehrere Partien, Lobbys, Welten).

---

## 3. Lebenszyklus einer MAB‑Instanz

Ein MAB durchläuft immer denselben grundlegenden Lebenszyklus:

1. **Load / Bind**
   - Host lädt die Library / bindet das MAB (z. B. DLL, Shared Library oder Go‑Package).

2. **Create / Init**
   - Host erstellt eine neue MAB‑Instanz, optional mit Konfiguration.

3. **Tick / Messages**
   - In einer Schleife:
     - Host gibt Zeit‑Delta (`dt`) vor,
     - Host liefert Messages an den MAB,
     - Host holt Messages/Events vom MAB ab.

4. **Shutdown / Destroy**
   - Instanz wird kontrolliert heruntergefahren und freigegeben.

---

## 4. Konzeptionelle ABI‑Operationen

Der MAB‑ABI definiert konzeptionell fünf grundlegende Operationen.  
Die genaue Signatur kann je nach Sprache variieren, die **Bedeutung** bleibt gleich.

### 4.1 `CreateInstance(config) → handle`

- Eingabe:
  - optionale Konfigurationsdaten (z. B. JSON, Pfad zu einer Config‑Datei, oder `null`).
- Ausgabe:
  - ein undurchsichtiger **Handle** (z. B. Integer oder Pointer), der diese Instanz identifiziert.
- Verhalten:
  - Allokiert internen Zustand für eine neue MAB‑Instanz.
  - Initialisiert Standardwerte und lädt ggf. Ressourcen/Config.

### 4.2 `DestroyInstance(handle)`

- Eingabe:
  - gültiger Instanz‑Handle.
- Verhalten:
  - Gibt alle zugehörigen Ressourcen frei.
  - Nach dem Aufruf darf der Handle nicht mehr benutzt werden.

### 4.3 `Tick(handle, dt_seconds)`

- Eingabe:
  - Instanz‑Handle,
  - Zeitdelta in Sekunden (als Float/Double).
- Verhalten:
  - Aktualisiert internen Zustand basierend auf `dt`.
  - Führt geplante Tasks, Timouts, Physik‑ oder Logik‑Updates aus.
- Hinweis:
  - Der Host entscheidet, wie oft und mit welchem `dt` er `Tick` aufruft (z. B. 60 FPS, 20 Ticks/Sek. usw.).

### 4.4 `SendMessage(handle, kind, payload)`

- Eingabe:
  - Instanz‑Handle,
  - `kind`: ein integer oder string, der die Message‑Art kennzeichnet,
  - `payload`: roher Datenblock (Byte‑Array) oder String.
- Verhalten:
  - Übergibt eine Host→MAB‑Nachricht an die Instanz.
  - Die Interpretation von `kind` und `payload` ist Teil der jeweiligen MAB‑Spezifikation (z. B. das RPG‑MAB‑Dokument).

### 4.5 `PollMessage(handle) → (hasMessage, kind, payload)`

- Eingabe:
  - Instanz‑Handle.
- Ausgabe:
  - Entweder „keine Nachricht“,
  - oder eine einzelne MAB→Host‑Message (Kind + Payload).
- Verhalten:
  - Gibt die nächste wartende Nachricht vom MAB an den Host zurück.
  - Host ruft diese Funktion üblicherweise in einer Schleife auf, bis keine Nachrichten mehr vorliegen.

Alternative Implementierungsformen:

- Statt Pull (Poll), kann auch ein Callback‑Mechanismus eingesetzt werden.  
  Für den **normativen ABI** wird jedoch das **Poll‑Modell** angenommen, da es in C‑ABI und über Sprachgrenzen hinweg einfacher handhabbar ist.

---

## 5. Nachrichten‑Konzept

Der MAB‑ABI schreibt nicht vor, wie das `payload` intern kodiert ist, diese Varianten sind zulässig:

- **Textbasiert**:
  - JSON, INI, einfache Key=Value‑Strings etc.
- **Binär**:
  - MessagePack, Protobuf, selbstdefinierte Binärformate.

Vorgaben:

- Host und MAB müssen sich über Format und Version der Messages klar sein  
  (z. B. durch ein MAB‑spezifisches Dokument wie „RPG‑MAB Spec“).
- AXWire‑Header/Felder können als äußere Hülle verwendet werden, sind aber im MAB‑ABI optional.

Empfehlung:

- Für frühe Implementierungen: JSON oder MessagePack zur leichteren Debugbarkeit.
- Später kann für Performance auf binäre Formate gewechselt werden.

---

## 6. Fehler‑ und Status‑Behandlung

Generelle Regeln:

- ABI‑Funktionen, die scheitern können, sollen dies klar signalisieren,
  z. B. über:
  - Rückgabe‑Codes (0 = OK, !=0 = Fehler),
  - oder spezielle Status‑Nachrichten (z. B. `kind = "error"`).

- Fehlerzustände sollten nie „stillschweigend verschluckt“ werden.

Empfehlungen:

- Jede MAB‑Spezifikation (z. B. „Online‑MAB“, „RPG‑MAB“) definiert:
  - ihre eigenen Fehler‑`kind`‑Werte und ‑payloads,
  - wie der Host damit umgehen soll.

---

## 7. Nebenläufigkeit & Threading

Der MAB‑ABI trifft die folgende **Grundannahme**:

- Alle ABI‑Aufrufe auf eine konkrete Instanz (`handle`) erfolgen **sequentiell**:
  - kein `Tick` und `SendMessage` gleichzeitig auf demselben Handle,
  - Host serialisiert die Aufrufe.

- MAB‑Implementierungen **dürfen** intern Nebenläufigkeit verwenden (z. B. Worker‑Threads, Goroutines),
  müssen aber:
  - sicherstellen, dass die ABI‑Funktionen selbst *thread‑safe aufrufenbar* sind,  
    oder
  - klar dokumentieren, dass der Host sie nur von einem Thread aus benutzt.

Empfehlung für erste Implementierungen:

- Pro Instanz: alle ABI‑Aufrufe aus einem Thread heraus (z. B. Game‑Loop‑Thread).

---

## 8. Mehrsprachige Nutzung

Der MAB‑ABI sieht den **C‑ABI** als normative Basis.  
Typische Nutzung:

- **C / C++**
  - Link gegen MAB‑Library (DLL/so/dylib),
  - ruft die fünf Kernfunktionen direkt auf.

- **C#**
  - benutzt `DllImport` / P/Invoke auf den C‑ABI,
  - optional: dünne OO‑Wrapper‑Klassen.

- **Go**
  - kann:
    - MABs direkt als Go‑Packages verwenden,
    - oder über FFI/Plugins/Kommando‑Zeilen‑Prozesse kommunizieren.

- **Andere Sprachen**
  - können über ihre jeweiligen FFI‑Mechanismen an den C‑ABI andocken.

Wichtig:

- Die konkrete **Funktionssignatur** (z. B. wie `payload` repräsentiert wird)  
  wird in einem ergänzenden technischen Dokument festgelegt,  
  bleibt aber auf die oben beschriebenen fünf Operationen beschränkt.

---

## 9. Stern‑Topologie & Unabhängigkeit der MABs

Aus Sicht des ABI:

- Jeder MAB ist ein **eigenständiges Modul** mit eigenem ABI‑Namespace.
- MABs reden **nie direkt** miteinander; Kommunikation läuft ausschließlich über:
  - Host ↔ MAB,
  - optional MAB ↔ REALMVOID Core (über AXWire, nicht über ABI‑Funktionen anderer MABs).

Konsequenzen:

- Fällt ein MAB aus oder wird nicht genutzt, betrifft das andere MABs nicht.
- Ein Host kann beliebige Kombinationen von MABs laden:
  - nur RPG,
  - RPG + LAN,
  - nur Online‑MAB,
  - etc.

---

## 10. Versionierung des MAB‑ABI

- Das hier definierte Modell ist **MAB‑ABI v0.1**.
- Änderungen, die:
  - neue Funktionen hinzufügen, ohne alte zu brechen,
  - oder nur Semantik präzisieren,
  können als v0.x weitergeführt werden.

- Änderungen, die:
  - eine der fünf Kernoperationen grundsätzlich ändern,
  - oder deren Bedeutung umdefinieren,
  führen zu **MAB‑ABI v1.x**.

Jeder MAB soll angeben:

- gegen welche ABI‑Version er entwickelt wurde,
- welche zusätzlichen Constraints (z. B. bevorzugte Messageformate) gelten.

---

## 11. Zusammenfassung

Der MAB‑ABI definiert ein **kleines, starkes Interface**:

- `CreateInstance`
- `DestroyInstance`
- `Tick`
- `SendMessage`
- `PollMessage`

Alles Weitere – Inhalt der Messages, konkrete Konfiguration, Fehlercodes –  
ist Aufgabe der jeweiligen MAB‑Spezifikationen (z. B. „RPG‑MAB Manifest“, „LAN‑MAB Spec“).

Dadurch bleibt REALMVOID:

- **modular** (Stern‑Topologie),
- **sprache‑ und engine‑agnostisch** (C‑ABI + AXWire),
- und **KISS auf Modul‑Ebene**, auch wenn das Gesamtsystem groß wird.
