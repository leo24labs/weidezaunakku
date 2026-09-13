# Logische Systemarchitektur (ASPICE SYS.3, Phase L)
## Wiederaufladbares 9V-Akku-Pack für Weidezaungeräte

| | |
|---|---|
| Prozess | SYS.3 – Systemarchitektur, **Phase L (logisch, technologie-neutral)** |
| Eingangsartefakt | `01_spezifikation_anforderungen.md` (SYS.2, System-Requirements) |
| Ausgangsartefakt | Diese Spezifikation (logische Architektur) |
| Nachfolgeprozess | Phase P (physische Architektur, SYS.3) → SYS.4 (Integration/Verifikation) |
| Elementtypen (SYS.3) | Hardware, Software (Schutz-/Regelungslogik), Mechanik |
| Status | Phase L abgeschlossen; keine Technologie-, Bauteil-, Hardware- oder Implementierungsentscheidungen |
| Stand | 2026-09-13 |

> **Phasengrenze (verbindlich):** Dieses Artefakt enthält ausschließlich logische
> Elemente, Funktionen, Schnittstellen und Verhalten. Alle technologischen,
> physischen und implementierungsspezifischen Entscheidungen fallen in **Phase P**
> (LiFePO4-3S ist dort seit 2026-09-13 **entschieden** — siehe docs/03 und docs/01 §8.1;
> dieses Dokument bleibt bewusst technologie-neutral).

---

## 1. Zweck & Vorgehen

- Ableitung logischer Elemente aus den 23 Systemanforderungen (F 7 / N 8 / S 8),
  ohne Bezug auf Zieltechnologie.
- Definition statischer Schnittstellen (Element ↔ Element, Element ↔ Außenwelt).
- Definition dynamischen Verhaltens (Zustände, Abläufe) rein logisch.
- Vollständige Traceability Anforderung → logisches Element.
- Konsistenzprüfung gegen die Systemanforderungen und in sich.
- Uneindeutige Zuordnungen bleiben als **offene Frage** markiert (Abschnitt 8),
  keine eigenmächtigen Annahmen.

**Außenwelt (Kontext):** 230V-Netzteil, optionales Solarmodul, das
Weidezaungerät (Referenzgerät 9V-Klasse, nicht Teil des Produkts), Anwender
(Landwirt), Umgebung (Deutschland: Witterung, Tierkontakt, Insekten-/Feldstaub),
Aufsichts-/Entsorgungsrahmen.

---

## 2. Kontextmodell

```
                          ┌────────────────────────────────────┐
   230V-Netz     ────────▶ │ LE-H2 Netz-Ladeeingang              │
                          │                                    │
   Solar (opt.)  ────────▶ │ LE-H3 Solar-Ladeeingang            │
                          │                                    │
                          │ LE-S2 Laderegelung    ◀─── LE-H1    │
                          │    │                    Energiespeicher
   Weidezaungerät◀────────│ LE-H4 Lastpfad-Schalter ◀─── LE-H5   │
                          │        Ausgangsschnittstelle        │
                          │                             ▲       │
                          │ LE-S1/S3/S4/S5 Schutzlogik  │       │
                          │ LE-H6 Störfestigkeit ◀──────┘       │
                          │ LE-M1 Gehäuse │ LE-M2 Kennzeichnung  │
                          └────────────────────────────────────┘
```

Logisch internes Verhalten bleibt technologieunabhängig: Energiespeicherung,
Laderegelung, Schutz-/Überwachungslogik, Strompfad-Trennung, mechanische
Rahmung. Die Physis (Zellen, Schutz-ICs, Controller, Bauteile, Material) ist
Phase P zugeordnet.

---

## 3. Logische Elemente

### 3.1 Hardware-Elemente (logische Funktionseinheiten)

| ID | Element | Logische Funktion |
|----|---------|-------------------|
| **LE-H1** | **Energiespeicher** | Speichert elektrische Energie und stellt über den gesamten Entladebereich eine Klemmenspannung von nominal ≈ 9 V für die Referenzgeräte bereit. Liefert den lasttypischen Effektivstrom (30–60 mA) einschließlich der pulsartigen Spitzenströme; Verhalten gegenüber Temperaturbereich (Entladen −10…+40 °C) und Selbstentladung (Lagersaison ≥ 6 Monate) ist Bestandteil der logischen Funktion. |
| **LE-H2** | **Netz-Ladeeingang** | Nimmt Energie aus dem mitgelieferten 230V-Netzteil für den Ladevorgang auf. Gewährleistet **sichere galvanische Trennung** zwischen Netzpfad und Zellseite (Isolationsbarriere); Versorgt die Laderegelung mit Energie und Status. |
| **LE-H3** | **Solar-Ladeeingang (optionale Variante)** | Nimmt Energie aus einem optionalen Solarmodul auf; verträgt das schwankende Einstrahlungs-/Temperaturprofil, ohne dass dies zu Schutzabschaltung oder Energiespeicher-Schädigung führt. |
| **LE-H4** | **Lastpfad-Schalteinrichtung** | Unterbricht bzw. begrenzt im Fehlerfall den Stromfluss zwischen Energiespeicher und Ausgang auf Kommando der Schutzlogik. Im Normalbetrieb einlassend (stromlos verschleißfrei). Sorgt für die Wiedereinschaltung nach Fehlerentfernung. |
| **LE-H5** | **Elektrische Ausgangsschnittstelle** | Stellt Klemmenspannung und Laststrom am Ausgang entsprechend dem Anschlussbild der 9V-Trockenbatterie bereit (Dropdown-Elektrik, Kontaktwiderstand ≤ 50 mΩ je Kontakt). Definiert die Polung; weist verpolte Anschaltungen ohne Schädigung zurück. |
| **LE-H6** | **Störfestigkeitsfunktion (Impuls-/EMV-Entkopplung)** | Begrenzt die Wirkung der vom Weidezaungerät erzeugten Hochspannungsimpulse/Funken an den Klemmen (kapazitiv/induktiv) auf Energiespeicher, Schutzlogik und Ladepfad. Keine Fehlauslösung, kein spezifizierter Ladezustandsverlust durch Störungen. |

### 3.2 Software-Elemente (logische Schutz-/Regelungslogik)

| ID | Element | Logische Funktion |
|----|---------|-------------------|
| **LE-S1** | **Tiefentlade-Schutzlogik** | Überwacht den Energiespeicher während der Entladung, erkennt das Erreichen der Abschaltgrenze (maximale nutzbare Entladetiefe, DoD) und kommandiert die Lasttrennung über LE-H4. Verhindert dauerhafte Zellschädigung entlang der Zyklenlebensdauer. **Zwingend (R8).** |
| **LE-S2** | **Laderegelung** | Führt den Ladeablauf in Abhängigkeit der Ladequelle (Netz LE-H2 / Solar LE-H3). Regelt Ladestrom/-spannung ladezustands- und temperaturalhängig; beendet die Ladung zuverlässig beim Ladeendekriterium (damit eigentlicher Überladeschutz). Muss bei variabler Leistung (Solar) robust gegen Fehlabschaltung/Überladung entscheiden. |
| **LE-S3** | **Kurzschluss-/Überstrom-Schutzlogik** | Erkennt Überstrom/Kurzschluss am Lastpfad und veranlasst Unterbrechung bzw. Begrenzung (LE-H4). Unterscheidet betriebsnormale, pulsartige Lastspitzen (kein Auslösen, Anforderung F-02) von persistenten Fehlerzuständen. |
| **LE-S4** | **Verpolungs-Schutzlogik** | Erkennt eine verpolte Verbindung am Geräte- bzw. Ladepfad und verhindert schädigenden Stromfluss (kein Strompfad, keine Brand-/Fehlfunktion). Nach korrekter Wiederanschaltung voll funktionsfähig, ohne irreversiblen Schaden. |
| **LE-S5** | **Thermisches Managementsystem / Übertemperaturschutz** | Erfasst thermische Messgrößen von Energiespeicher, Gehäuse und Lade-/Lastpfad; stoppt Laden und Entladen beim Überschreiten der Grenztemperatur. Trägt zur Verhinderung thermischen Durchgehens unter Einfachfehler-Bedingungen bei; entsperrt nach Unterschreiten der Freigabeschwelle. |

### 3.3 Mechanik-Elemente

| ID | Element | Logische Funktion |
|----|---------|-------------------|
| **LE-M1** | **Gehäuse / Drop-in-Trägerstruktur** | Formschlüssige Einpassung in das Batteriefach des Referenzgeräts (strikter Drop-in ohne Adapter/Umbau/Kabelbrücke), inkl. Verriegelung der Ausgangsschnittstelle im Fach. Spritzwasserschutz **IPx4 (DIN EN 60529, verbindlich)**. Robustheit gegenüber Sturz (1 m), Vibration und Handling; bei mechanischer Beschädigung keine gefährliche Formänderung/Freisetzung nach außen. |
| **LE-M2** | **Kennzeichnung & Begleitdokumentation** | Trägt dauerhafte Kennzeichnung (Nennspannung, Polung/Anschlussschema, Warn- und Entsorgungshinweise, CE, Batteriegesetz/Recyclingkennzeichnung) und liefert deutschsprachige Anleitung/Sicherheitshinweise. |

**Summe: 13 logische Elemente** (Hardware 6, Software-Logik 5, Mechanik 2).

---

## 4. Statische Schnittstellen (logisch)

| ID | Von | Nach | Art | Logischer Daten-/Energieinhalt |
|----|-----|------|-----|-------------------------------|
| IF-01 | 230V-Netzversorgung (Außenwelt) | LE-H2 | Energie | Primärenergie Netzladung |
| IF-02 | LE-H2 | LE-S2 | Energie + Information | Verfügbare Ladeleistung, Versorgungsstatus |
| IF-03 | Solarmodul (Außenwelt, optional) | LE-H3 | Energie | Schwankende Solarleistung (Einstrahlung/Temperatur) |
| IF-04 | LE-H3 | LE-S2 | Energie + Information | Ladeleistungsprofil, Status |
| IF-05 | LE-S2 | LE-H1 | Energie + Kommando | Geregelter Ladestrom, Ladeende-Kommando |
| IF-06 | LE-H1 | LE-S1, LE-S5 | Information | Klemmenspannung, Ladezustand, thermische Messgrößen |
| IF-07 | LE-S1/S3/S4/S5 | LE-H4 | Kommando | Trenn-/Begrenzungs-/Wiedereinschalt-Kommando |
| IF-08 | LE-H1 | LE-H4 | Energie | Lastpfad-Energiequelle |
| IF-09 | LE-H4 | LE-H5 | Energie | Geschalteter Laststrom |
| IF-10 | LE-H5 | Weidezaungerät (Außenwelt) | Energie | Klemmenspannung nominal ≈ 9 V, Laststrom (Anschlussbild 9V-Trockenbatterie) |
| IF-11 | LE-H6 | LE-H1, LE-S1…S5 | Schutzwirkung | Begrenzung/Entkopplung von Störimpulsen auf interne Pfade |
| IF-12 | LE-M1 | Batteriefach Referenzgerät (Außenwelt) | Mechanik | Formschluss, Passung, Verriegelung, IPx4-Dichtung |
| IF-13 | LE-M2 | Anwender / Behörden (Außenwelt) | Information | Kennzeichnung, Anleitung, Sicherheitshinweise |

Parameter der Schnittstellen (z. B. Spannungsfenster IF-10, Panel-Daten IF-03,
Isolationsnorm IF-01) bleiben bis zum Entscheid offen (siehe Abschnitt 8).

---

## 5. Dynamisches Verhalten

### 5.1 Logisches Zustandsmodell

| ID | Zustand | Beschreibung | Eintritt | Austritt |
|----|---------|--------------|----------|----------|
| Z1 | LAGERUNG / IDLE | Kein Last- und kein Ladestrom; minimale Selbstentladung; winterliche Außerbetriebnahme | Keine Aktivität, keine Ladequelle | Last angefordert → Z2; Ladequelle → Z5/Z5a |
| Z2 | ENTLADUNG | Versorgt das Weidezaungerät; Schutzlogik überwacht aktiv | Last über Ausgang angefragt, Betriebsfenster ok | Tiefentladung → Z3; Kurzschluss → Z4; Übertemperatur → Z6; Störimpuls → Z8 (transient) |
| Z3 | ENTLADESCHUTZ / LAST GETRENNT | Tiefentlade-Schutzlogik hat Last abgetrennt; Energiespeicher bleibt geschützt | Abschaltgrenze erreicht | Ladequelle angeschlossen → Z5/Z5a (Laden zwingend möglich) |
| Z4 | KURZSCHLUSS BEGRENZT | Strompfad unterbrochen/begrenzt | Überstrom/Kurzschluss am Ausgang | Fehler beseitigt + Wiederanlauf → Z2/Z1 |
| Z5 | LADUNG NETZ | Laderegelung führt Ladeablauf aus Netz-Ladeeingang | Netz-Ladequelle aktiv, Ladung erforderlich | Ladeendekriterium → Z1; Übertemperatur/Störung → Z6 |
| Z5a | LADUNG SOLAR (optional) | Ladung aus variabler Solarleistung | Solarquelle aktiv | Ladeendekriterium → Z1; Schutzfall → Z6 |
| Z6 | THERMISCHER STOPP | Laden und Entladen gestoppt | Grenztemperatur überschritten | Abkühlung unter Freigabeschwelle → Z1/Z2 |
| Z7 | VERPOLUNG ABGEWIESEN | Verpolte Anschaltung ohne schädigende Wirkung; keine Fehlfunktion | Verpolung erkannt (Geräte-/Ladepfad) | Korrekte Anschaltung → Z1/Z2 |
| Z8 | STÖRUNG AKTIV (transient) | Störimpuls vom Gerät wird begrenzt; kein Ladezustandsverlust | Impuls/Funken an Klemmen | Störung abgeklungen → Z2 (ohne Fehlauslösung) |
| Z9 | SICHERES VERSAGEN | Mechanische Beschädigung/Einfachfehler → keine gefährliche Freisetzung (Brand, Gas, Flüssigkeit, berstende Teile) | Einfachfehler/mechanische Einwirkung | Endzustand, nicht zurückgesetzt |

### 5.2 Logische Abläufe (Sequenzen)

**5.2.1 Entlade-/Tiefentladesequenz (Z2 → Z3)**
1. Ausgangsschnittstelle (LE-H5) wird mit dem Gerät verbunden, Last wird angefragt.
2. Energiespeicher (LE-H1) speist über Lastpfad (LE-H4) den Verbraucher.
3. Schutzlogik LE-S1 überwacht fortlaufend Spannung/Ladezustand (IF-06).
4. Erreicht der Zustand die Abschaltgrenze, kommandiert LE-S1 die Trennung (IF-07).
5. LE-H4 unterbricht den Lastpfad; der Energiespeicher bleibt im geschützten Bereich (Z3).
6. Erneute Entladung ist erst nach Ladung möglich (Beginn mit Z5/Z5a).

**5.2.2 Wirkzusammenhang Laufzeit (N-01):** Nutzbare Kapazität = Kapazität bis zur
Abschaltgrenze von LE-S1. Die Abschaltgrenze (DoD) ist **fixiert (LiFePO4 3S)**:
≤ 2,5 V/Zelle (7,5 V Pack unter Last) ≈ 80 % DoD (S-01, docs/01); Parametrierung
in Phase P.

**5.2.3 Ladesequenz Netz (Z5)**
1. Netz-Ladeeingang (LE-H2) erhält Primärenergie (IF-01); galvanische Trennung zur Zellseite ist sichergestellt.
2. LE-S2 erhält Ladeleistung/Status (IF-02) und führt einen geregelten Ladeablauf (IF-05).
3. Ladung endet zuverlässig beim Ladeendekriterium (Überladeschutz, S-02); Überladung über definierten Dauerstrom hinaus wird sicher verhindert.
4. Nach Ladeende Übergang Z5 → Z1; Gesamtdauer ≤ 12 h (N-08).
5. Bei Übertemperatur Abbruch → Z6.

**5.2.4 Ladesequenz Solar (optionale Variante, Z5a)**
1. Solar-Ladeeingang (LE-H3) nimmt schwankende Leistung auf (IF-03).
2. LE-S2 regelt die Ladung gemäß verfügbarer Leistung; keine Schutzabschaltung unter definiertem Einstrahlungs-/Temperaturprofil (F-06).
3. Ladeende-Kriterium wird auch unter Leistungsschwankungen zuverlässig erkannt (keine Überladung, S-02).
4. Kompatibilitätsnachweis Panel/Pack ist verpflichtend (F-06).

**5.2.5 Kurzschluss-Sequenz (Z4)**
1. An den Ausgangsklemmen (LE-H5) tritt ein Kurzschluss/persistenter Überstrom auf.
2. LE-S3 erkennt den Fehlerstrom (IF-06/Strompfad) und unterscheidet ihn von betriebsnormalen Lastpulsen (F-02, kein Auslösen).
3. LE-S4-bzw. LE-S3-Kommando (IF-07) → LE-H4 unterbricht/begrenzt den Strom.
4. Keine Entzündung/Glut, keine Gefahr; nach Fehlerentfernung und Wiederanlauf funktionsfähig (S-03).

**5.2.6 Verpolungs-Sequenz (Z7)**
1. Eine verpolte Verbindung wird am Geräte- oder Ladepfad hergestellt.
2. LE-S4 erkennt die Verpolung (IF-10/Polungskennung) und verhindert schädigenden Stromfluss.
3. Keine Zellschädigung, keine Brandgefahr, keine Fehlfunktion.
4. Nach korrekter Wiederanschaltung voll funktionsfähig (S-04).

**5.2.7 Übertemperatur-Sequenz (Z6)**
1. LE-S5 erfasst thermische Messgrößen (IF-06) über den Temperaturbereich (N-02).
2. Beim Überschreiten der Grenztemperatur werden Laden und Entladen gestoppt (IF-07).
3. Unter Einfachfehler-Bedingungen wird thermisches Durchgehen verhindert (S-05).
4. Nach Abkühlung unter die Freigabeschwelle: Rückkehr in Normalbetrieb.

**5.2.8 Störimpuls-Sequenz (Z8)**
1. Impulse/Funken des Weidezaungeräts wirken an den Klemmen (IF-10).
2. LE-H6 begrenzt die Störwirkung auf die internen Pfade (IF-11).
3. Keine Fehlauslösung der Schutzlogik, kein Ladezustandsverlust über Toleranz; nach Abklingen Rückkehr in Z2 ohne Funktionsbeeinträchtigung (F-07).

---

## 6. Traceability (Anforderungen → Logische Elemente)

| Anforderung | Logische Elemente | Anmerkung / offene Bezüge |
|-------------|-------------------|---------------------------|
| SYS-REQ-F-01 | LE-H1 | Spannungsfenster **festgelegt (LiFePO4 3S: 7,5–10,95 V, nominal 9,6 V)**; OQ-05 bestätigt am realen Gerät |
| SYS-REQ-F-02 | LE-H1, LE-S3, LE-S1 | Logische Abgrenzung Pulslast vs. Fehlerfall (F-02 ↔ S-03), Grenzwerte Phase P |
| SYS-REQ-F-03 | LE-H5, LE-M1 | Elektrik (Kontaktwiderstand ≤ 50 mΩ) + Verriegelung im Fach |
| SYS-REQ-F-04 | LE-M1 | Pack-Obergrenze B 185 × H 155 × T 125 mm (3,58 l) gesetzt; **Fach-Vermessung BESTÄTIGT (2026-09-13, M2)** |
| SYS-REQ-F-05 | LE-H2, LE-S2 | Modus „Laden bei montiertem Pack“ offen → OQ-04 |
| SYS-REQ-F-06 | LE-H3, LE-S2 | Panel-Spezifikation offen → OQ-06 |
| SYS-REQ-F-07 | LE-H6 | Wirkung auf LE-H1 und Schutzlogik (IF-11) |
| SYS-REQ-N-01 | LE-H1, LE-S1 | Nennkapazität ≥ 31 Ah (fixiert); Abschaltgrenze definiert nutzbaren Bereich (siehe 5.2.2) |
| SYS-REQ-N-02 | LE-H1, LE-S5 | Temperaturbereiche Entladen/Laden; **Grenzwerte fixiert (LiFePO4): −10…+40 °C / +0…+40 °C** |
| SYS-REQ-N-03 | LE-M1 | IPx4 nach DIN EN 60529 (verbindlich) |
| SYS-REQ-N-04 | LE-H1 | Selbstentladung Lagersaison ≥ 6 Monate; **Zielwert fixiert: ≤ 5 %/Monat, Mindestladezustand ≥ 70 %** |
| SYS-REQ-N-05 | LE-H1 | Eigenschaftselement; technologiespezifischer Pflegebedarf offen (kein eigener Zustand nötig) |
| SYS-REQ-N-06 | LE-M1 | Robustheit (Sturz/Vibration/Handling) |
| SYS-REQ-N-07 | LE-H1, LE-S1 | **Zyklusziel fixiert: ≥ 2000 Zyklen @ 80 % DoD (LiFePO4)**; **Preis ≤ 120 EUR / Amortisation = Business Case, keinem logischen Element zuordenbar** → OQ-07 |
| SYS-REQ-N-08 | LE-H2, LE-S2 | Ladedauer ≤ 12 h (fixiert); Laderate/Kennlinie Phase P |
| SYS-REQ-S-01 | LE-S1, LE-H4 | zwingend (R8); **DoD-Grenze fixiert: Abschaltgrenze ≤ 2,5 V/Zelle ≈ 80 % DoD** |
| SYS-REQ-S-02 | LE-S2, LE-H2 | Ladeendekriterium; keine Überschreitung max. Zellspannung |
| SYS-REQ-S-03 | LE-S3, LE-H4 | Unterbrechung/Begrenzung; Wiederanlauf nach Fehlerentfernung |
| SYS-REQ-S-04 | LE-S4, LE-H5 | Verpolungserkennung ohne Schädigung |
| SYS-REQ-S-05 | LE-S5 | Thermomanagement; Missbrauchsvorfälle verhindern Durchgehen |
| SYS-REQ-S-06 | LE-H2 | Galvanische Trennung Netzpfad ↔ Zellseite; Normauswahl offen → OQ-11 |
| SYS-REQ-S-07 | LE-M2 | Dauerhafte Kennzeichnung + deutschsprachige Dokumentation |
| SYS-REQ-S-08 | LE-M1, LE-H1 | Kein gefährliches Versagen/Einfachfehler; technologiespezifisches Missbrauchsverhalten offen → OQ-11/Technologie |

Statistik: **23/23 Systemanforderungen auf 13 logische Elemente abgebildet**
(davon: hart abgebildet 22; N-07 nur teilweise, da Kostenziel ohne Architektur-Element — siehe OQ-07).

---

## 7. Konsistenzprüfung

Die logische Architektur ist in sich und gegen die Anforderungen geprüft:

| Konflikt / Wechselwirkung | Architektonische Auflösung (logisch) |
|---------------------------|----------------------------------------|
| F-02 (keine Schutzauslösung bei Pulsen) ↔ S-03 (Kurzschlussschutz) | LE-S3 muss Betriebspulse von persistenter Überlast **logisch unterscheiden** (Trennkriterium); Schwellen erst Phase P |
| S-01 (Tiefentladung schützt Zelle) ↔ N-01 (nutzbare Laufzeit bis Abschaltgrenze) | Abschaltgrenze (DoD) ist der definierte Rand des Nutzbereichs — konsistent, da Identität der Grenze |
| N-01 (≥ 31 Ah) ↔ F-04 (striktes Drop-in, Packvolumen) | **Bestätigt (LiFePO4 3S): Zellblock ≈ 1,0–1,2 l / ≈ 2,8–3,3 kg in Pack-Budget 185 × 155 × 125 mm (3,58 l), Innen ≈ 3,0–3,2 l darstellbar**; Fach-Vermessung bestätigt Formpassung |
| N-08 (Ladezeit ≤ 12 h) ↔ N-01 (31 Ah) | Erfordert hinreichende Laderate/Kennlinie; logisch auf LE-S2/LE-H2 verortet, physisch Phase P |
| F-06 (wechselnde Solarleistung) ↔ S-02 (keine Überladung) | LE-S2 muss Ladeende auch bei variabler Leistung robust erkennen — explizite logische Anforderung an LE-S2 |
| F-07 (Störfestigkeit) ↔ S-03/S-01 (Schutzauslösung) | Störimpulse dürfen Schutzlogik nicht fehlauslösen; LE-H6 entkoppelt, Schwellen Phase P |
| N-04 (Selbstentladung Lagersaison) ↔ S-01 (Tiefentladung) | Selbstentladung darf den Energiespeicher in Lagerung nicht in den Tiefentladebereich führen; logische Verträglichkeit gefordert |
| N-02 (Laden +0…+40 °C) ↔ S-05 (Übertemperaturstopp) | Übertemperaturschwelle liegt über Betriebsbereich; Konsistenz der Grenzwerte Phase P |

Keine inneren Widersprüche der Elementfunktionen festgestellt.

---

## 8. Offene Zuordnungsfragen (Anforderungen ohne eindeutiges logisches Element)

Gemäß Base-Practice (Punkt 7) werden uneindeutige Zuordnungen **nicht selbst
entschieden**, sondern als offene Frage markiert:

| # | Offene Frage | Betroffene Anforderung | Situation |
|---|--------------|------------------------|-----------|
| OQ-04 | Laden bei montiertem/angeschlossenem Gerät (ja/nein) | F-05 | Bestimmt, ob ein zusätzlicher Betriebsmodus „Laden bei anliegendem Gerät“ in das Zustandsmodell (Z5 ↔ Z2) eingeführt werden muss — Zuordnung von F-05 auf LE-H2/LE-S2 ist bedingt |
| OQ-05 | Eingangsspannungsfenster der Zielgeräte (oem-seitig nicht veröffentlicht) | F-01 | **BESTÄTIGT (2026-09-13, M1):** 3S-Profil am realen Gerät verifiziert; Klemmenbereich als Zielwert fixiert |
| OQ-06 | Solar-Panel-Spezifikation (Leerlaufspannung, Leistungsklasse) | F-06 | Schnittstellenparameter von IF-03/LE-H3 offen; ohne Panel-Modell ist der Kompatibilitätsnachweis nicht spezifizierbar |
| OQ-09 | Nageschutz („nagesicher“ Gehäuse/Kabel) ja/nein | §5.1 (Marktanalyse) | Noch **keine Anforderung** und daher **kein logisches Element** — bei Entscheidung „ja“ ist ein zusätzliches mechanisches Element bzw. eine LE-M1-Zusatzfunktion zu definieren |
| OQ-10 | Ladezustandsanzeige als Feature ja/nein | — (Wunsch, nicht in SR) | Kein logisches Element vorhanden; bei Entscheidung „ja“ neues Element (z. B. LE-S6 Anzeige) erforderlich |
| OQ-11 | Prüf-/Normenrahmen (Zellabuse EN 62133, Netzteilsicherheit VDE/EN, IP IEC 60529, Gefahrgut UN3480/3481) | S-05, S-06, S-08, N-03 | Normenwahl wirkt auf Verifikationsziele, nicht auf die Elementstruktur; ohne Normbezug sind Zielkriterien von S-06/S-08 nicht bindend fixierbar |
| OQ-07 * | Preisziel ≤ 120 EUR / Amortisation ≥ 6 Saisons | N-07 | Kosten-Business-Case ist **keinem logischen Architektur-Element zuordenbar**; Einfluss auf Zielkosten/Stückliste erst in Phase P (S.8.3) |
| OQ-08 * | Technologieentscheidung (LiFePO4-3S vs. AGM vs. NiMH) | F-01, N-02, N-04, N-07, S-01, S-05, S-08 | **GESCHLOSSEN (2026-09-13): LiFePO4-3S final**; wirkt über die in docs/01 §8.1 fixierten Zielwerte auf die LE-H1-/Schutzlogik-Parametrierung — Phase P |
| OQ-12 * | Fachgeometrie-Maße der Referenzgeräte (SYS.3-Milestone) | F-03, F-04 | Pack-Obergrenze B 185 × H 155 × T 125 mm (3,58 l) **gesetzt**; LE-M1-Parametrierung (Passung/Verriegelung/IF-12) auf dieser Basis bestimmbar; **Fach-Vermessung BESTÄTIGT die Passung (2026-09-13, M2)** |

\* OQ-07…OQ-12 sind keine neuen Zuordnungsprobleme, sondern dokumentierte,
bewusst offene Entscheidungen aus der Anforderungsspezifikation (Abschnitt 7.2/8);
sie werden hier architekturseitig als offene Bezüge geführt.

---

## 9. Hinweise für die nächste Phase (Phase P, physisch)

Phase P muss folgende Punkte aufgreifen (Zielwerte für LiFePO4-3S fixiert):

1. **Fach-Vermessung der Referenzgeräte als Bestätigung** (SYS.3-Milestone, F-04,
   OQ-12) — Pack-Maximalmaße 185 × 155 × 125 mm (3,58 l) sind gesetzt; die
   Vermessung bestätigt die Formpassung (LE-M1).
2. **Technologieentscheidung — GESCHLOSSEN (LiFePO4-3S, 2026-09-13)**; alle
   Zielwerte in docs/01 §8.1 fixiert: DoD-/Abschaltgrenze 2,5 V/Zelle (S-01),
   Spannungsprofil 7,5–10,95 V (F-01), Temperaturgrenzen −10…+40/+0…+40 °C (N-02),
   Selbstentladung ≤ 5 %/Monat (N-04), Zyklusziel ≥ 2000 @ 80 % DoD (N-07),
   Ladeprofil CC/CV mit CV 3,65 V/Zelle (S-02, N-08), Thermoschwellen (S-05).
3. **Physikalische Zuordnung der 13 logischen Elemente** auf Baugruppen/Bauteile
   (z. B. Energiespeicher-Zellblock, Schutzschaltung gemäß LE-Sx, Schalteinrichtung
   LE-H4, Dicht-/Gehäusekonzept LE-M1).
4. **Trennkriterium Puls vs. Überstrom** für LE-S3 sowie Schwellen aller
   Schutzlogiken quantifizieren; Impulstabelle aus Reihenmessung (F-02/F-07).
5. **Isolationsauslegung** für LE-H2 (S-06) inkl. Normauswahl (OQ-11); Kennwerte
   für Netzpfad, Schutzklasse, Berührschutz.
6. **Thermisches Design** für LE-S5 (Sensorik, Wärmepfade), Kältebereich N-02,
   Heat-Management der Laderate (N-08).
7. **EMV-/Störfestigkeitsbeschaltung** für LE-H6 (Filter-/Beschaltungslösungen).
8. **Gehäusematerialien, IPx4-Dichtkonzept, Robustheitsprüfung** (N-03, N-06);
   Entscheidung Nageschutz (OQ-09) vor Materialwahl.
9. **Qualifizierungs-/Verifikationsplan** für die Verifikationskriterien der
   Anforderungsspezifikation (normen-, prüfungs-, und kriterienseitig gemäß OQ-11).
10. **Zielkosten-/Business-Case-Prüfung** ≤ 120 EUR, Amortisation ≥ 6 Saisons
    (N-07) gegen Phase-P-Stückliste; ggf. Solar-Varianten-BOM (mit/ohne, OQ-06).

---

## 10. Formale Hinweise

- Elementtypen gemäß SYS.3: Hardware (LE-H1…H6), Software-Logik (LE-S1…S5), Mechanik (LE-M1, LE-M2).
- Alle 23 Systemanforderungen tracebar zugeordnet (Abschnitt 6); offene Bezüge
  explizit gekennzeichnet und in Abschnitt 8 nachverfolgbar.
- Keine Technologie-, Bauteil-, Hardware-, Marken- oder Implementierungsaussagen
  enthalten — dies ist ausschließlich Phase P vorbehalten.
- Nächste Schritte: Freigabe Phase L (Review), dann Phase P.

> **Verweis Komponenten-Detaillierung:** Die physische Umsetzung ordnet die 13
> logischen Elemente den **12 physischen Elementen (PE-01…PE-12)** und damit der
> Baugruppen-Gliederung **K1 (Ladegerät mit Laderegler) / K2 (Batterie mit Zellen
> und BMS) / K3 (Solarmodul, optional)** zu — Detaillierung und Skizze in
> `docs/03`, Abschnitt 1.4. Die Zuordnungen auf logischer Ebene bleiben davon unberührt.