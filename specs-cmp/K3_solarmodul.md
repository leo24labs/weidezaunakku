# Komponenten-Spezifikation K3 · Solarmodul (optional, extern)

| | |
|---|---|
| Komponente | K3 — Solar-Panel (externes Zubehör) + Solar-Eingang PE-05 im Ladegerät K1 |
| Zweck | Komponenten-Auswahl nach Spezifikation (SYS-Ebene; **keine SWE-Phase**) |
| Stand | 2026-09-13 |
| Status | Optionale Variante; Spezifikation bereit zur Panelauswahl; **OQ-06 (Panel-Kennwerte) offen** |
| Bezugsdokumente | `../docs/01_spezifikation_anforderungen.md` (F-06) · `../docs/03_spezifikation_architektur_physisch.md` (PE-05, PE-04, PF-04 · Abschnitt 1.4) |

## 1. Einordnung

Das Solarmodul ist die **optionale** K3-Variante: ein externes Solar-Panel, dessen
Energie über den **Solar-Eingang PE-05 am externen Ladegerät K1** dem MPPT-fähigen
Laderegelpfad (PE-04) zugeführt wird (Konstruktionsorts-Entscheidung 2026-09-13:
Laderegler und Solar-Eingang sitzen im Ladegerät, nicht im Pack). Der
**Kompatibilitätsnachweis (F-06)** ist verpflichtend: Ertrag über die Ladesaison,
kein Fehlabbruch bei Teillast, keine Überladung.

## 2. Funktionale Anforderungen (MUSS)

| ID | Anforderung | Bezug SYS |
|----|-------------|-----------|
| K3-F01 | Panel liefert über die Einstrahlungsperiode ausreichend Energie, um die ≥ 31-Ah-/298-Wh-Batterie in der Zielzeit (≤ 12 h, N-08) bzw. saisonal nachzuliefern | F-06, N-08 |
| K3-F02 | Liefert auch bei **schwankender Einstrahlung/Teillast** weiter Energie — **kein Fehlabbruch**, **kein Überladen** (MPPT-Pfad in K1-PE-04) | F-06 |
| K3-F03 | **Kompatibilitätsnachweis (F-06)** zur gesamten Ladekette (Panel ↔ PE-05 ↔ PE-04 ↔ Zellblock) wird mitgeliefert | F-06 |
| K3-F04 | **Temperatur-/Einstrahlungsverhalten** am Einsatzort (Deutschland, Feld/Wiese, ganzjährige Nutzung) ist spezifiziert | N-02, F-06 |

## 3. Leistungs-/Umweltanforderungen

| ID | Kriterium | Anforderung | Bezug |
|----|-----------|-------------|-------|
| K3-N01 | Panel-Spannungsklasse | **nach OQ-06** (Referenz: 12-V-Panelklasse, MPP-Bereich des K1-Ladereglers im Fenster) | OQ-06 |
| K3-N02 | Panel-Leistungsklasse | **nach OQ-06** (Referenz: ~10–20 W für saisonale Nachladung) | OQ-06, N-08 |
| K3-N03 | Schutzart | mind. **IPx4** (Panel + Anschluss), Kabel/Kabeldurchführung entsprechend | N-03 |
| K3-N04 | Betriebstemperatur | −10…+40 °C (Umfeld; Panel-Modul gemäß Datenblatt) | N-02 |
| K3-N05 | Anschluss | Stecker/Kabel kompatibel zur Solarbuchse PE-05 am Ladegerät K1 (PF-04) | PF-04 |
| K3-N06 | Kosten-Aufpreis | Teil des Varianten-BOM („mit/ohne Solar", OQ-07), Gesamtbudget ≤ 120 EUR wird geprüft | OQ-07 |

## 4. Schnittstellen

| Kante | Von → Nach | PF-Bezug | Charakteristik |
|-------|------------|----------|----------------|
| Solar-Einspeisung | Panel K3 → K1 (PE-05 → PE-04) | PF-04 | Panel-Spannung/-Strom, MPPT-Regelung |
| Ladung ins Pack | K1 (PE-04) → K2 (PE-02) | PF-01 | geregelte SELV-Ladespannung wie Netz-Ladung |

## 5. Auswahl-/Bewertungskriterien (Komponentenauswahl)

| # | Kriterium | Referenzwert | Beurteilung |
|---|-----------|--------------|-------------|
| 1 | Panel-Spannungs-/Leistungsfenster | OQ-06-Klasse | Datenblatt |
| 2 | MPPT-Kompatibilität mit K1-PE-04 | MPP-Bereich im Ladereglermessbereich | Abstimmung K1-Parametrierung |
| 3 | Ertrag/Saisondeckelung | Deckt Entladebedarf (Energiebilanz) | Wirkungsgrad-Rechnung |
| 4 | Robustheit | IPx4, mecha-n. Haltbarkeit Stall/Außen | IP-/Stabilitätsprüfung |
| 5 | Temperaturverhalten | Derating im Kälte-/Hitzebereich | Datenblatt |
| 6 | Kosten-Aufpreis | im OQ-07-Budget | Stückkosten |

## 6. Normen & Zertifizierung (offen bis OQ-11)

- CE-Konformität Panel; Ertrags-/Sicherheitsnachweise gemäß OQ-11.
- Kompatibilitätsnachweis (F-06) dokumentiert: K3 ↔ K1 ↔ K2 über die Ladezeit.

## 7. Offene Punkte

| Punkt | Status |
|-------|--------|
| OQ-06 Panel-Spannungs-/Leistungsklasse | **offen** → determiniert Panelwahl (und MPP-Fenster in PE-04) |
| Variantenentscheidung „mit/ohne Solar" | offen (Business/OQ-07) |

## 8. Traceability & Status

- SYS-Trace: F-06 (Kompatibilitätsnachweis Pflicht), N-08 (Ladezeit), indirekt N-02/N-03.
- Komponenten-Spezifikation K3 → Grundlage der **Panelauswahl** nach OQ-06-Freigabe (kein SWE-Folgeprojekt).