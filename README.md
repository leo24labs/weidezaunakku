# Weidezaun-Akku-Pack (weidezaunakku)

Wiederaufladbarer **9-V-/9,6-V-LiFePO4-Akku** als **Drop-in-Ersatz** für 9-V-Trocken-
batterien (Zink-Kohle/Alkaline) in bestehenden batteriebetriebenen Weidezaungeräten
der 9-V-Klasse (z. B. Gallagher BA-Serie, Koltec, AKO).

## Motivation

- 9-V-Trockenbatterien in Weidezaungeräten sind teuer, schwach und landen regelmäßig
  im Müll.
- Ein **wiederaufladbarer Akku** spart Kosten über viele Saisons, reduziert Abfall und
  liefert über den Entladeverlauf mehr nutzbare Energie als Primärbatterien.
- Das Pack ist ein **reiner Ersatz**: Es muss ohne Umbau in vorhandene Geräte passen
  (**Drop-in**) und dieselbe elektrische Arbeit liefern.

## Ziel

Ein Akku-Pack, das in ein bestehendes Weidezaungerät eingesetzt wird und es über die
Weidesaison zuverlässig versorgt:

| Kenngröße | Zielwert |
|-----------|----------|
| Bauform | Drop-in in das Batteriefach (Max. 185 × 155 × 125 mm) |
| Spannung | 9,6 V nominal (7,5–10,95 V, 3S-LiFePO4) |
| Kapazität | ≥ 31 Ah (≈ 3 Wochen Betrieb ohne Laden) |
| Ladezeit | ≤ 12 h (Ladegerät ≥ 30 W) |
| Lebensdauer | ≥ 6 Saisons, ≥ 2000 Zyklen bei 80 % Entladetiefe |
| Preis | Verkauf ≤ 120 EUR |
| Schutz | IPx4, Temperatur-/Tiefentlade-/Überlade-/Kurzschlussschutz |

## Projektstand

Stand **2026-09-13**. Entwicklung nach **ASPICE-SYS-Ebene** (Requirements → logische →
physische Architektur). Zwei Kern-Messungen am realen Gerät sind **bestätigt**:

- **M1:** Das 9,6-V-3S-Spannungsprofil funktioniert am echten Weidezaungerät (F-01 bestätigt).
- **M2:** Die Pack-Umrisse (≤ 185 × 155 × 125 mm) passen ins echte Batteriefach (Drop-in bestätigt).

Stand der Komponenten-Auswahl: **Zellen (VariCore 32 Ah LiFePO4, 3 Stück)** und
**BMS (Lisolec 3-S LiFePO4, 15 A/7 A)** sind als Kandidaten dokumentiert und weitgehend
verifiziert; Beschaffung/Umsetzung sind vorbereitet, Aufbau und Eingangsmessung stehen noch aus.

> **Kein SWE-Projekt:** Dieses Projekt führt die SWE-Phase (Software-Engineering nach
> ASPICE SWE.1…SWE.5) bewusst **nicht** durch. Die Komponenten werden **nach
> Spezifikation ausgewählt** (Hardware; Kapazitäts-/Schutzaufgaben übernehmen Zelle + BMS).

## Entwicklungsschritte & Dokumente

| Schritt | Dokument | Status |
|---------|----------|--------|
| Markt-/Stakeholder-Analyse | `docs/00_marktanalyse.md` | abgeschlossen |
| Anforderungen (SYS.2, 23 Stück: F7/N8/S8) | `docs/01_spezifikation_anforderungen.md` | abgeschlossen, Teilentscheidungen fixiert |
| Logische Architektur (SYS.3-Phase L, 13 Elemente) | `docs/02_spezifikation_architektur_logisch.md` | abgeschlossen |
| Physische Architektur (SYS.3-Phase P, 12 Elemente, K1/K2/K3) | `docs/03_spezifikation_architektur_physisch.md` | abgeschlossen (Referenz LiFePO4 3S) |
| Bestätigungs-Messungen (M1, M2) | `docs/06_milestones_messungen.md` | **bestätigt 2026-09-13** |
| Komponenten-Spezifikationen (Auswahl) | `specs-cmp/K1_ladegeraet.md`, `K2_zellen.md`, `K2_bms.md`, `K3_solarmodul.md` | K2 (Zellen/BMS) weitgehend verifiziert; K1/K3 ausgestellt |
| **Umsetzung** (Aufbau, Verdrahtung, Pack-Datenblatt) | `build/AUFBAU.md`, `VERDRAHTUNG.md`, `DATENBLATT_AKKU.md` | vorbereitet |

Die aktuelle Planung ist in drei Komponenten gegliedert:

- **K1 — Ladegerät** (extern, mit integriertem Laderegler CC/CV): *aktuell ausgestellt.*
- **K2 — Akku-Pack** (Zellen + BMS): *aktiver Fokus.*
- **K3 — Solar-Panel** (externes Zubehör): *aktuell ausgestellt.*

## Aufbau (Kurzfassung)

Der Akku besteht aus:

- **3 Zellen** VariCore 3,2 V / 32 Ah LiFePO4, in Serie geschaltet (**3S1P** →
  9,6 V / 32 Ah / ≈ 307 Wh),
- **1 BMS** Lisolec 3-S LiFePO4 (15 A Entladen / 7 A Laden) mit Schutzfunktionen
  (Tiefentlade ≤ 2,5 V/Zelle, Überlade ~3,65 V/Zelle, Überstrom, Kurzschluss, Balance),
- **Sicherung F1 (5 A)** in der Plus-Leitung (P+) als zusätzlicher Pfadschutz — Details
  in `build/DATENBLATT_AKKU.md` (§2/§4) und `build/VERDRAHTUNG.md`.

Die konkrete Verdrahtung (Zellabgriffe B−/B1/B2/B3, Anschluss P+/P−, Sicherung F1 5 A
in P+) ist unter `build/VERDRAHTUNG.md` dokumentiert; Arbeitsschritte und Eingangsmessung
unter `build/AUFBAU.md`. Das **Datenblatt des fertigen Akkus** (elektrische/thermische/
mechanische Kenndaten, Schutzfunktionen, Messprotokoll) ist `build/DATENBLATT_AKKU.md`.

## Nutzungs-Anleitung (kurz)

**Einsetzen:**
1. Sicherstellen, dass das Weidezaungerät ausgeschaltet bzw. spannungsfrei ist.
2. Akku-Pack wie eine 9-V-Batterie einlegen — polungsrichtig (P+/+ und P−/− an den
   Geräteklemmen), bis er einrastet. Das Pack ist als Drop-in gebaut und passt in das
   Standard-Batteriefach.
3. Gerät einschalten; die Zaunimpulse werden normal erzeugt (9,6-V-Profil).

**Temperaturgrenzen:**
- **Entladen (Betrieb): nicht unter −10 °C** nutzen (kälter ⇒ Leistungs-/Kapazitäts-
  verlust, Schädigung möglich).
- **Laden: nicht unter 0 °C** laden (Kaltladen schädigt LiFePO4-Zellen).
- Obergrenze jeweils +40 °C (Laden) bzw. +40 °C (Betrieb).

**Laden:**
- Nur mit dem **vorgesehenen Ladegerät (K1)** bzw. einer 3S-LiFePO4-konformen Ladung
  (CC/CV, **Ladeschluss 10,95 V**) laden.
- **Nicht** mit Ladegeräten für 9-V-Block-Batterien oder Blei-Akkus laden!
- Ladezeit bei ≥ 30 W: laut Ziel **≤ 12 h**.

**Entnehmen / Lagern:**
- Bei längerer Lagerung (z. B. über den Winter) den Akku vorher **laden** (≥ 70 %)
  und **getrennt vom Gerät** trocken bei Raumtemperatur lagern (Selbstentladung
  ≤ 5 %/Monat, LiFePO4-schonend).
- Vor Wiedereinsatz im Frühjahr kurz nachladen.

**Sicherheit:**
- Der Akku ist durch das BMS geschützt (Tiefentlade, Überlade, Überstrom, Kurzschluss).
- **Nicht** öffnen, kurzschließen, beschädigen oder in Feuer werfen.
- Bei Beschädigung oder Aufblähung nicht weiterverwenden; fachgerecht entsorgen
  (Altbatterie/Lithium – UN3480-Transportklasse beachten).

## Mitmachen

Feedback gerne an **leo24labs@proton.me**.

Bei der Umsetzung eines eigenen Akkus nach dem hier veröffentlichten Vorgabe unterstütze ich gerne.

## Repo

<https://github.com/leo24labs/weidezaunakku>

![QR-Code zum Repo](build/img/qr-repo.png)