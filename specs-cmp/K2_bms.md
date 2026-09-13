# Komponenten-Spezifikation K2 · BMS (Schutzmodul PE-09 + Sensorik PE-10 + Lastschalter PE-06)

| | |
|---|---|
| Komponente | K2 — BMS-Schutzmodul (PE-09) inkl. Thermal-Sensorik (PE-10) und Lastpfad-Schalter (PE-06) |
| Zweck | Komponenten-Auswahl nach Spezifikation (SYS-Ebene; **keine SWE-Phase**) |
| Stand | 2026-09-13 |
| Status | Spezifikation bereit zur BMS-/Schalter-Auswahl |
| Bezugsdokumente | `../docs/01_spezifikation_anforderungen.md` (S-01…S-05, F-02, F-07, N-01, N-08) · `../docs/03_spezifikation_architektur_physisch.md` (PE-09, PE-10, PE-06, PE-08, PF-05…PF-13, KN-1) |

## 1. Einordnung

Das BMS ist die zentrale Schutz-/Überwachungs-Einheit des Packs (K2). Es konsolidiert
die logischen Schutzlogik-Funktionen **LE-S1 (Tiefentladung), LE-S3 (Kurzschluss/
Überstrom), LE-S4 (Verpolung), LE-S5 (Thermomanagement)** in einem physischen
Schutzmodul und kommandiert den Lastpfad-Schalter (PE-06). Es wirkt zusätzlich
als redundanter Überladeschutz im Ladepfad (Rückkanal PF-11 zum externen Ladegerät K1).

## 2. Funktionale Anforderungen (MUSS)

| ID | Anforderung | Bezug SYS |
|----|-------------|-----------|
| K2B-F01 | **Zellspannungs-Überwachung je Serienposition** (3S, Mittelanzapfungen), Schwellwert-Auswertung | S-01, PF-05 |
| K2B-F02 | **Tiefentladeschutz:** Abschaltgrenze **≤ 2,5 V/Zelle** (7,5 V Pack unter Last), zwingend (R8) | S-01 |
| K2B-F03 | **Kurzschluss-/Überstromschutz:** Trennung im Fehlerfall (S-03) bei korrektem Betrieb von Lastpulsen (F-02) — Schwellen parametrierbar | S-03, F-02 |
| K2B-F04 | **Verpolungsschutz (logisch):** Verpolungserkennung + Strompfad-Blockade; unterstützt mech. Sicherung an den Klemmen (S-04) | S-04 |
| K2B-F05 | **Thermomanagement:** Temperatur-Erfassung (NTC/Sensoren PE-10) am Zellblock und Gehäuse; **Ladestopp ≥ 50 °C, Entladestopp ≥ 60 °C** (fixiert, je Zell-Datenblatt Hysterese/Default) | S-05 |
| K2B-F06 | **Redundanter Überladeschutz** im Ladepfad: unabhängige Abschaltung bzw. Freigabe-Entzug via Rückkanal PF-11 zum externen Ladegerät (S-02) | S-02 |
| K2B-F07 | **Lastschalter (PE-06):** MOSFET-Schalter, im Normalbetrieb einlassend, **puls-/impulsfest** (Lastpulse F-02 ohne Fehlauslösung), schnelle Trennung im Fehlerfall, **Wiedereinschaltfähigkeit** | F-02, S-03 |
| K2B-F08 | **Strommessung** für Laden/Entladen (Sensoren), Grundlage der Überstrom- und Ladezustands-Auswertung | S-03, N-01 |
| K2B-F09 | **Balancierung** (Referenz: passiv) der 3S-Positionen im CV-Block | S-02, PF-05 |
| K2B-F10 | **Parametrierbarkeit:** Schwellen (Spannung/Strom/Temperatur), Variante Solar (K3) und Laderegler-Verhalten einstellbar | S-01…S-05 |

## 3. Leistungs-/Umweltanforderungen

| ID | Kriterium | Anforderung | Bezug |
|----|-----------|-------------|-------|
| K2B-N01 | Eigenverbrauch | **< 1 mA** (Einfluss auf die 504-h-Bilanz ist zu quantifizieren; Ziel ≈ < 0,5 Ah) | N-01 |
| K2B-N02 | Schutz ohne Fehlauslösung | Keine Fehlauslösung durch Zaun-Hochspannungsimpulse (F-07) — EMV-Beschaltung PE-08 wirksam | F-07 |
| K2B-N03 | Durchlasswiderstand Lastschalter | kleine Zehn-mΩ-Klasse (Verlustleistungs-Referenz minimiert) | F-02 |
| K2B-N04 | Temperaturbereich | −10…+40 °C Sensor-Betrieb; Schwellen nach Datenblatt | N-02 |
| K2B-N05 | Zuverlässigkeit | ≥ 6 Nutzsaisons, Wartungsfrei (N-05-Konzept) | N-05 |
| K2B-N06 | Sensoranzahl | mehrere Messpunkte je Zellpfad/Gehäuse (Punktezahl je Abuse-/Normauslegung OQ-11) | N-02, S-08 |

## 4. Schnittstellen innerhalb K2 / nach außen

| Kante | Von → Nach | PF-Bezug | Charakteristik |
|-------|------------|----------|----------------|
| Zellmessung/Balancierung | Zellblock ↔ BMS | PF-05 | je Serienposition; NTC-PE-10 |
| Lastpfad | Zellblock ↔ BMS-Lastschalter | PF-06 | niederohmig, kurz |
| Schaltausgang | BMS → Klemmen | PF-07 | zur PE-07 |
| Ladepfad-Rückkanal | BMS → Ladegerät K1 | PF-11, PF-03 | über Ladekabel PF-01 |
| Laderegler-Status | Ladegerät ↔ BMS | PF-03 | Ladekommando/Ladeende |
| Temperatur | PE-10 → PE-09 | PF-12 | analog/digital |
| Stör-Entkopplung | PE-08 ↔ PE-07/PE-01 | PF-09 | TVS/Filter |

## 5. Auswahl-/Bewertungskriterien (Komponentenauswahl)

| # | Kriterium | Referenzwert | Beurteilung |
|---|-----------|--------------|-------------|
| 1 | Schutzfunktions-Umfang | Tiefentlade/Überstrom/Verpol/Thermo/Überlade (redundant) komplett | BMS-IC-/Board-Spez |
| 2 | Zellanzahl/Topologie | 3S (3 Serienpositionen), Mittelanzapfungen unterstützt | Konfiguration |
| 3 | Eigenverbrauch | < 1 mA | Datenblatt |
| 4 | Lastschalter-Stromfähigkeit | Puls-/Fehlerfall-stromfeste MOSFET-Auswahl (Zehn-mΩ-Klasse) | Sim/I-V-Kurve |
| 5 | Verzögerungs-/Fehlauslöse-Verhalten | Pulsmuster F-02 vs. Fehlergrenzen S-03 disproportional | Parametrierung/Oszilloskop |
| 6 | EMV-Festigkeit | keine Fehlauslösung durch Zaunimpulse | Normprüfung F-07 |
| 7 | Temperatur-Schwellen | fixierbar 50/60 °C (+Hysterese +Derating) | Parametrierbarkeit |
| 8 | Redundanz Ladepfad | eigener Schutzkanal für Überladung (S-02), Rückkanal PF-11 | Kanalstruktur |
| 9 | Balancierung | passiv ausreichend (CV-Block), optional aktiv | Board-Option |
| 10 | Preis/BOM | Budget Q7 (≤ 120 EUR Gesamt, Anteil BMS) | Stückkosten |
| 11 | Zertifikate | EN 62133-kompatibel, CE; Herstellertest-Hinweis | Zulassungen |

## 5.1 Auswahl-Kandidat: 3S LiFePO4 BMS „mit Balance" (12 A / 15 A) — Stand 2026-09-13

| Kennwert | Wert | Bezug Ziel / Bewertung |
|----------|------|-------------------------|
| Hersteller | **Lisolec** (Label der günstigen 3S-LiFePO4-BMS-Klasse), LiFePO4, **15-A-Ausführung mit Balance** | Kandidat nach Spezifikation K2B; Kauf über AliExpress 1005007786624222 |
| Typ | 3-S BMS für **LiFePO4**, mit Balancierung (passiv) | K2B-F01 ✓, K2B-F09 ✓ (passiv = Referenz) |
| Stromauslegung | **15 A Dauer-Entlade / 7 A Dauer-Lade** (gewählt: 15-A-Ausführung, LiFePO4) | Last-Zyklus 30–60 mA + Pulse weit darunter → Auslegung unkritisch (K2B-F07) |
| Maße (Klasse) | typ. ≈ 56 × 45 × 3,5 mm (Verpackung Bestätigung), ≤ 20 mΩ Hauptpfad | K2B-N03 ✓ (Zehn-mΩ-Klasse) |
| Preis | 2 Stück = 5,36 € ⇒ **2,68 €/Stück** (1 verbaut + 1 Reserve) | OQ-07 sehr gut |

**Referenzkennwerte dieser BMS-Klasse (3-S LiFePO4, 7–15 A, „Balance") —
verifizierte Datenblatt-Werte (Bestechpower LTF3S1-15A, AYAA PCM-L03S06-337,
KLS/KLS-055, EYBMS PCM-L03S40):**

| Parameter (Klasse) | typ. Wert | Prüfpunkt gegen Spez |
|---------------------|-----------|-----------------------|
| Ladespannung | N × 3,65 V ⇒ Pack ≈ 10,95 V | passt F-01 / K1-CV-Profil (N-08) ✓ |
| Lade-Abschaltung (Überlade) | **3,65 V ± 0,025 V** (Bestechpower/AYAA-Spez) — Release ~3,55 V | deckend für S-02 (BMS redundant zum CV) ✓ |
| Balance-Startspannung | **3,6 V**, Balance-Strom **36–58 mA** (modellabhängig) | passiv möglich (K2B-F09) ✓ |
| Balance-Funktion | bei „mit Balance"-Varianten vorhanden (Bestechpower), einige ohne (EYBMS 40A) | beim Kauf prüfen (K2B-F09) |
| **Überentladungsschutz** | **2,5 V ± 0,05 V** (Bestechpower, AYAA); **2,0 V** (EYBMS); **2,7 V** (KLS) | **S-01: 2,5-V-Variante ist in dieser Klasse verfügbar und gefordert** |
| Entlade-Überstromschutz | ~20–45 A, Verzögerung ~10–100 ms | gegen Lastpulse F-02 (Pulsmuster) abzugrenzen |
| Kurzschlussschutz | externe Last, Ansprech **0,35–0,8 ms**, Release = Lastfrei | K2B-F03 ✓ |
| Eigenverbrauch (Klasse) | **≤ 20 µA** (AYAA/EYBMS-Spez, Ruhezustand < 0,1 µA) | K2B-N01 ✓ (< 1 mA, großzügig) |
| Temperaturschutzeingang (NTC) | **modellabhängig** (KLS: vorhanden, 55 °C Laden / 70 °C Entladen; EYBMS: vorhanden; Bestechpower: k.A.) | K2B-F05 → bei Wahl bevorzugen, sonst separate PE-10-Sensorik |
| Verpolungsschutz | **i. d. R. nicht belastbar vorhanden** | K2B-F04 → über PE-02-Eingangsschutz + mech. Klemmen-Sicherung S-04 abdecken |
| Maße | 56×45×3,5 mm (t.io) bis 158×45×10 mm (Bestechpower-Klasse) | K2B-N03-Formfaktor in Pack prüfen |

**Erkenntnis (2026-09-13):** Die Klasse bietet **2,5-V-Überentlade-Abschaltung**
(Bestechpower, AYAA) — der S-01-Referenzwert ist damit **real verfügbar**, nicht nur
Wunschziel. Die **exakte Schwelle des Lisolec-Modells** (2,5 V, 2,15 V oder 2,0 V)
bestimmt letztlich die Eignung und ist vor Kauf zu bestätigen ($5.1-Kaufbeleg).

### 5.2 Kaufbeleg / Listing-Verifikation (2026-09-13)

- **Shop/Link:** AliExpress — `de.aliexpress.com/item/1005007786624222.html`
- **Listing-Titel (verifiziert):** „LiFePO4 3S with Balance BMS 12A 15A 22A Protect
  Plates Charging Module for 9.6V 18650 32650 32700 Lithium Iron Phosphate Battery"
- **Bestätigt:** **LiFePO4-Chemie + 9,6-V-Pack (3S) mit Balance** — passt zu K2B-F01/F09 ✓
- **Gekaufte Ausführung (Nutzerangabe 2026-09-13):** **15-A-Variante — Chemie LiFePO4**,
  **Dauer-Entladestrom 15 A, Dauer-Ladestrom 7 A** („15A/7A") | Lastprofil 30–60 mA +
  Pulse (F-02) und K1-Ladung ≥ 30 W ≈ 3,2 A liegen **weit unter** den Board-Grenzen →
  Auslegung unkritisch (K2B-F07, N-01-Bilanz) ✓
- **Nicht extrahierbar:** die **exakten Schwellwerte** (Überentlade 2,5 V?, Überlade
  3,65 V?, NTC?, Balance-Strom, Maße) — AliExpress liefert die Spec als Bild/Javascript,
  nicht als Text; Werte nur durch Kaufbeleg-Bild oder **Eingangsmessung (build/AUFBAU.md)**
  zu verifizieren.

**Wichtiger Chemie-Hinweis (Differenzierung Klasse):** Der Referenz-Suchtreffer zur
Board-Klasse „DL-J04G3-L03S15ATJ" (tinytronics-PDF) beschreibt eine **3,7-V-Chemie-
Variante** (12,6 V / 4,125 V-Schwellen) — **nicht** einsetzbar. Das Lisolec-Label
vertreibt dieselbe Platinenfamilie **auch als LiFePO4-Version**; es muss explizit die
**9,6-V-LiFePO4-Variante** (Ladeschluss ~3,65 V/Zelle) bestellt werden, sonst
K2B-F02/S-01/S-02 verletzt (siehe kritischer Prüfpunkt 1).

**Kritische Prüfpunkte vor Freigabe:**
1. **Chemie-Variante:** Es muss die **LiFePO4-Version** sein (Ladeschluss ~3,65 V/
   Zelle); eine 4,2-V-Lithium-Variante ist **nicht** einsetzbar (K2B-F02, S-01/S-02).
2. **Überentladeschwelle:** Muss **≤ 2,5 V/Zelle** liegen (S-01, DoD ≈ 80 %, N-07) —
   Klasse liefert 2,5 V (Bestechpower); **2,0-/2,15-V-Modelle ausschließen** oder
   Zusatzabscheidung vorsehen. (**Achtung:** 2,0-V-Abschaltung ⇔ DoD > 80 % gefährdet
   N-07, siehe Zellen-Datenblatt 2,0-V-Abschluss.)
3. **Temperaturmanagement (K2B-F05):** nach NTC-Ausführung am Lisolec-Stück fragen —
   vorhanden = Vorteil, sonst separate Sensorik PE-10 + eigene Auswertung.
4. **Verpolungsschutz (K2B-F04):** über **PE-02 (Lade-Eingangsschutz) + mechanische
   Klemmen-Sicherung S-04 (PE-07)** sicherstellen; BMS trägt nur bei (je nach Chip).
5. **Überstrom-/Kurzschlussschutz (K2B-F03):** Schwelle + Auslösezeit gegen die
   Lastimpulse der Zielgeräte abzugrenzen (Reihenmessung F-02/F-07).

**Nächster Schritt:** Bestell-Beleg der 15-A-LiFePO4-Ausführung sichern; **Eingangsmessung**
der konkreten Schwellwerte (Überlade/Überentlade, Balance-Anlauf, NTC, Maße) beim
Wareneingang gemäß `build/AUFBAU.md`; Mess-Werte in `build/DATENBLATT_AKKU.md` §6 eintragen.

## 6. Normen & Zertifizierung (offen bis OQ-11)

- EN 62133 (Zelltest) + BMS-Schutzfunktionen in der Systemprüfung; CE; Transport UN3480.
- Abuse-/Normprüfungen gemäß OQ-11 vor Serien-Zertifizierung.

## 7. Offene Punkte

| Punkt | Status |
|-------|--------|
| **Lisolec 15-A-Ausführung im Kauf** — Chemie LiFePO4 (9,6 V), 15 A Entladen/7 A Laden (Nutzerangabe) | AliExpress 1005007786624222; exakte Schwellwerte erst per Eingangsmessung verifizierbar — **Warenkorb-Konfiguration gegen Beleg prüfen** |
| Balance-Variante prüfen (einige 3-S-Boards ohne Balance, z. B. EYBMS 40 A) | Kaufbeleg |
| Parametrier-Set (Schwellen, Hysterese, Derating) | Phase-P-Feintuning je Zell-Datenblatt |
| OQ-11 Norm-/Abuse-Rahmen | offen (vor Zertifizierung) |

## 8. Traceability & Status

- SYS-Trace: S-01…S-05, F-02, F-07, N-01, N-08 (SYS-Ebene geschlossen via docs/03, KN-1).
- Komponenten-Spezifikation K2-BMS → Grundlage der **BMS-Auswahl** (kein SWE-Folgeprojekt).