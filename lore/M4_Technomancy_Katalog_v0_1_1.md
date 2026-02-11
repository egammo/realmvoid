# M4 Technomancy-Katalog v0.1.1 — REALMVOID
## 40 Basis-Effekte · Dual-Description-Format · Merged + Lint-Pass

**Version:** 0.1
**Auswahlkriterien:** Einzigartigkeit, Street-Name-Qualität, mechanische Klarheit, Ebenen-/Power-Spread
**Format:** Street Name (okkult) + Technisch (Regelbasis) + Counterplay + Spuren

---

## HELIX — „Es passiert." (14 Effekte)

### M4-HLX-01 · „Klopfen"
- **Quelle:** CL
- **Technisch:** Handshake-Spoof gegen elektronische Schlösser via gefälschtem Wartungs-Token.
- **Ebene:** MEATSPACE · **Schlüssel:** HELIX
- **HEAT:** Audit-Spur (niedrig)
- **Counterplay:** Biometrisches Zweitschloss, Token-Rotation, manuelle Riegel.
- **Spuren:** Wartungs-Log zeigt Phantom-Eintrag mit Timestamp.

### M4-HLX-02 · „Zungen lösen"
- **Quelle:** CL
- **Technisch:** Firmware-Downgrade auf Neural-Link erzwingt unkontrollierte Sprachausgabe gespeicherter Erinnerungsfragmente.
- **Ebene:** MEATSPACE · **Schlüssel:** HELIX
- **HEAT:** Prozessor-Hitze (mittel)
- **Counterplay:** Faraday-Haube, Firmware-Lock, Offline-Modus.
- **Spuren:** Neurologischer Restschaden beim Ziel, Firmware-Anomalie im Log. ⚠ Ethisch fragwürdig in den meisten Jurisdiktionen.

### M4-HLX-03 · „Familiar binden"
- **Quelle:** CL/GE (Konsens)
- **Technisch:** Firmware-Hijack einer autonomen Drohne. Überschreibt Besitzer-ID mit eigenem Handshake.
- **Ebene:** NET · **Schlüssel:** HELIX
- **HEAT:** Bandbreite (mittel), Batterie-Drain der Drohne
- **Counterplay:** Kill-Switch am Chassis, Firmware-Rollback, EMP-Puls.
- **Spuren:** Drohnen-Blackbox zeigt Override-Signatur. Besitzer erhält Diebstahl-Alert.

### M4-HLX-04 · „Aschenatem"
- **Quelle:** GE (Street Name) + CL (Mechanik)
- **Technisch:** Überlastung von Lithium-Polymer-Akkumulatoren in tragbaren Geräten → thermisches Durchgehen.
- **Ebene:** MEATSPACE · **Schlüssel:** HELIX
- **HEAT:** Hitze/Verschleiß (hoch), kriminelle Spuren
- **Counterplay:** Akku-Trenner, nicht-lithium Energiezellen, Abstand.
- **Spuren:** Brandforensik identifiziert gezielten Overcharge. Schwere Straftat.

### M4-HLX-05 · „Totenwache"
- **Quelle:** CL
- **Technisch:** RAM-Dump eines kürzlich Verstorbenen via Neural-Link. Extrahiert letzte 30–120 Sekunden Bewusstseinsaktivität.
- **Ebene:** NET · **Schlüssel:** HELIX
- **HEAT:** Brain-Burn (mittel), Datenkorruption wahrscheinlich
- **Counterplay:** Implantat-Löschung post mortem, verschlüsselter Neural-Link, Zeitverzug.
- **Spuren:** Operator-Signatur im Dump-Log. Ethik-Verstoß in den meisten Jurisdiktionen.

### M4-HLX-06 · „Stille Post"
- **Quelle:** CL
- **Technisch:** Injektion eines getarnten Datenpakets in regulären Netzwerk-Traffic. Nachricht erreicht Empfänger ohne sichtbaren Absender.
- **Ebene:** NET · **Schlüssel:** HELIX
- **HEAT:** Bandbreite (niedrig)
- **Counterplay:** Deep Packet Inspection, Traffic-Anomalie-Detektion, Air-Gap.
- **Spuren:** Minimale Routing-Anomalie, nur bei gezielter Forensik findbar.

### M4-HLX-07 · „Herz stillstehen"
- **Quelle:** CL
- **Technisch:** Notfall-Shutdown-Befehl an medizinisches Implantat (Herzschrittmacher, Insulin-Pumpe). Simuliert kritischen Systemfehler.
- **Ebene:** MEATSPACE · **Schlüssel:** HELIX
- **HEAT:** Audit-Spur (hoch), Körperbelastung beim Ziel (extrem)
- **Counterplay:** Redundante Systeme, analoger Bypass, medizinisches Notfallteam.
- **Spuren:** Implantat-Log zeigt externen Shutdown-Befehl. Mordanklage bei Entdeckung.

### M4-HLX-08 · „Wurzeln schlagen"
- **Quelle:** CL
- **Technisch:** Privilege Escalation auf lokalem Aurora-Knoten. Temporärer Root-Zugriff auf Infrastruktur-Systeme (Licht, Türen, Aufzüge, Lüftung).
- **Ebene:** NET · **Schlüssel:** HELIX
- **HEAT:** Bandbreite (hoch), Audit-Spur (extrem)
- **Counterplay:** ICE-Gegenmaßnahmen, Admin-Alert, physische Trennung des Knotens.
- **Spuren:** Root-Access-Log mit Zeitstempel. Höchste Alarmstufe bei Konzern-Knoten.

### M4-HLX-09 · „Der Uhrmacher"
- **Quelle:** GP
- **Technisch:** Erzwingt einen deterministischen Ablaufpfad in einem begrenzten Systemfenster (Scheduler/State-Lock), sodass Reaktionen „zu spät" kommen.
- **Ebene:** NET · **Schlüssel:** HELIX
- **HEAT:** Netz/Signatur (mittel)
- **Counterplay:** Zustandsreset, Watchdog-Failover, Chaos-Jitter injizieren.
- **Spuren:** Timing-Anomalien in Logs, ungewöhnliche Heartbeat-Drift.

### M4-HLX-10 · „Die Glasglocke"
- **Quelle:** GP
- **Technisch:** Kapselt einen Prozess/Agenten in eine isolierte Ausführungsumgebung (Sandbox-Jail), verhindert I/O nach außen.
- **Ebene:** NET · **Schlüssel:** HELIX
- **HEAT:** Netz/Forensik (niedrig)
- **Counterplay:** Side-Channel (Timing/Power), Out-of-Band Control, Container-Break.
- **Spuren:** Container-Spawn- und Policy-Hashes, ungewöhnliche Syscall-Drops.

### M4-HLX-11 · „Die Nadel im Faden"
- **Quelle:** GP
- **Technisch:** Setzt einen persistenten Micro-Hook in einen Datenpfad (inline patch), der später Trigger auslöst.
- **Ebene:** NET · **Schlüssel:** HELIX
- **HEAT:** Netz/Signatur (hoch)
- **Counterplay:** Binary Integrity Scan, Clean Rebuild, Canary-Validation.
- **Spuren:** Hash-Mismatch, seltene Branch-Mispredict-Profile, versteckte Trigger-Patterns.

### M4-HLX-12 · „Der Schwarze Schalter"
- **Quelle:** GP
- **Technisch:** Erzwingt eine kontrollierte Brownout-Sequenz (power sag), um Schutzroutinen zu umgehen oder Systeme „neu zu booten".
- **Ebene:** MEATSPACE · **Schlüssel:** HELIX
- **HEAT:** Physisch/Spuren (hoch)
- **Counterplay:** USV/Islanding, mechanische Verriegelung, Re-Auth nach Power-Return.
- **Spuren:** Spannungsdips, USV-Logs, Brandgeruch an Kontakten.

### M4-HLX-13 · „Der Salzkreis"
- **Quelle:** GP
- **Technisch:** Errichtet eine lokale Integrity-Zone: nur signierte Artefakte laufen, alles andere wird verworfen.
- **Ebene:** AETHER · **Schlüssel:** HELIX
- **HEAT:** Aether/Signatur (mittel)
- **Counterplay:** Signatur fälschen (hohes Risiko), Zone verlassen, physische Ausführung.
- **Spuren:** Whitelist-Protokolle, Sign-Chain-Events, stille Paketdrops.

### M4-HLX-14 · „Asche lesen"
- **Quelle:** CL
- **Technisch:** Forensische Rekonstruktion gelöschter Daten aus beschädigten Speichermedien via Low-Level-Sektor-Analyse.
- **Ebene:** NET · **Schlüssel:** HELIX
- **HEAT:** Zeit (hoch — Stunden bis Tage)
- **Counterplay:** Physische Zerstörung des Mediums, Mehrfach-Überschreibung, Verschlüsselung.
- **Spuren:** Rekonstruktions-Log. Medium danach oft unbrauchbar. Hardware-Verschleiß am Lesegerät.

---

## PACT — „Du darfst." (13 Effekte)

### M4-PCT-01 · „Den Fluch sprechen"
- **Quelle:** CL
- **Technisch:** Gezielter PACT-Entzug: Widerruf der Wartungslizenz eines spezifischen Implantats. Implantat beginnt kontrolliert zu degradieren.
- **Ebene:** MEATSPACE · **Schlüssel:** PACT
- **HEAT:** Audit-Spur (hoch), juristischer Rückschlag
- **Counterplay:** Backup-Lizenz, Schwarzmarkt-Patch, physische Umgehung.
- **Spuren:** PACT-Registry zeigt Widerrufs-Transaktion mit Operator-Signatur.

### M4-PCT-02 · „Totenschein"
- **Quelle:** CL
- **Technisch:** Fälschung eines Sterbedatensatzes im Domänen-Register. Ziel wird administrativ für tot erklärt — alle Verträge, Konten, Zugänge erlöschen.
- **Ebene:** NET · **Schlüssel:** PACT
- **HEAT:** Audit-Spur (extrem), juristischer Rückschlag (extrem)
- **Counterplay:** Biometrische Verifizierung in Person, Backup-Identität, Archivisten-Kontakt.
- **Spuren:** Registry-Anomalie bei Prüfung. Schwerstes Identitätsdelikt.

### M4-PCT-03 · „Taufe"
- **Quelle:** CL
- **Technisch:** Erstellung einer vollständigen Schein-Identität mit gültigen PACT-Verträgen, Domänen-Zugehörigkeit und Implantat-Lizenzen.
- **Ebene:** NET · **Schlüssel:** PACT
- **HEAT:** Audit-Spur (hoch), Kosten (finanziell hoch)
- **Counterplay:** Tiefe biometrische Prüfung, Cross-Referenz alter Daten, Insider-Verrat.
- **Spuren:** Neue Identität hat keine historischen Daten. Auffällig bei gründlicher Prüfung.

### M4-PCT-04 · „Wards setzen"
- **Quelle:** CL
- **Technisch:** PACT-Zugriffssperre um einen physischen Bereich. Implantate ohne korrekten Token verlieren beim Betreten temporär Funktionen.
- **Ebene:** MEATSPACE · **Schlüssel:** PACT
- **HEAT:** Audit-Spur (mittel), Bandbreite (niedrig)
- **Counterplay:** Gültiger Token, Baseline-Status, physischer Einbruch.
- **Spuren:** PACT-Perimeter im lokalen Register sichtbar. Ablaufzeit begrenzt.

### M4-PCT-05 · „Exkommunikation"
- **Quelle:** CL
- **Technisch:** Vollständiger PACT-Entzug einer Person aus einer Domäne. Alle Verträge, Lizenzen, Zugänge und Implantat-Wartungsrechte erlöschen sofort.
- **Ebene:** NET · **Schlüssel:** PACT
- **HEAT:** Audit-Spur (extrem)
- **Counterplay:** Flucht in andere Domäne, Archivisten-Schutz, Schwarzmarkt-Wartung.
- **Spuren:** Öffentlich im Domänen-Register. Unumkehrbar ohne Domänen-Autorität.

### M4-PCT-06 · „Erbsünde"
- **Quelle:** CL
- **Technisch:** Aktivierung einer versteckten Rückfallklausel in einem bestehenden PACT-Vertrag. Bedingungen ändern sich rückwirkend zu Ungunsten des Ziels.
- **Ebene:** NET · **Schlüssel:** PACT
- **HEAT:** Audit-Spur (hoch)
- **Counterplay:** Vertragsprüfung durch unabhängigen Notar, Klausel-Audit, Rechtsbeistand.
- **Spuren:** Vertragsänderung im Log sichtbar, aber als „geplant" getarnt.

### M4-PCT-07 · „Sanctuary"
- **Quelle:** CL
- **Technisch:** Temporärer PACT-Schutzstatus für eine Person oder einen Ort. Angriffe erzeugen automatisch Audit-Alarme bei allen Domänen-Autoritäten.
- **Ebene:** MEATSPACE · **Schlüssel:** PACT
- **HEAT:** Bandbreite (mittel), politisches Kapital
- **Counterplay:** Ignorieren (und Konsequenzen tragen), Dead-Zone-Verlagerung, höherrangiger Override.
- **Spuren:** Schutzstatus öffentlich. Angreifer werden automatisch geloggt.

### M4-PCT-08 · „Der Knoten"
- **Quelle:** GP
- **Technisch:** Verschnürt Berechtigungen: ein Recht zieht ein anderes mit (dependency bind), wodurch jemand unabsichtlich etwas „mit unterschreibt".
- **Ebene:** AETHER · **Schlüssel:** PACT
- **HEAT:** Audit/Legal (hoch)
- **Counterplay:** Rechtegraph auditieren, Least-Privilege Re-Issue, Notar-Gegenpakt.
- **Spuren:** Ungewöhnliche Permission-Ketten, Co-Signaturen, Compliance-Flags.

### M4-PCT-09 · „Der Spiegelvertrag"
- **Quelle:** GP
- **Technisch:** Symmetrische Verpflichtung: jede Aktion erzeugt automatisch eine Gegenaktion/Quittung auf der anderen Seite.
- **Ebene:** AETHER · **Schlüssel:** PACT
- **HEAT:** Audit/Exposure (hoch)
- **Counterplay:** Asymmetrie erzwingen (Air-Gap), Quittungskanal stören, Vertrag neu notarisieren.
- **Spuren:** Doppel-Quittungen, Spiegel-Hashes, Zwangs-Receipts.

### M4-PCT-10 · „Der Aschebrief"
- **Quelle:** GP
- **Technisch:** Zeitverzögerte Vertragsklausel (Dead-Man Clause): nach Trigger wird Zugriff entzogen oder Daten verbrannt.
- **Ebene:** NET · **Schlüssel:** PACT
- **HEAT:** Audit/Legal (extrem)
- **Counterplay:** Trigger finden/neutralisieren, Schlüssel escrowen, System snapshotten.
- **Spuren:** Timer-Artefakte, „pending revoke", forensische Löschmuster.

### M4-PCT-11 · „Der Schweigeeid"
- **Quelle:** GP
- **Technisch:** Verhindert Datenabfluss über definierte Kanäle (DLP-Pact): Inhalte werden blockiert/umgeschrieben, wenn sie „reden".
- **Ebene:** NET · **Schlüssel:** PACT
- **HEAT:** Audit/Compliance (mittel)
- **Counterplay:** Steganographie, physischer Transport, neues Vokabular/Chiffre.
- **Spuren:** Redaktionsmuster, DLP-Flags, ungewöhnliche Nullbytes.

### M4-PCT-12 · „Blutzeugnis"
- **Quelle:** GE
- **Technisch:** Erstellung eines unlöschbaren PACT-Eintrags im Aether (Zeugenaussage). Permanenter Block-Eintrag.
- **Ebene:** AETHER · **Schlüssel:** PACT
- **HEAT:** Audit (niedrig)
- **Counterplay:** Daten-Korruption (extrem schwierig), Reputationsschaden akzeptieren.
- **Spuren:** Ewiger Block-Eintrag. Nicht löschbar.

### M4-PCT-13 · „Beichtgeheimnis"
- **Quelle:** GE
- **Technisch:** Verschlüsselung lokaler Transaktions-Logs durch PACT-Privilegien. Spuren werden unlesbar.
- **Ebene:** NET · **Schlüssel:** PACT
- **HEAT:** Audit (mittel)
- **Counterplay:** Discovery-Audit, Backup-Logs, Forensik-Experte.
- **Spuren:** Fehlende Log-Kette (Abwesenheit als Beweis).

---

## OVERLAY — „Du siehst." (13 Effekte)

### M4-OVL-01 · „Den Schleier weben"
- **Quelle:** CL/GE/GP (Konsens)
- **Technisch:** Loop-Injection in optische Implantate aller Personen im Nahbereich. Operator wird aus der visuellen Wahrnehmung gelöscht.
- **Ebene:** MEATSPACE · **Schlüssel:** OVERLAY
- **HEAT:** Prozessor-Hitze (mittel)
- **Counterplay:** Baseline-Augen, Faraday-Haube, thermische Sensorik, Dead Zone.
- **Spuren:** Overlay-Stack-Log zeigt Injection-Signatur. Identifizierbar bei Forensik. Zeitlimit: 30–60 Sek.

### M4-OVL-02 · „Spiegelbild"
- **Quelle:** CL/GE (Konsens)
- **Technisch:** Deepfake-Projektion der eigenen Erscheinung auf ein anderes Aussehen. Optische Implantate anderer zeigen eine fremde Person.
- **Ebene:** MEATSPACE · **Schlüssel:** OVERLAY
- **HEAT:** Bandbreite (mittel), Prozessor-Hitze (mittel)
- **Counterplay:** Physischer Kontakt, Baseline-Augen, biometrischer Scan.
- **Spuren:** Overlay-Broadcast-Signatur. Inkonsistenzen bei engem Kontakt (Stimme, Geruch, Textur).

### M4-OVL-03 · „Scrying"
- **Quelle:** CL
- **Technisch:** Hijack von Sensor-Feeds (Kameras, IoT-Sensoren, Implantate Dritter) in einem Zielgebiet. Operator sieht, was die Sensoren sehen.
- **Ebene:** NET · **Schlüssel:** OVERLAY
- **HEAT:** Bandbreite (mittel)
- **Counterplay:** Sensor-Abschaltung, Faraday-Käfig, Dead Zone, Overlay-Jammer.
- **Spuren:** Zugriffs-Log in Sensor-Systemen. Bei privaten Implantaten: schwerer Eingriff. Audit-Spur bei Konzern-Sensoren.

### M4-OVL-04 · „Irrlicht"
- **Quelle:** CL
- **Technisch:** AR-Projektion eines nicht existierenden Objekts oder einer Person in den Overlay-Layer. Alle Implantat-Träger im Bereich nehmen das Phantom wahr.
- **Ebene:** MEATSPACE · **Schlüssel:** OVERLAY
- **HEAT:** Bandbreite (mittel), Prozessor-Hitze (niedrig)
- **Counterplay:** Physische Interaktion (greift ins Leere), Baseline-Augen, Overlay-Filter.
- **Spuren:** AR-Broadcast-Signatur. Phantom hat keine physische Präsenz.

### M4-OVL-05 · „Babel"
- **Quelle:** CL
- **Technisch:** Manipulation der Sprach-Overlay-Module im Neural-Link. Ziel hört alle gesprochene Sprache als unverständliches Rauschen.
- **Ebene:** MEATSPACE · **Schlüssel:** OVERLAY
- **HEAT:** Prozessor-Hitze (niedrig)
- **Counterplay:** Overlay-Neustart, Lip-Reading, schriftliche Kommunikation, Baseline-Gehör.
- **Spuren:** Neural-Link-Log zeigt Sprach-Modul-Anomalie. Effekt ist zeitlich begrenzt.

### M4-OVL-06 · „Letzte Worte"
- **Quelle:** CL
- **Technisch:** Rekonstruktion der letzten Overlay-Wahrnehmung eines Toten aus dem Cache seines Neural-Links. Operator erlebt die letzten Sekunden aus Perspektive des Opfers.
- **Ebene:** AETHER · **Schlüssel:** OVERLAY
- **HEAT:** Brain-Burn (hoch), psychische Belastung (extrem), Datenkorruption
- **Counterplay:** Cache-Löschung post mortem, verschlüsselter Neural-Link, Zeitverzug.
- **Spuren:** Psychische Nachwirkungen beim Operator. Neural-Link des Toten danach unbrauchbar.

### M4-OVL-07 · „Die Flüstermaske"
- **Quelle:** GP
- **Technisch:** Formt Audiosignale zu gezielter Bedeutungsverschiebung (psychoakustische Layer), sodass Worte anders „ankommen".
- **Ebene:** MEATSPACE · **Schlüssel:** OVERLAY
- **HEAT:** Neurologisch (hoch)
- **Counterplay:** White-Noise, Knochenleitung aus, Text-Transkript/Verifikation.
- **Spuren:** Tinnitus, Stressmarker, Audiolog-Inkongruenz.

### M4-OVL-08 · „Das Hexenlicht"
- **Quelle:** GP
- **Technisch:** AR-Markierungen/Glows auf Ziele via AR-Tagging, sichtbar nur für autorisierte Wahrnehmungskanäle.
- **Ebene:** NET · **Schlüssel:** OVERLAY
- **HEAT:** Netz/Signatur (niedrig)
- **Counterplay:** Tag-Scrubber, Kanal wechseln, Sichtmodus auf „raw".
- **Spuren:** Tag-IDs, Beacon-Pings, AR-Layer-Diff.

### M4-OVL-09 · „Der Salzspiegel"
- **Quelle:** GP
- **Technisch:** Dreht Erkennungsmodelle gegen sich selbst (adversarial overlay): Gesichter/Objekte werden falsch klassifiziert.
- **Ebene:** MEATSPACE · **Schlüssel:** OVERLAY
- **HEAT:** Netz/Signatur (hoch)
- **Counterplay:** Multi-modal (Thermal/Radar), menschliche Verifikation, Modell-Update.
- **Spuren:** Fehlalarme, Confusion-Matrix-Drift, stille Re-ID-Retries.

### M4-OVL-10 · „Der Falschheilige"
- **Quelle:** GP
- **Technisch:** Injiziert Reputation-Skin: eine Identität trägt visuelle/semantische Autorität (Uniform/Badge/Zertifikat) in fremden Overlays.
- **Ebene:** NET · **Schlüssel:** OVERLAY
- **HEAT:** Audit/Exposure (mittel)
- **Counterplay:** Zertifikat live prüfen, Challenge-Response, physische Token-Kontrolle.
- **Spuren:** Overlay-Badge-Hashes, Social-Graph-Anomalien, Mis-Auth-Events.

### M4-OVL-11 · „Der Traumfänger"
- **Quelle:** GP
- **Technisch:** Schreibt Mikro-Suggestionen in Schlaf-/Trance-Phasen (Neuro-Priming), um Entscheidungen am nächsten Tag zu beeinflussen.
- **Ebene:** AETHER · **Schlüssel:** OVERLAY
- **HEAT:** Neurologisch (extrem)
- **Counterplay:** Schlaf-Hygiene-Shield, EEG-Monitor, isolierte Schlafkammer (Dead Zone/Faraday).
- **Spuren:** Erinnerungslücken, REM-Anomalien, neurochemische Dysbalance.

### M4-OVL-12 · „Der Stumme Chor"
- **Quelle:** GP
- **Technisch:** Synchronisiert Wahrnehmungsframes einer Gruppe (shared overlay), damit alle „dasselbe sehen" — für Täuschung oder Koordination.
- **Ebene:** AETHER · **Schlüssel:** OVERLAY
- **HEAT:** Aether/Signatur (mittel)
- **Counterplay:** Teilnehmer trennen, individuelle Kalibrierung, Shared-Key rotieren.
- **Spuren:** Identische Frame-IDs, Gruppen-Sync-Pings, kollektiv gleiche Fehlwahrnehmung.

### M4-OVL-13 · „Neon-Wüste"
- **Quelle:** GE
- **Technisch:** Löschung aller nützlichen AR-Navigationsdaten in der Wahrnehmung des Ziels. Totale Desorientierung in urbaner Umgebung.
- **Ebene:** MEATSPACE · **Schlüssel:** OVERLAY
- **HEAT:** Hitze (niedrig)
- **Counterplay:** Analoger Kompass/Karte, Baseline-Orientierung, Overlay-Neustart.
- **Spuren:** AR-Blackout-Log im Neural-Link.

---

## STATISTIK

| | HELIX | PACT | OVERLAY | **Σ** |
|---|---|---|---|---|
| **MEATSPACE** | 5 | 3 | 8 | 16 |
| **NET** | 7 | 7 | 3 | 17 |
| **AETHER** | 1 | 3 | 3 | 7 |
| **Σ** | **14** | **13** | **13** | **40** |

| Power-Level | Anzahl |
|---|---|
| niedrig | 8 |
| mittel | 16 |
| hoch | 11 |
| extrem | 5 |

| Quelle | Beitrag |
|---|---|
| CL (Claude) | 18 Effekte |
| GP (GPT) | 14 Effekte |
| GE (Gemini) | 5 Effekte (+3 Konsens-Merges) |

---

## NICHT AUFGENOMMEN (Reservebank für v0.2)

Starke Effekte, die knapp rausgeflogen sind — Kandidaten für Erweiterung:

| ID | Street Name | Quelle | Grund für Reserve |
|---|---|---|---|
| GP-HLX-02 | Der Knochenwürfel | GP | Zu abstrakt für Starter-Set, gut für Advanced |
| GP-HLX-06 | Der Nachtwächter | GP | Nah an „Wurzeln schlagen", überschneidet sich |
| GP-HLX-07 | Die Leere Lunge | GP | Biofeedback-Loop — stark, aber nischig |
| GE-HLX-06 | Bannkreis ziehen | GE | Signal-Jamming — stark, ähnelt Dead-Zone-Konzept |
| GE-HLX-08 | Echorausch | GE | DDoS — stark, aber generisch |
| CL-PCT-05 | Beichte | CL | Audit-Erzwingung — ähnelt Beichtgeheimnis (inverse) |
| CL-PCT-06 | Seelenkauf | CL | Vertragsübernahme — extrem, für v0.2 Advanced |
| GE-PCT-05 | Sklavenpakt | GE | Geräte-Übereignung — ähnelt Seelenkauf |
| GE-PCT-07 | Atemzoll | GE | Med-Implantat-Sperre — ähnelt „Herz stillstehen" (HELIX) |
| GP-PCT-05 | Der Blutpfand | GP | Biometrische Kopplung — stark, nischig |
| CL-OVL-06 | Alptraum | CL | Neuro-Folter — ähnelt Traumfänger, weniger subtil |
| CL-OVL-08 | Orakel | CL | Aether-Query — stark, für Advanced (zu mystisch für v0.1) |
| CL-OVL-09 | Stimmen | CL | Audio-Phantome — ähnelt Flüstermaske |
| GE-OVL-05 | Ghost-Walk | GE | Bewegungsmaskierung — ähnelt Schleier |
| GE-OVL-10 | Schattenkuss | GE | Cyberware-Licht dimmen — zu subtil für v0.1 |

---

*M4 Technomancy-Katalog v0.1.1 (lint-pass) · REALMVOID · 2026-02-10*
*Lint: Ebenen normalisiert, HEAT auf Single-Type, ethische Flags → Spuren*
