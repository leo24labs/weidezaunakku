# Physische Systemarchitektur (ASPICE SYS.3, Phase P)
## Wiederaufladbares 9V-Akku-Pack für Weidezaungeräte — Referenzrealisierung LiFePO4 3S

| | |
|---|---|
| Prozess | SYS.3 – Systemarchitektur, **Phase P (physische Realisierung)** |
| Eingangsartefakt | `02_spezifikation_architektur_logisch.md` (Phase L) und `01_spezifikation_anforderungen.md` (SYS.2) |
| Ausgangsartefakt | Diese Spezifikation (physische Architektur) |
| Nachfolgeprozess | SYS.4 (Integration/Verifikation) → übernimmt Verifikationskriterien aus SYS.2 |
| Elementtypen | Hardware, Software (Embedded-Schutz-/Regelungslogik), Mechanik |
| Status | **Referenzrealisierung LiFePO4 3S — Technologieentscheidung final (2026-09-13)**; Pack-Maximalmaße 185 × 155 × 125 mm fixiert |
| Detaillierung | **Komponenten-Gliederung (K1/K2/K3) + Architektur-Skizze** (2026-09-13) — siehe Abschnitt 1.4 |
| Stand | 2026-09-13 |

> **Phasencharakter:** Dieses Artefakt konkretisiert die 13 logischen Elemente
> (LE-H1…LE-M2) auf physische Elemente einer **Referenzrealisierung LiFePO4 3S
> (9,6 V)**. Abweichungen, die eine andere Technologie (= AGM/NiMH/sonstige) an
> den physischen Elementen verursacht, sind als **Einfluss-Notizen (E-Nx)** gekennzeichnet —
> es werden keine Alternativen als gleichwertige Parallelpläne geführt.

---

## 1. Zweck, Vorgehen und Eingangsnotizen

### 1.1 Vorgehen (Base Practices Phase P)

1. Abbildung jedes logischen Elements auf ein physisches Element (Technologie/HW/SW/Baugruppe).
2. Konkretisierung der physischen Schnittstellen (Baugruppen ↔ Baugruppen, Pack ↔ Außenwelt).
3. Ressourcenabschätzung je physischem Element (Volumen, Gewicht, Energie, Bauteil-Klassen).
4. Konsistenz zur Phase L (jede physische Entscheidung auf LE rückführbar).
5. Bewertung der physischen Architektur (Wartbarkeit, Testbarkeit, Wiederverwendbarkeit, Technologie-Austauschbarkeit).
6. Traceability LE ↔ PE sowie zweistufige Traceability Anforderung → LE → PE.
7. Mehrdeutige Zuordnungen als offene Frage markieren.

### 1.2 Eingangsnotizen aus Phase L (maßgeblich für diese Realisierung)

| Eingangsnotiz | Wirkung auf die physische Architektur |
|---------------|---------------------------------------|
| OQ-05 · Spannungsfenster der Zielgeräte (oem-seitig unbekannt) | Technologie **entschieden: LiFePO4 3S** (9,6 V nominal, 7,5–10,95 V-Band); Messung am realen Gerät (M1, docs/06) **BESTÄTIGT (2026-09-13)** → F-01-Zielwert fixiert |
| OQ-12 · Fachgeometrie der Referenzgeräte / Pack-Maximalmaße (SYS.3-Milestone) | **Verbindliche Pack-Obergrenze vom Auftraggeber gesetzt: B × H × T = 185 × 155 × 125 mm (3,58 l Gesamtvolumen)** → determiniert die Zelltopologie des 31-Ah-Blocks (Anzahl paralleler Zellpfade, Formfaktor). Fach-Vermessung der Referenzgeräte (M2, docs/06) **BESTÄTIGT das Drop-in (2026-09-13)** |
| OQ-04 · Laden bei montiertem Pack (ja/nein) | Legt fest, ob die Ladebuchse bei montiertem Pack zugänglich bleiben muss und ob Lastpfad/Ladepfad gleichzeitig aktiv sein können → Gehäusekonzept + Schutzlogik |
| OQ-06 · Solar-Panel-Spezifikation | Bestimmt den Spannungs-/Leistungsbereich von PE-05 (Panel-Buchse), PE-04 (Laderegler) |
| OQ-11 · Prüf-/Normenrahmen (EN 62133, VDE/EN, IEC 60529, UN3480/3481) | Determine Anforderungen an Isolation, Abuse-Festigkeit, IP-Prüfung, Transportklassifikation der Zellen |
| OQ-07 · Zielkosten ≤ 120 EUR | Budgettreiber; wirkt auf die Wertklasse aller physischen Elemente (Referenz: Zielkostenbewertung gegen LiFePO4-BOM) |

### 1.3 Referenz-Scope (Entscheidungsstatus)

- **Ladegerät (extern, mitgeliefert, K1):** Wandlung 230 V AC → netzgetrennte, **geregelte** Niederspannungs-Gleichspannung (Referenz: SELV, ~12–15 V); **Laderegler PE-04 (CC/CV) ist Bestandteil dieses externen Ladegeräts** (seit 2026-09-13, Auftraggeber-Vorgabe); galvanische Trennung außenseitig (S-06). Das Pack selbst empfängt **keine** 230-V-Spannung und enthält **keinen** Laderegler (Referenzlösung — minimiert Volumen, Gewicht, CE-Komplexität, Risiko; am Pack nur Buchse PE-02 + Zuleitung PE-03).
  - *Alternative (bewusst nur als Einfluss-Notiz, nicht ausgeplant):* 230-V-Wandler im Pack (PE-03a) → mehr Volumen, Gewicht, höhere Sicherheits-/Emissionsanforderungen (E-N6).
- **Solar (Option):** optionaler Panel-Eingang PE-05; Kompatibilitätsnachweis verpflichtend (F-06).

### 1.4 Komponenten-Gliederung (Architektur-Detaillierung)

Die 12 physischen Elemente werden in **drei Komponenten (Subsysteme)** gegliedert.
Sie bilden die Baugruppen-Ebene für Detailentwurf, BOM und SYS.4-Verifikation:

| Komponente | Bezeichnung | Enthaltene physische Elemente | außen liegend |
|------------|-------------|-------------------------------|----------------|
| **K1** | **Ladegerät mit integriertem Laderegler** | **Netzteil mit Laderegler PE-04 (CC/CV, optional MPPT) im Gehäuse** · SELV-Ladekabel (PF-01) · PE-02 Ladebuchse + Eingangsbeschaltung (am Pack) · PE-03 Pack-Ladezuleitung | komplettes Ladegerät inkl. PE-04 extern; nur Ladebuchse PE-02 + Zuleitung PE-03 reichen ins IPx4-Pack |
| **K2** | **Batterie mit Zellen und BMS** | PE-01 Zellblock 3S LiFePO4 · PE-09 BMS-Schutzmodul · PE-10 Thermische Sensorik · PE-06 Lastpfad-Schalter · PE-07 Ausgangsklemmen · PE-08 EMV-Beschaltung · PE-11 Gehäuse (IPx4) · PE-12 Kennzeichnung/Doku | gesamtes Pack (Drop-in) |
| **K3** | **Solarmodul (optional)** | PE-05 Solar-Input + externes Panel | Panel außen; Buchse am Pack |

> **Verortung der Ladekette (2026-09-13, Auftraggeber-Vorgabe):** Der **Laderegler
> (PE-04, CC/CV)** ist **Bestandteil des externen Ladegeräts** (Netzteil-Gehäuse,
> 230 V → geregelte SELV-Ladespannung, galvanisch getrennt, S-06). Am Pack liegen
> nur die **Ladebuchse PE-02** (Eingangs-/Verpolungs-/Überspannungsschutz) und die
> kurze **Pack-Ladezuleitung PE-03** (Buchse → Zellblock). Dadurch bleibt das Pack
> geschlossen (Drop-in, IPx4), der Modus „Laden bei montiertem Pack" (OQ-04) und
> die redundante Überlade-Abschaltung über das BMS (S-02, PE-09) sind weiterhin
> realisierbar — der Überladeschutz-Rückkanal (PF-11) läuft über das Ladekabel.
> Netzzuleitung ins Pack: **keine** (S-06).

```
  K1 · LADEGERÄT (extern, im Netzteil-Gehäuse)     K2 · BATTERIEPACK (Drop-in, IPx4)         K3 · SOLARMODUL (Zubehör)
┌──────────────────────────────────────────┐   ┌────────────────────────────────────┐   ┌──────────────────────────┐
│ 230-V-Netzteil → SELV, galvanisch getr.  │   │ Ladebuchse PE-02 (Verpolungs-/     │   │ Solar-Panel (extern)      │
│ (S-06) · Laderegler PE-04 (CC/CV,       │   │  Überspannungs-Schutz) → Pack-     │   │                           │
│  optional MPPT) · Solar-Eingang PE-05   │   │  Zuleitung PE-03 (Buchse → Zell)   │   └─────────────┬────────────┘
│  (optional) · Ladekabel PF-01 + Status  │   └──────────────────┬─────────────────┘                 │
└───────────────┬──────────────────────────┘   │  PF-08 · Ladestrom (geregelt)  │                   │ PF-04
                │  PF-01 · SELV-Ladekabel      └──────────────────┬─────────────────┘                   │
                ▼  (Ladeleistung + Status)                        │                                      ▼
                                            ┌─────────────────────▼─────────────────┐   ┌─────────────┴──────────┐
                                            │ Zellblock 3S LiFePO4 · PE-01          │   │  Solarenergie über     │
                                            │ 31 Ah · 9,6 V · ≈ 1,0–1,2 l           │   │  PE-05 → PE-04 (MPPT  │
                                            └─────────────────────┬─────────────────┘   │  = K1) zum Laderegler │
                                            │  PF-05 (Zellspg./Temp)                  └────────────────────────┘
                                            │ ┌─────────────────────▼─────────────────┐
                                            │ │ BMS PE-09 (S1/S3/S4/S5) + PE-10      │
                                            │ │  · Schutz · Sensorik                  │
                                            │ └─────────────────────┬─────────────────┘
                                            │  PF-10 (Trenn-/Freigabe)
                                                │ ┌──────────────────▼─────────────────┐
                                                │ │ Lastpfad-Schalter PE-06            │
                                                │ └──────────────────┬─────────────────┘
                                                │  PF-07 (geschalteter Lastpfad)
                                                │ ┌──────────────────▼─────────────────┐
                                                │ │ Ausgangsklemmen PE-07 ──PF-13──►  │
                                                │ │   Zaun-Gerät (Batteriefach)        │
                                                │ └────────────────────────────────────┘
                                                │ EMV PE-08 · Gehäuse PE-11 · Kennz.    │
                                                │ PE-12 (IPx4, Drop-in, Verriegelung)   │
                                                └──────────────────────────────────────┘
```

**Schnittstellen auf Komponenten-Ebene** (Baugruppen-Schnittstellen, Details in Abschnitt 4):

| Kante | Komponenten | PF-Bezug | Energie/Daten |
|-------|-------------|----------|---------------|
| Netz laden | K1 ↔ K2 (externes Ladegerät ⇄ Pack) | PF-01 (SELV-Kabel) → PE-02 → PE-03 → Zellblock; Rückkanal PF-11 (über Kabel) | geregelte SELV-Ladespannung/-strom, Ladeend-/Fehler-Status |
| Solar laden (opt.) | K3 ↔ K1 (Panel ⇄ Ladegerät) | PF-04 → PE-05 → PE-04 | Panel-Leistung, MPPT-Regelung im Ladegerät |
| Last-Ausgang | K2 ↔ Außenwelt (Pack ⇄ Zaun-Gerät) | PF-13, PF-14 | 9V-Last (Pulse), Formschluss/Verriegelung |
| interne Mess-/Steuerpfade | innerhalb K2 | PF-05, PF-06, PF-07, PF-10, PF-12 | Zellspg., Temperatur, Schutz-Kommando |

---

## 2. Mapping logische → physische Elemente

| Logisches Element | Physisches Element (Referenz) | Art der Realisierung |
|-------------------|-------------------------------|----------------------|
| LE-H1 · Energiespeicher | PE-01 · Zellblock 3S LiFePO4 (Referenz) | Zellen + Zellverbinder (3-Serie, Parallelbündel nach Fachgeometrie) |
| LE-H2 · Netz-Ladeeingang | K1 · PE-02 · Ladebuchse (im Pack), PE-03 · Pack-Ladezuleitung (im Pack) | Mechanischer Steckverbinder + Schutzzeile am Eingang; SELV-Leitweg zur Zelle (Laderegler PE-04 im externen Ladegerät K1) |
| LE-H3 · Solar-Ladeeingang (Option) | PE-05 · Solar-Input (Buchse + Eingangsbeschaltung, optional) | Steckverbinder + Schutzzeile, optional MPPT-fähig |
| LE-H4 · Lastpfad-Schalteinrichtung | PE-06 · Lastpfad-Schalter (Leistungshalbleiter + Treibstufe) | MOSFET-Schalter mit Treiberlogik |
| LE-H5 · Elektrische Ausgangsschnittstelle | PE-07 · Ausgangsklemmen nach 9V-Anschlussbild | Feder-/Bügelkontakte, Kontaktwiderstand ≤ 50 mΩ |
| LE-H6 · Störfestigkeitsfunktion | PE-08 · EMV-/Störfestigkeits-Beschaltung | Filter/Entkopplung, TVS-Klemmschutz an Ausgang/Ladepfad |
| LE-S1 · Tiefentlade-Schutzlogik | PE-09 · BMS-Schutzmodul (Teilfunktion) | Analog-/uC-gestützte Schutzlogik inkl. Lasttrenn-Kommando |
| LE-S2 · Laderegelung | K1 · PE-04 · Laderegler CC/CV (im externen Ladegerät) | Ladereglerstufe (referenziert CC/CV; Solar-Modus mit Leistungsanpassung); konstruktiv im externen Ladegerät-Gehäuse |
| LE-S3 · Kurzschluss-/Überstrom-Schutzlogik | PE-09 · BMS-Schutzmodul (Teilfunktion) + PE-06 | Strommessung, Schwellenauswertung, Trennung |
| LE-S4 · Verpolungs-Schutzlogik | PE-09 · BMS-Schutzmodul (Teilfunktion) + PE-07 | Verpolungserkennung, Blockade des Strompfads |
| LE-S5 · Thermomanagement | PE-09 (Teilfunktion) + PE-10 · Thermische Sensorik | NTC-/Temperatursensoren, Auswertung & Freigabe |
| LE-M1 · Gehäuse/Drop-in | PE-11 · Gehäuse (Drop-in, IPx4) + PE-07-Montage | Spritzgussgehäuse, Dichtrings-System, Verriegelung |
| LE-M2 · Kennzeichnung & Dokumentation | PE-12 · Kennzeichnung & Begleitdokumentation | Dauerhafte Beschriftung + deutschsprachige Anleitung |

**Anzahl physischer Elemente: 12** (PE-01…PE-08 Hardware, PE-04/PE-09 incl. Embedded-SW, PE-10 Sensor-HW, PE-11/PE-12 Mechanik/Information).

> **Komponenten-Gliederung:** Die 12 physischen Elemente bilden die drei Baugruppen
> **K1 = Ladegerät mit Laderegler** (PE-02, PE-03, PE-04 + externes Netzteil),
> **K2 = Batterie mit Zellen und BMS** (PE-01, PE-06…PE-12) und
> **K3 = Solarmodul (optional)** (PE-05) — siehe Abschnitt 1.4 (Skizze + Zuordnung).
> Der Begriff „Pack (K2)" und „Ladeset (K1)" wird im weiteren Text synonym verwendet.

---

## 3. Physische Elemente (Referenzrealisierung LiFePO4 3S)

| Komp · ID | Baugruppe | Physische Funktion (Kurzform) | Technologiezuordnung | Ressourcen-Referenz |
|----|-----------|-------------------------------|----------------------|---------------------|
| K2 · PE-01 | **Zellblock 3S LiFePO4** | 3 Zellen/Serienposition, Nennspannung 9,6 V, Packnennkapazität ≥ 31 Ah; liefert Effektivlast 30–60 mA und Pulse; Betriebstemperatur (Entladen) −10…+40 °C; geringe Selbstentladung für Saisonlagerung | LiFePO4-3S; Zellklasse Zylinder 26 mm-Klasse oder Prismatik/Pouch (Formfaktor nach OQ-12); 3S-Topologie, je Position Parallelbündel für ≥ 31 Ah (Referenz: 8–10 Zellen/Pfad oder große Prismatikzellen) | ≈ 298 Wh; Volumen Zellblock ≈ 1,0–1,2 l; Gewicht ≈ 2,8–3,3 kg (LiFePO4-Komm. 90–110 Wh/kg) |
| K1 · PE-02 | **Ladebuchse (im Pack) + Eingangsbeschaltung** | mechanische Verbindung zum externen Ladegerät K1 (SELV); Verpolungs-, Überspannungs-, Überstrom-Schutz am Eingang; definierte Ladeschnittstelle (Referenz: Koaxial-/Hohlstecker-Klasse, SELV 12–15 V) | Elektromechanischer Steckverbinder + diskrete Schutzbeschaltung (TVS, Sicherungs-Teil) | Volumen ≈ 5–8 cm³, Gewicht ≈ 15–25 g |
| K1 · PE-03 | **Pack-Ladezuleitung (SELV, im Pack)** | empfängt die geregelte Ladespannung des externen Ladegeräts an der Pack-Buchse und führt sie zum Zellblock; hält die Trennung zum Netzpfad (Trennung im externen Ladegerät); Stör-/EMV-Entkopplung am Eingang | Passive Leistungs- und Filter-Bahn + Eingangsschutz; **keine 230-V-Führung im Pack (Referenz)** | kleine Leiterplattenfläche ≈ 5–10 cm²; Gewicht ≈ 10 g |
| K1 · PE-04 | **Laderegler CC/CV (im externen Ladegerät K1)** | führt den Ladeablauf (Referenz: Konstantstrom → Konstantspannung, Abschluss nach Ladeendekriterium); passt den Ladeablauf bei Solarvariante an variable Leistung (Leistungsanpassung, kein Abbruch bei Teillast); verhindert Überspannung des Zellblocks (Überladeschutz, Primärstufe); **Konstruktionsorts-Entscheidung 2026-09-13: im Netzteil-Gehäuse (Pack bleibt reglerfrei)** | Laderegler-Baugruppe im Ladegerät-Gehäuse (IC-Klasse CC/CV, optional MPPT-fähig für Solar) + Steuerlogik (Parametrierung Phase-P-Detail); Ladestrom-/Spannungsmessung am Ausgang; Status-Leitung zum Pack (PF-03) | Ladeleistung ≥ 30 W (≈ C/10) als Referenz; der ≤ 12 h-Nachweis (N-08, docs/01) ist inkl. CV-Abschlussblock und Temperatur-Derating zu führen; Leiterplatte im Ladegerät ≈ 10–16 cm² |
| K3 · PE-05 | **Solar-Eingang (optional, am Ladegerät K1)** | optionale Panel-Buchse am externen Ladegerät mit Eingangsbeschutz; speist den Laderegler (PE-04) direkt aus Panelenergie (gleiche Baugruppe); verträgt schwankende Einstrahlung/Temperatur (kein Fehlabbruch, kein Überladen) | Steckverbinder + Schutz-/Filterbeschaltung; Eingangsspannungsbereich nach OQ-06; mit PE-04 im Ladegerät-Gehäuse | Volumen ≈ 5–8 cm³; Gewicht ≈ 15 g |
| K2 · PE-06 | **Lastpfad-Schalter** | unterbricht/begrenzt den Strom zwischen Zellblock und Ausgang auf BMS-Kommando; im Normalbetrieb einlassend, puls- und impulsfest; Wiedereinschaltfähigkeit | Leistungs-MOSFET-Schalter + Treiber/Beschaltung; Verlustleistung minimiert; Schwellen gemäß F-02 (Pulse) vs. S-03 (Fehlerfall) | Durchlasswiderstand klein (ZehnmΩ-Klasse); Leiterplatte ≈ 5–8 cm² |
| K2 · PE-07 | **Ausgangsklemmen (9V-Anschlussbild)** | Klemmen nach Anschlussbild der 9V-Trockenbatterie; Kontaktwiderstand ≤ 50 mΩ je Kontakt; verriegelt im Fach; verpolungssicher (mechanische + logische Sicherung S-04) | Feder-/Bügelkontakte, Kontaktwerkstoffe (Referenz: Kontaktflächen verzinnt/vergoldet Klasse); Geometrie nach OQ-12 | Kontaktkraft und -lebensdauer nach Anschlussbild; Gewicht ≈ 20–30 g |
| K2 · PE-08 | **EMV-/Störfestigkeits-Beschaltung** | begrenzt kapazitive/induktive Impuls- und Funkenrückwirkungen vom Weidezaungerät an Klemmen und Ladepfad; keine Fehlauslösung der Schutzlogik, kein Ladezustandsverlust | TVS-/Klemmdioden-Klasse, Filterkondensatoren, Entkopplungs- und Ferritmaßnahmen; Layout-Notizen (kurze Pfade, Masseführung) | Leiterplattenfläche integriert in PE-09/PE-06-Baugruppe ≈ 5–8 cm² |
| K2 · PE-09 | **BMS-Schutzmodul** | zentrale Schutz-/Überwachungslogik: Zellspannungsüberwachung (je Serienposition), Strommessung (Ladung/Entladung), Temperaturauswertung; realisiert Tiefentladeschutz (LE-S1), Kurzschluss-/Überstromschutz (LE-S3), Verpolungsschutz (LE-S4), Übertemperaturstopp (LE-S5); kommandiert PE-06 und wirkt unabhängig auf den Ladepfad ein (redundanter Überladeschutz zu PE-04, S-02) | Analog-Beschaltung + Embedded-Schutzlogik (µC/eBMS-Klasse, Referenz: parametrierbare Schutzfunktionen); Balancierungsfunktion nach Technologie-/Zellwahl | Eigenverbrauch < 1 mA (Dominanz über N-01-Rechnung geprüft); Leiterplatte ≈ 15–25 cm²; Gewicht ≈ 30–50 g |
| K2 · PE-10 | **Thermische Sensorik** | erfasst Temperaturen am Zellblock (Referenz: mehrere Messpunkte je Zellpfad) und am Gehäuse/Ladezweig; liefert Messgrößen an PE-09 (Thermomanagement) | NTC-/Temperatursensoren-Klasse, thermisch angekoppelt an Zellen und Gehäuse | Kleinstbauteile; Leiterplattenanteil in PE-09 integriert; Sensorzahl nach Abuse-/Normauslegung (OQ-11) |
| K2 · PE-11 | **Gehäuse (Drop-in, IPx4)** | formschlüssige Einpassung ins Batteriefach (striktes Drop-in, Verriegelung); Spritzwasserschutz IPx4 nach DIN EN 60529 (Dichtkonzept, Ladebuchsen-Abdeckung); Robustheit (Sturz 1 m, Vibration, Handling); im Schadensfall keine gefährliche Freisetzung nach außen | Spritzgussgehäuse (Referenz: UV-/wetterstabiler Kunststoff, z. B. PC/ABS-Klasse), Dichtring-/Lippen-System, mechanische Verriegelung; Auslegung nach OQ-12 | Volumen nach Maximalmaßen (OQ-12): B 185 × H 155 × T 125 mm (3,58 l außen), Innenvolumen ≈ 3,0–3,2 l; Wandstärke/Zellhaltung gemäß N-06; Gewicht ≈ 150–250 g (ohne Zellblock) |
| K2 · PE-12 | **Kennzeichnung & Dokumentation** | dauerhafte Beschriftung (Nennspannung, Polung/Anschlussschema, Warn-/Entsorgungshinweise, CE, Batteriegesetz/Recyclingkennzeichnung) + deutschsprachige Anleitung/Sicherheitshinweise | UV-beständige Lasermarkierung/Etikettierung; Dokumenten-Set (deutsch) | Informations-/Dokumenten-Element; Gewicht vernachlässigbar |

---

## 4. Physische Schnittstellen (Baugruppen-Ebene)

| ID | Von | Nach | Realisierung (konkret) | Logischer Bezug |
|----|-----|------|------------------------|-----------------|
| PF-01 | Ladegerät K1 (extern: Netzteil + PE-04 Laderegler) | PE-02 | SELV-Ladekabel mit Stecker → Pack-Buchse; geregelte Ladespannung (Referenz 12–15 V), Polaritätskennzeichnung, Status-Leitung (Ladeende/Fehler, PF-11) | IF-01, IF-02 |
| PF-02 | PE-02 | PE-03/PE-04 | Schutz-/Filterleitung zum Ladepfad; Eingangsspannungserkennung an PE-09 | IF-02 |
| PF-03 | PE-04 (Laderegler im Ladegerät K1) | PE-09 | Digitale Steuer-/Statusleitungen (Logikpegel) über das Ladekabel: Ladekommandos, Ladeende-Information, Freigabequittung | IF-05, IF-07 |
| PF-04 | PE-05 (opt.) | PE-04 | Panelanschlussleitung, Eingangsspannung/-strom Richtung Laderegler (MPPT-fähig) | IF-03, IF-04 |
| PF-05 | PE-01 | PE-09 | Zellspannungs-Messleitungen (je Serienposition), Balancierungsanschlüsse, Temperatursensorik | IF-06 |
| PF-06 | PE-01 | PE-06 | Leistungspfad Zellblock ↔ Lastschalter (kurz, niederohmig) | IF-08 |
| PF-07 | PE-06 | PE-07 | geschalteter Lastpfad zu den Klemmen (Schalter ↔ Klemmen) | IF-09 |
| PF-08 | PE-02/PE-03 (Pack-Ladepfad) | PE-01 | Ladepfad Ladebuchse ↔ Zellblock (Pack-intern); geregelter Ladestrom vom externen Ladegerät (K1) am PE-02-Eingang | IF-05 |
| PF-09 | PE-08 | PE-07, PE-01, PE-04 | Entkopplungs-/Klemm-Beschaltung an Ausgangs- und Ladepfad | IF-11 |
| PF-10 | PE-09 | PE-06 | Trenn-/Freigabe-Kommando des BMS zum Lastschalter | IF-07 |
| PF-11 | PE-09 | PE-04 (Laderegler im Ladegerät K1) | Überladeschutz-Rückkanal (unabhängige Abschaltung/Entzug Freigabe) über die Status-Leitung des Ladekabels (PF-01) | IF-07 |
| PF-12 | PE-10 | PE-09 | Temperaturmesswerte (analog/digital) | IF-06 |
| PF-13 | PE-07 | Batteriefach-Kontakte (Außenwelt) | elektromechanische Kontaktierung nach 9V-Anschlussbild; Kontaktwiderstand ≤ 50 mΩ | IF-10 |
| PF-14 | PE-11 | Batteriefach (Außenwelt) | Formschluss, Verriegelung, IPx4-Dichtung (Kontaktfläche + Buchsen) | IF-12 |
| PF-15 | PE-12 | Anwender/Behörden (Außenwelt) | Beschriftung, Anleitung, Sicherheitshinweise (deutsch) | IF-13 |
| PF-16 | PE-09 | PE-02 | Verpolungs-/Eingangs-Auswertung am Ladeeingang | IF-02 |

---

## 5. Ressourcenbedarf (Referenzrealisierung)

| Ressource | Wert (Referenz, LiFePO4) | Annahmen / Offenheiten |
|-----------|--------------------------|------------------------|
| **Energie/Zellblock** | ≈ 298 Wh (31 Ah × 9,6 V) | **DoD fixiert: Abschaltgrenze ≤ 2,5 V/Zelle (7,5 V Pack unter Last) ≈ 80 % DoD als Zyklen-Referenz (S-01, docs/01)** |
| **Zellblock-Volumen** | ≈ 1,0–1,2 l (Zellniveau) | LiFePO4-Referenz ~250–300 Wh/l; **Bindung: Pack-Obergrenze 3,58 l (185 × 155 × 125 mm), Innenvolumen ≈ 3,0–3,2 l → LiFePO4 locker darstellbar, hohe Reserve für Zelltopologie-Varianten** |
| **Zellblock-Gewicht** | ≈ 2,8–3,3 kg | LiFePO4 ~90–110 Wh/kg; inkl. Verbinder |
| **Pack-Gewicht gesamt** | ≈ 3,0–3,8 kg (Komponentensumme: Zellblock 2,8–3,3 kg + Elektronik/Beschaltung inkl. Klemmen 0,1–0,15 kg + Gehäuse 0,15–0,25 kg ⇒ 3,05–3,70 kg; zzgl. ≤ 0,1 kg Verpackung/Befestigung/Toleranzen) | **Technologieentscheidung final (LiFePO4 3S); Pack-Budget 3,58 l fixiert; Fachpassung M2/OQ-12 bestätigt (2026-09-13); finale Fixierung je Zelltopologie in Phase P** |
| **Ladeleistung** | ≥ 30 W (≈ C/10) als Referenz (298 Wh ÷ 12 h = 24,8 W ⇒ Null-Marge, daher Marge auf ≥ 30 W) | Der ≤ 12 h-Nachweis (N-08, docs/01) ist **inkl. CV-Abschlussblock und Temperatur-Derating** zu führen; Ladegerät-Klasse (Netzteil + PE-04) und Pack-Zuleitung PE-03 darauf dimensionieren |
| **Ladestrom (Referenz)** | ≈ 3,1 A (≈ C/10), Bereich 2,5–3,3 A je Ladesatz | Zell-/BMS-Verträglichkeit (LiFePO4) bestätigen; CV-Abschlussblock und Derating sind in den ≤ 12 h-Nachweis einzurechnen (N-08) |
| **BMS-Eigenverbrauch** | < 1 mA | Einfluss auf N-01/N-04 über 504 h quantifizieren (≈ < 0,5 Ah) |
| **Bauteil-Klassen** | Zellklasse (Zylinder/Prismatik) · MOSFET-Leistungsschalter · BMS-µC-Schutzbaustein · Laderegler-IC (CC/CV, optional MPPT) · TVS-/Filterkomponenten · NTC-Sensoren · Spritzgussgehäuse PC/ABS · UV-beständige Beschriftung | konkrete Lieferanten/ICs = Phase-P-Detail nach Technologie- und Normentscheid |

**Ressourcen-Bilanz je Komponente (K1/K2/K3):**

| Komponente | Enthaltene PE | Anteil-Kennwerte (Referenz) | Besonderheit |
|------------|---------------|-----------------------------|--------------|
| K1 · Ladegerät (extern) | PE-04 (+ PE-05-optionaler Eingang); PE-02/PE-03 als Pack-seitige K1-Kette | **komplett extern, Pack reglerfrei**: Ladegerät-Platine PE-04 ≈ 10–16 cm², Solar-Eingang PE-05 ≈ 5–8 cm³/15 g; Netzteil/SELV-Kabel PF-01 Teil des Sets | PE-04 im Netzteil-Gehäuse; Pack-Buchse PE-02 + Zuleitung PE-03 gehören physisch zum Pack, funktional zu K1 |
| K2 · Batteriepack | PE-01, PE-06…PE-12 | **das gesamte Pack**: Zellblock ≈ 1,0–1,2 l / 2,8–3,3 kg + Elektronik/Beschaltung 0,1–0,15 kg + Gehäuse PE-11 0,15–0,25 kg ⇒ Pack ≈ 3,0–3,8 kg | Drop-in, IPx4; PE-09/PE-10/PE-08 in Board integriert; M1/M2-Messungen betreffen K2 (Spannungsfenster, Fachpassung) |
| K3 · Solarmodul (opt.) | externes Solar-Panel; Einspeisung über PE-05 am Ladegerät | Panel extern (Set-Zubehör); keine Pack-Panelbuchse | Optionale Variante; Kompatibilitätsnachweis F-06; MPPT-Regelung über K1/PE-04 |

---

## 6. Einfluss-Notizen (Archiv: Bewertung der abweichenden Technologien)

LiFePO4-3S ist seit 2026-09-13 die **finale** Technologieentscheidung (docs/01,
§8.1). Die folgenden Notizen zur Bewertung von AGM-/NiMH-Abweichungen werden
als Dokumentationsbasis der Entscheidung geführt (keine Parallelpläne):

| E-N | Betroffenes Element | Änderung bei AGM | Änderung bei NiMH |
|-----|---------------------|------------------|-------------------|
| E-N1 | PE-01 · Zellblock | 4×2-V-Serienblock (8 V) — spannungsseitig **marginal zu F-01/OQ-05**; deutlich schwerer/großer (Energiedichte ~30–40 Wh/kg) → Machbarkeit von 31 Ah im Fachvolumen **zugespitzt** | 7–8 Serienzellen (1,2 V) → 8,4–9,6 V; höhere Selbstentladung (N-04 gefährdet), kälteempfindlich (N-02) |
| E-N2 | PE-04 · Laderegler | Ladeprofil wechselt auf IUoU/mehrstufig (AGM-spezifisch), kein CC/CV-LiFePO4-Profil | Ladeverfahren dV/dT-basiert; kein LiFePO4-CC/CV-Abschluss |
| E-N3 | PE-09 · BMS | Schutztiefentladung vereinfacht möglich (Zellspannungsmonitoring auf 2-V-Niveau); Überlade-Charakteristik anders | BMS optional; Einzellüberwachung je nach Zellgröße; Eigenverbrauch/ausgangsleitungen anders |
| E-N4 | PE-06 · Lastschalter | unverändert (Topologie nicht betroffen) | unverändert |
| E-N5 | PE-11 · Gehäuse | größeres Volumen/Gewicht nötig → Fachgeometrie-Konflikt F-04 **verschärft** | mittlere Änderung (Serienzellen verbaut); gasende Zellen → Entgasungsöffnung (Abweichung IPx4-Konzept!) |
| E-N6 | PE-02/PE-03 · Ladepfad | Schnittstelle zum Netzteil bleibt Netz-geeignet; AGM verträgt einfachen Lader | Ladespannungsbereich anpassen (Peak > 10 V bei 8S) |
| E-N7 | PE-12 · Kennzeichnung | Gefahrgut-Klassifikation **Säure** (ADR-Umfeld) statt UN3480 | Transportklasse anders; Entsorgungshinweise angepasst |
| E-N8 | **Sicherheitsbewertung gesamt** (S-05/S-08/OQ-11) | Missbrauchsverhalten anders (Säureaustritt) → Abuse-Normenwahl tiefer im elektro-/chemischen Design | Nass-/gasende Zellen → S-08/Belüftungskonzept, insb. IPx4-Konflikt |

**Konsequenz:** Die Entscheidung beeinflusst primär PE-01, PE-04, PE-09, PE-11,
PE-12 sowie die Norm-/Gefahrgutauswahl; PE-06, PE-07, PE-08, PE-10 bleiben
topologisch weitgehend stabil. Eine Entscheidung gegen LiFePO4 muss vor der
Fachgeometrie-Fixierung erfolgen, da sie das Volumen-/Gewichtskonzept negiert.

---

## 7. Architektur-Bewertung

### 7.1 Bewertungskriterien (Base-Practice Punkt 5)

| Kriterium | Bewertung (Referenzrealisierung) |
|-----------|-----------------------------------|
| **Wartbarkeit** | Wartungsarm (N-05) durch hermetisch verschlossenes, IPx4-Dichtkonzept; **keine** wartungsbedürftigen Elemente (kein Wasser/Ausgleich). Nachteil nur bei Zelltausch — Gehäuse als nicht-reparables Drop-in-Pack ausgelegt (Zielgruppenakzeptanz) |
| **Testbarkeit** | BMS-Herstellertest (Zellspannungs-/Strom-/Temperaturprüfung) gut; Schutzfunktionen einzeln und als System testbar; Ladepfad-Netzteil prüfbar; **Bedingung:** Verifikationskriterien aus SYS.2 (F-01…S-08) auf Baugruppenebene aufteilbar — in SYS.4 zu verankern |
| **Wiederverwendbarkeit** | PE-04/PE-05-Laderegelung und PE-09-BMS als eigenständige, wiederverwendbare Schaltung unterstützen Solar- und Netzvariante; PE-08-Störschutz prinzipiell wiederverwendbar |
| **Technologie-Austauschbarkeit** | **Bewusst eingeschränkt:** Zellblock (PE-01) zentral für Volumen/Gewicht/Spannungsprofil; Austausch AGM/NiMH zieht E-N1…E-N8 nach sich. PE-04/PE-09 parametrierbar (Ladeprofil/DoD-Schwellen), unterstützt Technologiewechsel **auf logischer/** Topologie-Ebene — die Gehäuse-/Volumeninvestition (PE-11) ist aber technologieabhängig |

### 7.2 Risiken

| # | Risiko | Schwere | Bemerkung / Milderung |
|---|--------|---------|------------------------|
| R-1 | **31 Ah im Drop-in-Fach nicht realisierbar** (Volumenkonflikt N-01 ↔ F-04) | Hoch → **GESCHLOSSEN (2026-09-13)** | **Mit Pack-Obergrenze 3,58 l (185 × 155 × 125 mm) für LiFePO4 entschärft** (Innenvolumen ≈ 3,0–3,2 l vs. Zellbedarf ≈ 1,0–1,2 l); **Fach-Vermessung M2 bestätigt das Drop-in**; Zelltopologie-Optimierung (Parallelbündel/Prismatik) in Phase P |
| R-2 | **Spannungsfenster der Zielgeräte unkompatibel mit 3S-Profil** (9,6 V vs. Gerät; OQ-05) | Hoch → **GESCHLOSSEN: kein Abweichungsfall** | Messung am realen Gerät (M1, docs/06) **BESTÄTIGT (2026-09-13)** das 3S-Profil → Risiko geschlossen |
| R-3 | Ladezeitziel ≤ 12 h bei 298 Wh (→ ≥ 30 W ≈ C/10, plus CV-Abschlussblock/Derating) kollidiert mit Zell-, BMS- und Gehäuse-Thermo (N-08) | Mittel | Laderate gegen N-02 (Laden +0…+40 °C) und S-05 abstimmen; Wärmepfade in PE-11 berücksichtigen; ≤ 12 h-Nachweis inkl. CV und Derating (N-08, docs/01) |
| R-4 | Dauerbetrieb am echten Gerät: Schutzlogik löst unter Lastpulsen fehl aus (F-02 ↔ S-03) | Mittel | Impulstabelle aus Reihenmessung; Parametrierung PE-09-Schwellen + PE-08-Entkopplung; Verifikationskriterium Oszilloskop (SYS.4) |
| R-5 | Störfestigkeit vs. Zaun-Hochspannungsimpulse (F-07) führt zu Fehlauslösungen oder Ladezustandsverlust | Mittel | PE-08-Beschaltung + Layout; Dauertest ≥ 72 h (Zielwert) |
| R-6 | BMS-Eigenverbrauch reduziert Laufzeit (N-01/N-04) | Gering–Mittel | Eigenverbrauch < 1 mA Referenz; Berechnung in 504-h-Bilanz |
| R-7 | Zielkosten > 120 EUR bei LiFePO4-BOM + Netzteil-Set (N-07/OQ-07) | Mittel (Business) | Zielkosten-Bewertung in Phase-P-Review; ggf. Solar-Variante als Aufpreis, Set-Preispolitik |
| R-8 | Normrahmen (OQ-11) unbestimmt → Zertifizierungspfad offen (S-06/S-08/N-03) | Mittel (Prozess) | Normauswahl vor Zertifizierungsplan; EN 62133-Zellabuse und VDE/EN-Netzteil in SYS.4 verankern |

### 7.3 Entscheidungen / offene Punkte dieser Phase

| Punkt | Status |
|-------|--------|
| Referenzrealisierung LiFePO4-3S (9,6 V) als Planungsstand | **ENTSCHEIDEN (2026-09-13): LiFePO4 3S** — Referenz, final freigegeben (docs/01 §8.1) |
| Laderegler-Konstruktionsort | **ENTSCHEIDEN (2026-09-13): PE-04 im externen Ladegerät K1** (Netzteil-Gehäuse), Pack reglerfrei — nur Ladebuchse PE-02 + Pack-Zuleitung PE-03 im Pack (Auftraggeber-Vorgabe) |
| Netzseitige Trennung im externen, mitgelieferten Netzteil; Pack führt keine 230 V (PE-02/PE-03) | **Referenzentscheidung** — Alternative (In-Pack-Wandler) nur als E-N6 notiert |
| Ladeschnittstelle Referenz: SELV ca. 12–15 V (Koaxial-/Hohlstecker-Klasse) | Referenzparametrierung, offen bis Norm-/Netzteillösung (OQ-11) |
| Pack-Maximalmaße B 185 × H 155 × T 125 mm (OQ-12, Auftraggeber-Vorgabe) | **Gesetzt + BESTÄTIGT (2026-09-13, M2)** — bindende Obergrenze für PE-01-Topologie und PE-11-Geometrie; Formpassung im realen Batteriefach verifiziert |
| Spannungsmessung der Zielgeräte (OQ-05) | **BESTÄTIGT (2026-09-13, M1)** — 3S-Profil am realen Gerät verifiziert, F-01-Zielwert fixiert |
| Solar-Panel-Spezifikation (OQ-06) | offen (bedingt PE-04-MPP-Konfiguration) |
| Modus „Laden bei montiertem Pack" (OQ-04) | offen (bedingt Gehäusekonzept PE-11/Buchsenzugang und Zustandsmodell) |
| Zielkosten-Freigabe ≤ 120 EUR gegen LiFePO4-BOM (OQ-07) | offen (Business-Review) |

---

## 8. Zweistufige Traceability (Anforderung → Logisch → Physisch)

| Anforderung | Logisches Element | Physisches Element | Status / Lücke |
|-------------|-------------------|--------------------|----------------|
| F-01 | LE-H1, LE-H5 | PE-01, PE-07 (← LE-H5) | **BESTÄTIGT (2026-09-13, M1/OQ-05):** 3S-Profil am realen Gerät verifiziert — deckend |
| F-02 | LE-H1, LE-S1, LE-S3 | PE-01, PE-06, PE-09 | deckend; Puls-/Fehlerfall-Abgrenzung in PE-09 parametrieren |
| F-03 | LE-H5, LE-M1 | PE-07, PE-11 | deckend |
| F-04 | LE-M1 | PE-11 | **BESTÄTIGT (2026-09-13, M2/OQ-12):** Formpassung ≤ 185 × 155 × 125 mm im realen Batteriefach — deckend |
| F-05 | LE-H2, LE-S2, LE-S1 | PE-02, PE-03 (← LE-H2), PE-04 (← LE-S2), PE-09 (← LE-S1, BMS, KN-1) | deckend; **offen:** OQ-04 (Laden bei montiertem Pack) |
| F-06 | LE-H3, LE-S2 | PE-05, PE-04 | deckend; **offen:** OQ-06 (Panel-Spez) |
| F-07 | LE-H6 | PE-08 | deckend |
| N-01 | LE-H1, LE-S1 | PE-01, PE-09 | deckend; **Abschaltgrenze (DoD) fixiert: ≤ 2,5 V/Zelle ≈ 80 % DoD** |
| N-02 | LE-H1, LE-S5 | PE-01, PE-09, PE-10 | deckend; **Kältegrenzwerte fixiert: −10…+40 °C / +0…+40 °C** |
| N-03 | LE-M1 | PE-11 | deckend (IPx4 verbindlich) |
| N-04 | LE-H1 | PE-01 | deckend; Klemmenselbstentladung LiFePO4-Referenz orientiert ≤ 5 %/Monat |
| N-05 | LE-H1, LE-M1 | PE-01, PE-11 (← LE-M1) | deckend (kein Wartungselement) |
| N-06 | LE-M1 | PE-11 | deckend |
| N-07 | LE-H1, LE-S1 | PE-01, PE-09 | **teilw. Lücke:** Zyklusziel → PE-01/PE-09 (DoD); **Kosten-/Amortisationsziel = Business Case, kein physisches Element** → OQ-07 |
| N-08 | LE-H2, LE-S2, LE-S1 | PE-02, PE-04 (← LE-S2), PE-09 (← LE-S1, BMS, KN-1) | deckend; Laderate/Kennlinie Phase-P-Detail |
| S-01 | LE-S1, LE-H4 | PE-09, PE-06 | deckend (zwingend, R8) |
| S-02 | LE-S2, LE-H2 | PE-04 (← LE-S2), PE-09 (← LE-S2, redundant, KN-1), PE-02/PE-03 (← LE-H2) | deckend (redundante Überlade-Abschaltung) |
| S-03 | LE-S3, LE-H4 | PE-09, PE-06 | deckend |
| S-04 | LE-S4, LE-H5 | PE-09, PE-07 | deckend (Verpolungserkennung + mech. Sicherung) |
| S-05 | LE-S5 | PE-09, PE-10 | deckend |
| S-06 | LE-H2 | PE-02, PE-03 (+ externes Netzteil als Set-Bestandteil) | deckend, **aber Interpretations-/Normpunkt:** Isolation realisierend im mitgelieferten externen Netzteil; Normauswahl OQ-11 offen |
| S-07 | LE-M2 | PE-12 | deckend |
| S-08 | LE-M1, LE-H1 | PE-11, PE-01 | deckend; technologiespez. Abuse-Verhalten abhängig von Technologie (Referenz LiFePO4 intrinsisch stabil; S-05/S-08-Norm OQ-11) |

> **KN-1 · Konsolidierungsnotiz PE-09 (BMS-Schutzmodul, verlinkt auf Abschnitt 2/3.2):**
> PE-09 konsolidiert die logischen Schutzlogik-Elemente **LE-S1** (Tiefentlade-Schutzlogik),
> **LE-S3** (Kurzschluss-/Überstrom-Schutzlogik), **LE-S4** (Verpolungs-Schutzlogik) und
> **LE-S5** (Thermisches Managementsystem) in einer physischen Baugruppe — Ursprünge in
> `02_spezifikation_architektur_logisch.md`, Abschnitt 3.2. In den Lade-Anforderungszeilen
> (F-05, N-08) steht PE-09 für den BMS-Schutzaufsichtskanal im Ladepfad (Bezugs-LE LE-S1,
> konsolidiert S3/S4/S5); bei S-02 realisiert PE-09 die **redundante** Überlade-Abschaltung
> und ist zu LE-S2 (Laderegelung/Überladeschutz) rückführbar. Ergänzende Ziele: PE-04
> (Laderegler) ← LE-S2, PE-02/PE-03 ← LE-H2, PE-07 ← LE-H5, PE-11 ← LE-M1.

**Deckungsstatistik:** 23/23 Anforderungen zweistufig zuordenbar. **Voll deckend: 22**.
**Bedingt/offen: 1** — N-07 (OQ-07, Kostenziel ohne physisches Element; Business-Freigabe offen).
Weitere nicht-blockierende Offenheiten: F-05/OQ-04, F-06/OQ-06, S-06/OQ-11 (F-01/OQ-05 und F-04/OQ-12 sind bestätigt).

---

## 9. Offene Punkte für das Phase-P-Review (Technologieblockaden & Entscheidungen)

| # | Offener Punkt | Blockiert | Detail |
|---|---------------|-----------|--------|
| 1 | ~~Spannungsfenster der Zielgeräte messen (OQ-05)~~ | ~~finale Bestätigung~~ | **GESCHLOSSEN (2026-09-13, M1):** 3S-Profil am realen Gerät **BESTÄTIGT** — LiFePO4-3S (F-01) fixiert |
| 2 | ~~Fachgeometrie der Referenzgeräte bestätigen (OQ-12)~~ | ~~Zelltopologie/Gehäuse/Drop-in~~ | **GESCHLOSSEN (2026-09-13, M2):** Formpassung im realen Batteriefach **BESTÄTIGT** — Zelltopologie/PE-11-Design tragfähig |
| 3 | **Technologieentscheidung (LiFePO4 vs. AGM vs. NiMH)** | ~~PE-01/PE-04/PE-09/PE-11/PE-12 + Zielwerte~~ | **GESCHLOSSEN (2026-09-13): LiFePO4-3S final; Zielwerte in docs/01 §8.1 fixiert** — PE-04/PE-09-Parametrierung (CC/CV, Abschaltgrenze) auf dieser Basis |
| 4 | **Normrahmen (OQ-11)** | Zertifizierung S-06/S-08/N-03; Zellabuse-, IP- und Transportprüfungen | Auswahl EN 62133, VDE/EN, IEC 60529, UN3480/3481 (LiFePO4) |
| 5 | **Laden bei montiertem Pack (OQ-04)** | Gehäusekonzept/Ladebuchsen-Zugänglichkeit, Zustandsmodell | Designfreigabe PE-11/PF-Zuordnung |
| 6 | **Solar-Panel-Spezifikation (OQ-06)** | PE-05/PE-04-MPP-Konfiguration, Varianten-BOM | Panel-Spannungs-/Leistungsklasse + Kompatibilitätsnachweis |
| 7 | **Zielkosten 120 EUR (OQ-07)** | Business-Freigabe LiFePO4-BOM | Zielkostenberechnung Pack + Netzteil-Set; Solar-Aufpreis |
| 8 | **Impuls-/Pulsmuster-Tabelle (F-02/F-07-Reihenmessung)** | Parametrierung PE-09-Schwellen/PE-08 | Zeit-/Strommuster aus Reihenmessung; Verifikation SYS.4 |

**Nächste Schritte:** Bestätigungs-Messungen M1/M2 **erledigt (2026-09-13)** →
Detail-Entwurf je Komponente (**K1** Ladegerät mit Laderegler, **K2** Zelltopologie/
BMS/Layout/Gehäuse, **K3** Solar-Panel + Solar-Eingang im Ladegerät) → Baugruppen-BOM je K →
SYS.4-Verifikationsplan (auf K1/K2/K3-Basis).
Ein Technologiewechsel (E-N1…E-N8) ist verworfen und wäre nur nach neuer
Entscheidungsrunde möglich.

---

## 10. Formale Hinweise

- 13 logische Elemente → **12 physische Elemente** (Konsolidierung von LE-S1/S3/S4/S5
  in PE-09 BMS; LE-H2 realisiert durch PE-02 + PE-03; LE-H3 → PE-05; LE-H5-Montage
  in PE-07/PE-11).
- **Baugruppen-Gliederung K1/K2/K3** gemäß Abschnitt 1.4: K1 Ladegerät mit Laderegler
  (PE-02/03/04 + externes Netzteil), K2 Batterie mit Zellen und BMS (PE-01, PE-06…PE-12),
  K3 Solarmodul optional (PE-05).
- Vollständige zweistufige Traceability in Abschnitt 8; Lücken und Offenheiten
  explizit referenziert (OQ-04…OQ-12).
- Alle physischen Entscheidungen sind auf logische Elemente rückführbar (Punkt 4
  des Base-Practice-Skills eingehalten).
- Keine Lieferanten-/Bauteilmarken-Festlegung; nur **Bauteil-Klassen** (Phase-P-Detail
  nach Technologie- und Normentscheid).
- Die Technologieentscheidung ist **final: LiFePO4-3S** (2026-09-13, docs/01 §8.1);
  die ausgearbeiteten AGM-/NiMH-Abweichungen sind als Einfluss-Notizen E-N1…E-N8
  archiviert.