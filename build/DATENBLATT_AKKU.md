# Datenblatt · Akku-Pack K2 (3S1P LiFePO4, Zellen + BMS)

| | |
|---|---|
| Bezeichnung | Weidezaun-Akku-Pack **K2** — wiederaufladbar 9,6-V-LiFePO4 |
| Aufbau | 3 × VariCore 3,2 V/32 Ah (3S1P) + Lisolec 3S-LiFePO4-BMS (15 A/7 A) + **Sicherung F1 5 A in P+** |
| Zweck | **Datenblatt des fertigen Packs** (= Zellen + BMS verbaut) — Track agiert als Registrierstelle |
| Stand | 2026-09-13 (angelegt; Messwerte beim Aufbau eintragen) |
| Bezug | `specs-cmp/K2_zellen.md` §5.1, `specs-cmp/K2_bms.md` §5.1/5.2, `docs/01…03, 06` |

> Einzelkomponenten-Datenblätter (Zelle, BMS-Leiter) bleiben beschaffungsseitig
> Quellenangabe; **dieses Blatt beschreibt den fertigen Akku als System**.

## 1. Elektrische Kenndaten

| Kenngröße | Wert | Anmerkung |
|-----------|------|-----------|
| System-/Bauform | Akku-Pack K2, 3S1P | 3 Serienzellen à 1 Parallel |
| Technologie | LiFePO4 (lithium-eisen-phosphat) | |
| Nennspannung | 9,6 V (3 × 3,2 V) | F-01 |
| Spannungsbereich | 7,5–10,95 V (2,5–3,65 V/Zelle) | F-01 |
| Ladespannung (CV) | 10,95 V (3,65 V/Zelle) | N-08 |
| Ladeschluss-Abschaltung (BMS) | ~10,95 V (3,65 V/Zelle) | redundant zu K1-CV |
| Nennkapazität | **≥ 31 Ah** (Ziel) / 32 Ah (Zellnennwert) | N-01 |
| Nennenergie | ≈ 298–307 Wh | N-01/K2Z-N03 |
| Laden | bis 7 A (BMS-Grenze), Zielbetrieb ≥ 30 W (≈ 0,1 C) | 15 A-Entlade-/7 A-Lade-Variante |
| Entladen | bis 15 A (BMS-Grenze), Betriebslast 30–60 mA + Pulse | F-02 |
| Selbstentladung | ≤ 5 %/Monat (Ziel) — messen | N-04 |
| Innenwiderstand Pack | (messen; Zelle ~3 mΩ) | K2B-N03 |

## 2. Schutzfunktionen (BMS)

| Schutz | Schwelle (Ziel) | Status |
|--------|-----------------|--------|
| Tiefentladung (Überentlade) | Abschaltgrenze ≤ 2,5 V/Zelle (7,5 V Pack) | S-01; **Eingangsmessung nötig** |
| Überladung | ~3,65 V/Zelle Abschaltung | S-02; redundant |
| Überstrom Entladen | ~20–45 A (Klasse), Ansprech 10–100 ms | S-03; gegen F-02-Pulsmuster prüfen |
| Kurzschluss | < ~1 ms, Release Lastfrei | S-03 |
| Temperatur | Ladestopp ≥ 50 °C / Entladestopp ≥ 60 °C | S-05; Umsetzung über PE-10 Sensorik |
| Verpolung | mechan. Klemmen-Sicherung + PE-02-Eingangsschutz | S-04 |
| Balancierung | passiv, Anlauf ~3,6 V | K2B-F09 |
| **Pfadsicherung F1** | **5 A** (in P+-Leitung) | zusätzlicher Plus-Pfadschutz; Wert/Charakteristik beim Aufbau festlegen |

## 3. Thermische Kenndaten

| Kenngröße | Wert |
|-----------|------|
| Laden | **+0…+40 °C** — **nicht unter 0 °C laden** (Kaltladen schädigt LiFePO4) |
| Entladen | **−10…+40 °C** — **nicht unter −10 °C betreiben** |
| Lagern | (Zellendatenblatt; für Lagersaison geeignet) |

## 4. Mechanische Kenndaten

| Kenngröße | Wert |
|-----------|------|
| Pack-Außenmaße **Ist** (2026-09-13) | **18,4 × 13,4 × 8,9 cm** (B × H × T) ≈ **2,19 l** |
| Pack-Außenmaße (max. Vorgabe Fach) | B 185 × H 155 × T 125 mm (3,58 l) — **Ist liegt darunter ✓** |
| Zellblock | 3 × VariCore 142/145 × 100 × 21/22 mm (je ~0,3 l, ~0,63 kg) |
| Pack-Gewicht **Ist** (2026-09-13) | **2,6 kg** (komplett mit Gehäuse/BMS) |
| Pack-Gewicht (Schätzung Zellblock) | ≈ 1,9 kg (Zellen) + Gehäuse/BMS ≈ 2,6 kg ✓ |
| Fach-/Passung | Drop-in bestätigt (M2/OQ-12) |
| Anschlüsse | Ladebuchse PE-02, Pack-Zuleitung/Last PE-03/PE-07, Zellabgriffe zum BMS |
| Sicherung F1 | 5 A in P+ (vor Klemme +), Austausch-/Prüfpunkt dokumentiert |

## 5. Normen / Transport

| Kenngröße | Wert |
|-----------|------|
| Zellnorm | EN 62133-Test (Ziel; OQ-11) |
| Transportklasse | UN3480 (Li-Ionen-Transport) |
| CE | vorgesehen (OQ-11) |

## 6. Messprotokoll (Eingangsprüfung beim Aufbau — auszufüllen)

| Messpunkt | Datum | Soll | Ist | OK |
|-----------|-------|------|-----|----|
| Zellspannungen 3S (Leerlauf) | | 2,5–3,65 V/Zelle | | ☐ |
| Pack-Spannung Gesamt | | 7,5–10,95 V | | ☐ |
| BMS-Überentladeschwelle | | ≤ 2,5 V/Zelle | | ☐ |
| BMS-Überladeschwelle | | ~3,65 V/Zelle | | ☐ |
| BMS-NTC/Balance-Nachweis | | vorhanden/funktional | | ☐ |
| **Sicherung F1 geprüft** | | **5 A in P+ (Durchgang, korrekt platzierte Einbaulage)** | | ☐ |
| **Maße/Gewicht Pack** | **2026-09-13** | ≤ Volumen-Budget (3,58 l) | **Ist: 18,4 × 13,4 × 8,9 cm ≈ 2,19 l** | **☐** |
| **Gewicht Pack** | **2026-09-13** | ≤ Zielgewicht | **2,6 kg** | **☐** |

## 7. Traceability

- SYS-Level: F-01, N-01, N-02, N-04, N-07, S-01…S-05; Komponenten K2Z/K2B.
- Verifikation: derzeit SYS-Abdeckung 22/23 (nur N-07/OQ-07 offen); Pack-Nachweise gemäß
  Messprotokoll §6 verdichten.