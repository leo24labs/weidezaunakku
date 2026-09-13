# Milestones: Blockierende Messungen (Bestätigungs-/Verifikationsbasis)
# Stand 2026-09-13: Technologieentscheidung GEFALLEN (LiFePO4 3S), Pack-Maximalmaße fixiert, M1+M2 BESTÄTIGT

## Einordnung

Die Technologieentscheidung (LiFePO4 3S, docs/01 §8.1) und die Pack-Maximalmaße
(185 × 155 × 125 mm, 3,58 l) sind seit 2026-09-13 **entschieden**. Beide
Bestätigungsmessungen am realen Gerät sind **erfolgt und bestätigen das Design**
(2026-09-13); Serien-Fixierung auf dieser Basis freigegeben:

- **M1 (OQ-05):** 3S-Spannungsprofil am realen Referenzgerät **bestätigt** (F-01).
- **M2 (OQ-12):** Formpassung des Packs (≤ 185 × 155 × 125 mm) im echten
  Batteriefach **bestätigt** (F-04 = striktes Drop-in); Volumen-Budget für den
  Zellblock (1,0–1,2 l, 31 Ah) erfüllt.

Sie sind als OQ-Punkte in `docs/01_spezifikation_anforderungen.md` (§7.2) und
`docs/03_spezifikation_architektur_physisch.md` (R-1, R-2) verankert.

---

## M1 — OQ-05: Spannungsmessung am realen Weidezaungerät

**Bestätigt:** F-01 — das festgelegte 3S-Profil (7,5–10,95 V, nominal 9,6 V) am
realen Referenzgerät (R-2 in docs/03; Technologie bereits entschieden LiFePO4 3S)

**Ziel:** Das reale Eingangsspannungsfenster der Referenzgeräte messen und
bestätigen, dass mit dem LiFePO4-3S-Profil störungsfrei gearbeitet wird.

**Referenzgeräte (mindestens eines, ideal alle drei):**
- Gallagher BA30 (34 mA)
- Gallagher BA40 (43 mA)
- Koltec EC20 (30 mA)

**Vorgehen:**
1. Eingangsspannung mit geregelter Netzgerätequelle in 0,5-V-Schritten von
   12,0 V bis 5,5 V abwärts fahren; dabei je Stufe: Zaunimpuls-Frequenz und
   -Spannung messen (Zaunprüfer/Oszilloskop an Zaunanschluss).
2. Abschaltgrenze (letzte zuverlässige Impulserzeugung) und Verhalten bei
   Unterspannung (Warn-LED, Sparmodus) protokollieren.
3. Wiederholung mit Batterie-Emulation: Spannungsabfall unter Pulslast
   (Oszilloskop, 10 ms-Auflösung) gegen Geräteschwelle prüfen.

**Akzeptanzkriterium (Bestätigung LiFePO4-3S):**
- Gerät erzeugt im Bereich 7,5–10,95 V ununterbrochen Pulse; Abschaltgrenze
  ≥ 7,0 V → 3S-Profil bestätigt, F-01-Freigabe.
- Messung widerspricht dem festgelegten Profil (z. B. Abschaltgrenze deutlich
  über 8 V) → Abweichungsfall: M1-Ergebnis zurück in die SYS.2/SYS.3-Review,
  Anpassung des Spannungsziels (R-2).

**Ergebnis (2026-09-13):** Messung am Referenzgerät **abgeschlossen**; das
3S-Profil (7,5–10,95 V, nominal 9,6 V) wurde **bestätigt** → F-01-Zielwert fixiert.
Kein Abweichungsfall ausgelöst.

---

## M2 — OQ-12: Fachgeometrie-Vermessung der Batteriefächer

**Blockiert:** F-04 (strikt Drop-in), Zelltopologie 31 Ah, Gehäusedimensionierung

**Status (2026-09-13):** Pack-Obergrenze vom Auftraggeber **gesetzt**:
B × H × T = **185 × 155 × 125 mm → 3,58 l** Gesamtvolumen (Innenvolumen
≈ 3,0–3,2 l). Damit ist die Machbarkeit von 31 Ah-LiFePO4 (Zellbedarf
≈ 1,0–1,2 l) volumenmäßig gegeben; offen bleibt die **Bestätigung**, dass das
Pack in das reale Fach eines Referenzgeräts passt (formschlüssig, Verriegelung,
Deckelfreiheit).

**Ziel:** Innenmaße, Anschlussterminals, Verriegelung und Deckelfreiheit der
Batteriefächer der Referenzgeräte aufnehmen und prüfen, ob das Pack
(≤ 185 × 155 × 125 mm) darin formschlüssig unterzubringen ist.

**Vorgehen (je Referenzgerät):**
1. Innenmaße L×B×H des Fachs (Schieblehre/Maßband, 0,5 mm).
2. Position/Geometrie der Batterieklemmen und Polungsfeld aufnehmen (Foto + 
   Maßzeichnung).
3. Verriegelung/Deckelschließung prüfen (max. Höhe bei geschlossenem Deckel).
4. Pack passen (max. 185 × 155 × 125 mm), sofern ein Prototyp/Phantom vorliegt;
   andernfalls Maße des Faches gegen die Pack-Vorgabe prüfen.
5. Vergleich gegen Pack-Vorgabe: Passt 185 × 155 × 125 mm formschlüssig hinein?

**Akzeptanzkriterium:**
- Fachvolumen bzw. -innenmaße ≥ Pack-Außenmaße → Drop-in bestätigt, Vorgabe
  185 × 155 × 125 mm trägt; Zellvolumen-Budget für 31 Ah-LiFePO4 (1,0–1,2 l)
  ist gegeben.
- Fach nimmt 185 × 155 × 125 mm nicht vollständig auf → Rückmeldung an
  Auftraggeber; ggf. Zielkosten-/Kapazität-Neuabstimmung oder Pack-Anpassung.

**Ergebnis (2026-09-13):** Fach-Vermessung **abgeschlossen**; das Pack
(≤ 185 × 155 × 125 mm) passt **formschlüssig** in das Batteriefach des
Referenzgeräts → Drop-in (F-04) bestätigt; Zellvolumen-Budget (1,0–1,2 l, 31 Ah)
gegeben. Fließt in die Gehäuse-Fixierung (PE-11, docs/03) ein.

---

## Reihenfolge / Abhängigkeiten

1. Technologie-Entscheidung (LiFePO4 3S) und Pack-Maximalmaße
   (185 × 155 × 125 mm) sind seit 2026-09-13 **fixiert**.
2. M2 (Formpassung) **erfolgt (2026-09-13)**: Drop-in-Zielgerät und Formpassung
   (≤ 3,58 l) im realen Batteriefach **bestätigt**.
3. M1 (Spannung) **erfolgt (2026-09-13)**: 3S-Spannungsprofil (F-01) **bestätigt**.
→ Beide Bestätigungsbasis erledigt; Serien-Fixierung vorbereitet.

## Eigentümer / Status

| Milestone | Status | Eigentümer | Nächster Schritt |
|-----------|--------|-----------|------------------|
| M1 OQ-05 | **bestätigt (2026-09-13)** | Nutzer (Messung) | Messprotokoll liegt vor; Ergebnis in docs/01/docs/03 übertragen |
| M2 OQ-12 | **bestätigt (2026-09-13)** | Nutzer (Messung) | Vermessungsbericht liegt vor; Ergebnis in docs/01/docs/03 übertragen |