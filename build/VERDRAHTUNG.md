# Umsetzung · Verdrahtung (build)

| | |
|---|---|
| Verzeichnis | `build/` |
| Zweck | Verdrahtungsplan und Anschluss-Doku des Akku-Packs K2 (3S1P) |
| Stand | 2026-09-13 (angelegt) |
| Bezug | `docs/03_spezifikation_architektur_physisch.md` (PE-01…PE-12, PF-01…PF-13), `specs-cmp/K2_zellen.md`, `specs-cmp/K2_bms.md` |

## Topologie

- **3S1P**: 3 × VariCore 3,2 V / 32 Ah LiFePO4 in Serie: 9,6 V Nenn, 7,5–10,95 V Pack.

## Verdrahtungsplan (Referenz)

```
B+ ──► BMS (B+)              Last + ──► PE-07 (Klemme +)
B1 ──► BMS (B1, Mittel)      Last − ──► BMS P− ──► PE-07 (Klemme −)
B2 ──► BMS (B2, Mittel)      Lade in: PE-02 (Buchse) → Laderegel-/Schutzpfad
B− ──► BMS (B−)
                                              PE-10 (NTC) → BMS / eigene Auswertung
```

> Anschlussreihenfolge und Spannungsanker je Serienposition beim Verdrahten messen
> (3-S-Spannungsprofil M1 gemäß docs/06).

## Querschnitte / Kontakte

> Beim Zusammenbau festlegen und hier dokumentieren (Zell-Verschraubung/-Verbindung,
> BMS-Balancer-Anschlüsse, Last-/Ladekabel).

## Sicherheit

- Verpolungsschutz: PE-02-Eingangsschutz + mech. Klemmen-Sicherung (S-04).
- Lastschalter PE-06 (BMS-P−) im Normalbetrieb einlassend, im Fehlerfall trennend.

## Control Records

- (leer — bei Verdrahtung inkl. Datum und Messwerten ergänzen)

## Tracker

- [ ] Verdrahtungsplan final
- [ ] Verdrahtung durchgeführt
- [ ] Durchgangs-/Polprüfung
- [ ] Funktionsprobe