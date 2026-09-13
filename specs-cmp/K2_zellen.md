# Komponenten-Spezifikation K2 · Zellen (LiFePO4-3S-Zellblock, PE-01)

| | |
|---|---|
| Komponente | K2 — Batterie-Zellblock (PE-01), 3S-LiFePO4 |
| Zweck | Komponenten-Auswahl nach Spezifikation (SYS-Ebene; **keine SWE-Phase**) |
| Stand | 2026-09-13 |
| Status | Technologie final: **LiFePO4 3S**; Spezifikation bereit zur Zellauswahl |
| Bezugsdokumente | `../docs/01_spezifikation_anforderungen.md` (F-01, N-01, N-02, N-04, N-07, S-01) · `../docs/03_spezifikation_architektur_physisch.md` (PE-01, PE-02/PE-03, PF-05/PF-08) · docs/06 (M1/M2 bestätigt 2026-09-13) |

## 1. Einordnung

Der Zellblock ist der Energiespeicher des Packs (K2). Technologie **LiFePO4**,
Topologie **3S** (drei Serienpositionen, je Position Parallelbündel für die
Zielkapazität). Formfaktor optimierbar (Zylinder-26-mm-Klasse oder Prismatik/
Pouch) innerhalb der bestätigten Fachgeometrie (OQ-12/M2: Fach-Innenmaße akzeptieren
185×155×125 mm-Außenmaß; Zellvolumen-Budget 1,0–1,2 l).

## 2. Funktionale Anforderungen (MUSS)

| ID | Anforderung | Bezug SYS |
|----|-------------|-----------|
| K2Z-F01 | Nennspannung **9,6 V** (3×3,2 V), Entladebereich **7,5–10,95 V** (2,5–3,65 V/Zelle) | F-01 |
| K2Z-F02 | Pack-Nennkapazität **≥ 31 Ah** (Referenzbetrieb BA40 @ 43 mA, 3 Wochen, 0,7-Nutzfaktor) | N-01 |
| K2Z-F03 | Liefert die Effektivlast 30–60 mA **und** Lastpulse (Impuls-/Pulsmuster gemäß F-02/F-07-Reihenmessung) | F-02 |
| K2Z-F04 | Geringe **Selbstentladung**: **≤ 5 %/Monat**, Mindestladezustand ≥ 70 % nach ≥ 6 Monaten Lagersaison | N-04 |
| K2Z-F05 | **Zyklenfestigkeit ≥ 2000 Zyklen @ 80 % DoD** (Abschaltgrenze 2,5 V/Zelle ⇔ DoD ≈ 80 %) | N-07, S-01 |
| K2Z-F06 | Abschaltgrenze der Entladung **≤ 2,5 V/Zelle** (7,5 V Pack unter Last) — nicht unterschreiten | S-01 |
| K2Z-F07 | Ladung: CV **3,65 V/Zelle**, CC/CV-tauglich laut K1-Ladeprofil | N-08, S-02 |

## 3. Leistungs-/Umweltanforderungen

| ID | Kriterium | Anforderung | Bezug |
|----|-----------|-------------|-------|
| K2Z-N01 | Betriebstemperatur **Entladen** | **−10…+40 °C** | N-02 |
| K2Z-N02 | Betriebstemperatur **Laden** | **+0…+40 °C** | N-02 |
| K2Z-N03 | Packnennenergie | ≈ 298 Wh (31 Ah × 9,6 V) | N-01 |
| K2Z-N04 | Zellblock-Volumen | ≈ **1,0–1,2 l** (im Innenvolumen-Pack 3,0–3,2 l) | F-04 |
| K2Z-N05 | Zellblock-Gewicht | ≈ **2,8–3,3 kg** (LiFePO4 ~90–110 Wh/kg) | N-01, docs/03 §5 |
| K2Z-N06 | Einzelzell-Toleranz | Zellspannung je Serienposition innerhalb BMS-Messbereich; Balancierungs-Anschlüsse (Mittelanzapfungen) vorgesehen | PE-09 |
| K2Z-N07 | Beständigkeit | Tiefentlade-Toleranz bis Abschaltgrenze; kein gefährliches Verhalten bei Missbrauch (LiFePO4 intrinsisch, EN 62133-Zelltest) | S-08, OQ-11 |
| K2Z-N08 | Transportklasse | **UN3480** (Li-Ionen) / ggf. UN3481 bei integriertem Equipment | OQ-11 |

## 4. Schnittstellen innerhalb K2

| Anschluss | Bedeutung |
|-----------|-----------|
| Pack-Plus/Minus (Entlade) | Leistungspfad zum BMS-Lastschalter (PE-06), PF-06 |
| Pack-Plus/Minus (Lade) | Ladezuufluss über PE-02/PE-03 (PF-08) |
| Zellabgriffe 1/2/3 (Mittelanzapfungen) | Zellspannungsmessung + Balancierung zum BMS (PF-05) |
| Wärmeanbindung | Montageflächen für NTC-Sensoren (PE-10) |

## 5. Auswahl-/Bewertungskriterien (Komponentenauswahl)

| # | Kriterium | Referenzwert | Beurteilung |
|---|-----------|--------------|-------------|
| 1 | Kapazität/Zelle bzw. Bündel | Summe je Pfad ≥ 31 Ah (redundant-seriell abgesichert) | Datenblatt + Entladekurve 25 °C |
| 2 | Zyklenfestigkeit | ≥ 2000 @ 80 % DoD | Datenblatt / Herstellerangabe |
| 3 | Selbstentladung | ≤ 5 %/Monat | Datenblatt, Messung |
| 4 | Energiedichte | ~90–110 Wh/kg → Zielblock 2,8–3,3 kg | Pack-Volumen/Test |
| 5 | Temperaturfenster | −10…+40 °C Entladen, +0…+40 °C Laden | Datenblatt + Kälte-Derating |
| 6 | Formfaktor | Zylinder 26 mm oder Prismatik/Pouch; Bündelgeometrie im bestätigten Fach (M2) | CAD/Passtest |
| 7 | Abschaltverhalten | 2,5 V/Zelle stabil erreichbar; Feldschutz über BMS | Datenblatt + Messung |
| 8 | Zertifikate | EN 62133 (Zelltest), UN3480, CE | Zulassungsdokumente |
| 9 | Preis | Ziel-BOM je kWh → Gesamtpaket ≤ 120 EUR (OQ-07) | Stückkosten |
| 10 | Liefer-/Verfügbarkeit | langfristig (≥ 6 Saisons, ggf. 2. Quelle) | Angebot |

## 6. Normen & Zertifizierung (offen bis OQ-11)

- EN 62133-Test der Zelle/des Blocks; UN3480-Transportprüfung; CE.
- Ggf. zusätzliche Abuse-Prüfung (Nadel-/Crushtest etc.) nach OQ-11.

## 7. Offene Punkte

| Punkt | Status |
|-------|--------|
| Zell-Formfaktor/Topologie-Detail (Zylinder vs. Prismatik; Bündelzahl) | Phase-P-Auswahl anhand der Kriterien §5 |
| Konkreter Zell-Hersteller/BOM | nach Spezifikation + Angebot |
| OQ-11 Zellabuse-/Transportnorm | offen (vor Zertifizierung) |

## 8. Traceability & Status

- SYS-Trace: F-01, N-01, N-02, N-04, N-07, S-01 (SYS-Ebene geschlossen via docs/03;
  M1/M2-Spannungsfenster- und Fach-Bestätigung 2026-09-13 liegen vor).
- Komponenten-Spezifikation K2-Zellen → Grundlage der **Zellauswahl** (kein SWE-Folgeprojekt).