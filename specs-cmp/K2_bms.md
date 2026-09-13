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

## 6. Normen & Zertifizierung (offen bis OQ-11)

- EN 62133 (Zelltest) + BMS-Schutzfunktionen in der Systemprüfung; CE; Transport UN3480.
- Abuse-/Normprüfungen gemäß OQ-11 vor Serien-Zertifizierung.

## 7. Offene Punkte

| Punkt | Status |
|-------|--------|
| Konkreter BMS-IC/Board | nach Spezifikation + Angebot zu wählen |
| Parametrier-Set (Schwellen, Hysterese, Derating) | Phase-P-Feintuning je Zell-Datenblatt |
| OQ-11 Norm-/Abuse-Rahmen | offen (vor Zertifizierung) |

## 8. Traceability & Status

- SYS-Trace: S-01…S-05, F-02, F-07, N-01, N-08 (SYS-Ebene geschlossen via docs/03, KN-1).
- Komponenten-Spezifikation K2-BMS → Grundlage der **BMS-Auswahl** (kein SWE-Folgeprojekt).