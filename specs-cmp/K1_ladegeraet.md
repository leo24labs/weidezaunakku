# Komponenten-Spezifikation K1 · Ladegerät mit integriertem Laderegler

| | |
|---|---|
| Komponente | K1 — Ladegerät (extern, mit integriertem Laderegler PE-04) |
| Zweck | Komponenten-Auswahl nach Spezifikation (SYS-Ebene; **keine SWE-Phase**) |
| Stand | 2026-09-13 |
| Status | Spezifikation bereit zur Komponentenauswahl |
| Bezugsdokumente | `../docs/01_spezifikation_anforderungen.md` (F-05, N-08, S-02, S-06) · `../docs/03_spezifikation_architektur_physisch.md` (Abschnitt 1.4, §3 PE-04/PE-05, §4 PF-01/PF-03/PF-08/PF-11) |
| Gesamtbudget | Zielkosten Pack **inkl. K1-Set** ≤ 120 EUR (N-07, OQ-07) |

## 1. Einordnung

Das Ladegerät ist das **externe** K1-Pack-Set (Konstruktionsorts-Entscheidung
2026-09-13): 230-V-Netzteil **mit integriertem Laderegler (CC/CV)** in einem
Gehäuse. Es speist über ein SELV-Ladekabel (PF-01) die Pack-Ladebuchse (PE-02);
das Pack selbst ist **reglerfrei** (nur Buchse + Zuleitung PE-03). Eine optionale
Solarbuchse (PE-05) mit MPPT-fähigem Laderegelpfad macht das Gerät
Solar-fähig (K3, Kompatibilitätsnachweis F-06).

## 2. Funktionale Anforderungen (MUSS)

| ID | Anforderung | Bezug SYS |
|----|-------------|-----------|
| K1-F01 | Wandelt 230 V AC (50 Hz) in eine **netzgetrennte SELV-Gleichspannung** (galvanische Trennung im Ladegerät, S-06) | S-06 |
| K1-F02 | Führt den Ladeablauf **CC → CV** für LiFePO4-3S: CV = **3,65 V/Zelle ⇒ ≤ 10,95 V**, CC-Referenz **≈ 3 A (≥ C/10, Ladeleistung ≥ 30 W)** | N-08, docs/03 PE-04 |
| K1-F03 | Beendet den Ladevorgang über ein **Ladeendekriterium** (Strom-Abklang im CV-Block, Referenz: Strom ≤ ~C/50) | F-05, N-08 |
| K1-F04 | **≤ 12 h Voll-Ladung** (inkl. CV-Abschlussblock + Temperatur-Derating) sind nachweisbar | N-08 |
| K1-F05 | Überträgt Ladezustand bzw. **Ladeende-/Fehler-Status** zum Pack (Status-Leitung in PF-01 → PF-11, kompatibel zum BMS-Rückkanal) | S-02, docs/03 PF-03/PF-11 |
| K1-F06 | Realisiert die **primäre Überladescontrolle** und akzeptiert den **redundanten** Trenn-Befehl des BMS (S-02, Rückkanal PF-11) | S-02 |
| K1-F07 | Ausgangsseite **verpolungssicher** bzw. verpolungsgeschützt (harmonisiert mit PE-02/PF-01) | S-04 |
| K1-F08 | Optionale **Solarbuchse PE-05**: Annahme von Panelenergie (Variableleistung) mit **MPPT-fähigem** Ladereglerpfad ohne Fehlabbruch | F-06, OQ-06 |
| K1-F09 | SOP/Parametrierung: Ladeprofil (CV-Spannung, CC-Strom, Abschluss-Schwellen, Derating) **parametrierbar** für die 3S-LiFePO4-Referenz | N-08, S-02 |

## 3. Leistungs-/Umweltanforderungen

| ID | Kriterium | Anforderung | Bezug |
|----|-----------|-------------|-------|
| K1-N01 | Ladespannung | SELV **12–15 V** (Referenzklasse; Ausgang an Pack-Buchse) | F-05, docs/03 PF-01 |
| K1-N02 | Nennausgangsleistung | ≥ 30 W (≈ C/10), kurzzeitig CV-Erhalt | N-08 |
| K1-N03 | Betriebstemperatur Laden | **+0…+40 °C** (Derating oberhalb Referenztemperatur) | N-02 |
| K1-N04 | Schutzart / Umgebung | Ladegerät im Außen- und Stallumfeld: **mind. IP54** bevorzugt (Einzelfestlegung OQ-11); Pack-Seite IPx4 | N-03, OQ-11 |
| K1-N05 | Galvanische Trennung | Netzseitig getrennt (SELV), Prüfspannung/Isolation gemäß OQ-11 | S-06 |
| K1-N06 | Netz-Kompatibilität | 230 V AC 50 Hz, Europa-Netzklasse (Normangabe OQ-11) | — |
| K1-N07 | Ladekabel | PF-01 mit Stecker passend zur Pack-Buchse PE-02 (Koaxial-/Hohlstecker-Klasse); Polaritätskennzeichnung; ausreichend Kabelrigidität für Stallbetrieb | S-06, PF-01 |
| K1-N08 | Kostenanteil | K1-Anteil am Gesamtbudget ≤ 120 EUR inkl. Zubehör (OQ-07) | OQ-07 |
| K1-N09 | Zuverlässigkeit | ≥ 6 Nutzsaisons ohne Wartung (N-05-Konzept; Ladegerät-Elektronik ausgelegt auf Arbeitspunkt) | N-05, N-07 |

## 4. Schnittstellen

| Kante | Von → Nach | PF-Bezug | Charakteristik |
|-------|------------|----------|----------------|
| Netz | 230-V-Netz ↔ K1 | — | Netz-Stecker |
| Lade-SELV | K1 → Pack-Buchse PE-02 | PF-01 | 12–15 V, Polaritätskennzeichnung |
| Status/Rückkanal | K1 ↔ BMS (Pack) | PF-03, PF-11 | Logik-Pegel über Ladekabel; Ladeende/Fehler, Freigabe Trenn-Befehl |
| Solar (opt.) | Panel K3 → K1 | PF-04, PE-05 | Panelenergie → PE-04 (MPPT) |

## 5. Auswahl-/Bewertungskriterien (Komponentenauswahl)

| # | Kriterium | Referenzwert | Bewertung (Auswahl) |
|---|-----------|--------------|---------------------|
| 1 | Ladeprofil LiFePO4-3S | CC ≈ 3 A, CV 10,95 V, Abschluss-Kriterium | eEPROM-/Dreh-konfigurierbarer Laderegler-IC |
| 2 | Ladeleistung | ≥ 30 W (≥ C/10) | Netzteil-Klasse ≥ 30 W |
| 3 | SELV + galvanische Trennung | EN/VDE-Class-II-Netzteil | Zertifizierungsnachweis (CE) |
| 4 | Status-/Trenn-Rückkanal | PF-11 kompatibel | Zusätzliche Steuerleitung im Ladekabel oder Kabel-Fuß |
| 5 | Solar-MPPT-Option (K3) | PE-05 + MPPT-Pfad | Panel-Spannungsbereich nach OQ-06 |
| 6 | Parametrierbarkeit | Schwellen/Derating | Schutz-IC bzw. µC-Parametrierung |
| 7 | Temperaturbereich | +0…+40 °C Laden | Betriebsdatenblatt |
| 8 | Gehäuse | min. IP54, Stall-tauglich | IP-Schutzklasse + Halterung |
| 9 | BOM-Kosten | OQ-07-Budget | Stückkosten-Kalkulation |

## 6. Normen & Zertifizierung (offen bis OQ-11)

- Netzteil: VDE/EN-IEC 62368-1-bzw. Niederspannungs-/LV-Rahmen (Festlegung OQ-11).
- CE-Konformität; EMV: Ableitung F-07/PE-08-mäßig geprüft.
- Risiko: OQ-11-Entscheidung vor finaler Netzteil-Wahl.

## 7. Offene Punkte

| Punkt | Status |
|-------|--------|
| OQ-06 Solar-Panel-Spannungs-/Leistungsklasse | offen → determiniert K1-PE-05/MPPT-Bereich |
| OQ-11 Normrahmen (VDE/EN, EMV, IP) | offen → vor Netzteil-Zertifizierung |
| OQ-07 Kostenfreigabe (≤ 120 EUR inkl. K1) | offen (Business-Review) |

## 8. Traceability & Status

- SYS-Trace: F-05, N-08, S-02, S-06 (geschlossen auf SYS-Ebene via docs/03);
- Komponenten-Spezifikation K1 → Grundlage der **Komponentenauswahl** (kein SWE-Folgeprojekt).