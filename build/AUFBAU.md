# Umsetzung · Aufbau (build)

| | |
|---|---|
| Verzeichnis | `build/` (Umsetzung — neben `docs/` und `specs-cmp/`) |
| Zweck | Laufende Doku zum physischen Aufbau des Akku-Packs K2 sowie der Folge-Packs |
| Stand | 2026-09-13 (angelegt) |
| Bezug | `specs-cmp/K2_zellen.md`, `specs-cmp/K2_bms.md`; Maße B 185 × H 155 × T 125 mm |

## Arbeitsschritte (geplant)

1. **Komponenten-Beschaffung** — VariCore 3,2 V/32 Ah (3 Stück) + Lisolec 3-S-BMS (2 Stück).
2. **Eingangsprüfung** — Maße/Gewicht, Datenblatt-Abgleich, Abmessungsdoku (siehe `datenblatt/`).
   Bei BMS zusätzlich **Schwellen-Messung** (Überentlade-/Überladeschwelle, Balance-Anlauf,
   NTC-Pad) gegen K2B-Spez — AliExpress liefert die Spec nur als Bild.
3. **Form-/Passprüfung** — Drop-in in das Batteriefach des Zielgeräts (Reihenmessung M2, docs/06).
4. **Verdrahtung** — Umsetzung gemäß `VERDRAHTUNG.md`.
5. **Funktionsprobe** — 3-S-Spannungsprofil, BMS-Schwellen, Lade-/Entladeverhalten.

## Aufbau-Details

> Wird beim physischen Zusammenbau (Schritt 4–5) fortgeschrieben.

## Control Records

- (leer — bei jedem Bau-Schritt inkl. Datum und Messwerten ergänzen)

## Tracker

- [ ] Beschaffung
- [ ] Eingangsprüfung
- [ ] Formpassung
- [ ] Verdrahtung
- [ ] Funktionsprobe