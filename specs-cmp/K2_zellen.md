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

## 5.1 Auswahl-Kandidat: VariCore 3,2 V / 32 Ah LiFePO4 (prismatisch) — Stand 2026-09-13

| Kennwert | Wert | Bezug Ziel / Bewertung |
|----------|------|-------------------------|
| Hersteller/Modell | VariCore LiFePO4 (prismatisch), 3,2 V Nenn, 32 Ah | Kandidat nach Spezifikation K2Z |
| Topologie | **3S1P** (3 Zellen, 1 parallel) | Entscheidung Nutzer 2026-09-13: 3 Zellen à 32 Ah |
| Pack-Kapazität | **32 Ah** (≥ 31 Ah Ziel) | K2Z-F02 ✓ (Redundanz ≈ +3 %) |
| Pack-Nennenergie | **307,2 Wh** (3 × 102,4 Wh) | ≥ 298 Wh Ziel (K2Z-N03) ✓ |
| Spannungsfenster | 2,5–3,65 V/Zelle ⇒ 7,5–10,95 V Pack | F-01 / K2Z-F01 ✓ |
| Preis (Zellanteil) | 6 Zellen = 85,23 € ⇒ **14,21 €/Zelle; 3 Zellen ≈ 42,63 €** | OQ-07: sehr gut im Budget (K2Z-N08-Kostenkriterium) |
| Maße (Klasse, verifiziert) | je Quelle **Variante A: 142 × 100 × 21 mm** bzw. **Variante B: 145 × 100 × 22 mm** (22/20 mm je Anbieter), vorwiegend M6-Anschluss | **Achtung: zwei abweichende Maßangaben im Umlauf** → an konkretem Stück bestätigen |
| Gewicht (Klasse) | ~0,63–0,64 kg ⇒ **Block ≈ 1,9 kg** (≈ 162 Wh/kg) | K2Z-N05 günstiger als Ziel 2,8–3,3 kg (mehr Reserve), passt ✓ |
| Spannungsfenster (Zelle) | Ladeschluss **3,65 V**, Entladeabschluss **2,0 V** (min., T>0 °C) | F-01 ✓; **2,0 V nur Zellwert — Abschaltung des Packs übernimmt BMS bei ≤ 2,5 V (S-01)** |
| Laden | CC/CV, max. **1 C (32 A)**, empfohlen ~0,5 C | N-08/S-02 ✓ (K1 mit ≥ 30 W ≈ 0,1 C weit darunter) |
| Entladen | max. ~2–3 C; Dauerentladung großzügig über Lastbedarf | F-02 ✓ |
| Zyklenfestigkeit | **angewzeigt ~2000 Vollladungszyklen** (Anbieter-Angaben, „≥8000")
      vs. **„≥ 2.000" (Langzeit-Anbieter)** | **N-07 (≥ 2000 @ 80 % DoD): knapp erfüllbar — 80 %-DoD gibt Reserve vs. 100 %-Zyklen; exakt zu verifizieren** |
| Temperaturfenster (Zelle) | Laden +0…+45 °C; Entladen −20…+60 °C (Variante 30 Ah: −20…+60 °C) | K2Z-N01/N02 ✓ (Entladen −10…+40, Laden +0…+40 inkludiert) |
| Selbstentladung | LiFePO4-typisch niedrig (~3–5 %/Monat, Messung offen) | N-04: Messung erforderlich |
| Innenwiderstand | gemessen ~3 mΩ (Anbieter) | K2B-N03-Referenz, PF-Verluste minimal ✓ |
| Offen (Datenblatt erforderlich) | EN 62133/UN3480/CE-Zertifikate; definitive Maße & Zyklenangabe des gelieferten Stücks | → Bewertung §5 Punkte 2–8 abschließen |

**Vorbehalt:** Es kursieren **zwei Maß-/Zyklen-Varianten** derselben 32-Ah-Zelle
(Variante A: 142×100×21 mm, „≥8000 Zyklen"; Variante B: 145×100×22 mm, „~2000 Zyklen",
~631 g). Beide passen volumenmäßig in das Zell-Budget (je Zelle ≈ 0,30 l ⇒ Block
≈ 0,9 l < Budget 1,0–1,2 l). Das **konkret gelieferte Stück** (Maße, Gewicht,
Zyklenangabe, Zertifikate) ist beim Kauf/Angebot zu fixieren (CE-CAD- und Passtest M2).

**Prüfpunkte vor Freigabe:**
1. **Maße/Form:** gelieferte Zelle = Variante A oder B; Passtest M2 (Fach 185×155×125 mm).
2. **Zyklenangabe:** „≥8000" vs. „~2000" klären — N-07 (≥2000 @ 80 % DoD) braucht die
   **reale** Angabe des Anbieters; Kälte-/Lewes-Datenblatt anfordern.
3. **Zertifikate:** EN 62133, UN3480, CE beim Anbieter erfragen (OQ-11).
4. **Abschaltkopplung:** Zellwert 2,0 V ≠ Systemabschaltung 2,5 V (S-01) → BMS-Abschaltung
   ist die verbindliche Grenze (Reihenprüfung mit BMS).

**Nächster Schritt:** Angebot/Beleg des konkreten VariCore-32-Ah-Stücks anfordern
(Maße, Gewicht, Zyklen, Zertifikate) und gegen §5 (Punkte 1–8 & 10) bewerten;
Blatt in `build/datenblatt/` ablegen.

## 6. Normen & Zertifizierung (offen bis OQ-11)

- EN 62133-Test der Zelle/des Blocks; UN3480-Transportprüfung; CE.
- Ggf. zusätzliche Abuse-Prüfung (Nadel-/Crushtest etc.) nach OQ-11.

## 7. Offene Punkte

| Punkt | Status |
|-------|--------|
| Variante A vs. B (Maße/Zyklen) — konkretes Stückfixieren | vor Kauf/Angebot zu klären |
| Zyklenangabe: „≥8000" (Marketing) vs. „~2000" (Realisierbar) — N-07 80 %-DoD-Reserve prüfen | Bestätigung durch Anbieter erforderlich |
| EN 62133/UN3480/CE-Zertifikate | offen (OQ-11, vor Serienzertifizierung) |

## 8. Traceability & Status

- SYS-Trace: F-01, N-01, N-02, N-04, N-07, S-01 (SYS-Ebene geschlossen via docs/03;
  M1/M2-Spannungsfenster- und Fach-Bestätigung 2026-09-13 liegen vor).
- Komponenten-Spezifikation K2-Zellen → Grundlage der **Zellauswahl** (kein SWE-Folgeprojekt).