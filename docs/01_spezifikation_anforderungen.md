# System-Anforderungsspezifikation (ASPICE SYS.2)
## Wiederaufladbares 9V-Akku-Pack für Weidezaungeräte

| | |
|---|---|
| Prozess | SYS.2 – System Requirements Analysis (Systemebene) |
| Eingangsartefakt | `00_marktanalyse.md` (Stakeholder-Requirements-Grundlage) |
| Ausgangsartefakt | Diese Spezifikation (System-Requirements) |
| Nachfolgeprozess | SYS.3 (Systemarchitektur) — Elementtypen: Hardware, Software (BMS), Mechanik |
| Stand | 2026-09-13 · SYS.3-Technologieentscheidung: **LiFePO4 3S bestätigt**; Pack-Maximalmaße 185 × 155 × 125 mm fixiert |
| Status | Anforderungen atomar, kategorisiert, verifizierbar; **Teilentscheidungen fixiert (R9–R13, R14 = LiFePO4 3S); technologiespezifische Zielwerte festgelegt** |

---

## 1. Zweck & Geltungsbereich

Das Produkt ist ein **reines Akku-Pack** (ca. 9 V) als **Drop-in-Ersatz** für
9V-Trockenbatterien (Zink-Kohle/Alkaline) in bestehenden batteriebetriebenen
Weidezaungeräten der 9V-Klasse (Gallagher BA-Serie, Koltec, AKO, o.ä.).

Das Weidezaungerät selbst ist **nicht Bestandteil der Entwicklung**. Das Pack
versorgt das Gerät elektrisch. Gegenstand dieser Spezifikation ist ausschließlich
das Akku-Pack inkl. Schutzschaltung, Ladefunktion, Gehäuse und Kennzeichnung.

Der Prozessbericht obliegt nicht dem Requirements-Agenten (SYS.2-Artefakt).

---

## 2. Nicht verhandelbare Produktrahmendaten

Diese Rahmendaten sind vom Auftraggeber vorgegeben und gelten als verbindlich:

| # | Rahmenbedingung |
|---|---|
| R1 | Produkt ist ein reines Akku-Pack, ca. 9 V, Drop-in-Ersatz für 9V-Trockenbatterien in bestehenden 9V-Weidezaungeräten (Gallagher BA-Klasse, Koltec, AKO, o.ä.) |
| R2 | Das Pack speist das Weidezaungerät (nicht das Gerät selbst entwickeln) |
| R3 | Zielgruppe: Landwirt, mittlere Weiden ca. 1–3 km Zaunlänge, mittlerer Bewuchs |
| R4 | Land: Deutschland (Funkenwirkung, Outdoor, Witterungsschutz, IP-Klassifikation relevant) |
| R5 | Laden über 230V-Netzteil und optional Solarmodul |
| R6 | Ziel-Laufzeit ohne Nachladen: 2–4 Wochen (bei typischem Verbrauch 30–60 mA) |
| R7 | Referenz-Verbrauchswerte: BA30 = 34 mA/9 V, BA40 = 43 mA/9 V, Koltec EC20 = 30 mA/9 V |
| R8 | Maximale nutzbare Entladetiefe → **Tiefentladeschutz zwingend** für Zyklenlebensdauer |
| R9 | **Entschieden (Auftraggeber):** Betriebspunkt Laufzeit = BA40 @ 43 mA, 3 Wochen → Nennkapazität ≥ 31 Ah |
| R10 | **Entschieden:** striktes Drop-in in das Batteriefach eines Referenzgeräts; externes Pack nicht zulässig; Fachgeometrie-Vermessung = SYS.3-Milestone |
| R11 | **Entschieden:** Witterungsschutz IPx4 (DIN EN 60529) verbindlich |
| R12 | **Entschieden:** Amortisation ≥ 6 Nutzsaisons; Verkaufspreis-Obergrenze 120 EUR |
| R13 | **Entschieden:** Ladezeit Netz ≤ 12 h („über Nacht“) |
| R14 | **Entschieden (2026-09-13, SYS.3):** Zelltechnologie = **LiFePO4 3S** (9,6 V nominal, 7,5–10,95 V); alle technologiespezifischen Zielwerte festgelegt (§8.1); Pack-Maximalmaße 185 × 155 × 125 mm |

---

## 3. Stakeholder-Requirements (Eingangsbasis, abgeleitet aus Marktanalyse)

Für die Traceability werden die Stakeholder-Anforderungen aus `00_marktanalyse.md`
retroaktiv mit IDs belegt:

| SR-ID | Stakeholder-Anforderung | Quelle (Marktanalyse) |
|-------|--------------------------|------------------------|
| SR-01 | Das Pack ist direkter Drop-in-Ersatz für 9V-Trockenbatterien in bestehenden 9V-Weidezaungeräten (Gallagher BA, Koltec, AKO) | §1, §2, §7 |
| SR-02 | Das Pack speist das Weidezaungerät; kein Gerätekauf/-umbau beim Anwender; Geräteneubau nicht Teil des Produkts | §1, §5 |
| SR-03 | Zielgruppe ist der Landwirt mit mittleren Weiden (1–3 km Zaun, mittlerer Bewuchs) | §5 |
| SR-04 | Laufzeit ohne Nachladen: Ziel 2–4 Wochen | §5, §7 |
| SR-05 | Wiederaufladbar über 230V-Netzteil | §5 |
| SR-06 | Optional aufladbar über Solarmodul | §5 |
| SR-07 | Robust, wetterfest (Outdoor, Deutschland), wartungsarm | §5, §7 |
| SR-08 | Kosten: Amortisation gegenüber Einweg-Trockenbatterien wichtig | §5, §2 (30–80 EUR/Saison) |
| SR-09 | Betrieb mit vorhandenen Geräten ohne Gerätekauf | §5 |
| SR-10 | Das Pack muss das Verbrauchsspektrum der Referenzgeräte decken (30–43 mA @ 9 V; Betriebsfenster aus §4) | §4 |
| SR-11 | Technologieentscheidung (LiFePO4 3S vs. AGM vs. NiMH) **entschieden: LiFePO4 3S** — begründet per Entscheider-Review in SYS.3 (Abschnitt 8.1) | §6, §7 |
| SR-12 | Marktlücke: kein tauschbares wiederaufladbares 9V-Pack am Markt; Differenzierung über Drop-in, Zyklenfestigkeit, Preis/Laufzeit | §3, §8 |

---

## 4. Systemanforderungen

### 4.1 Kategorien-Legende

| Kürzel | Kategorie |
|--------|-----------|
| **F** | funktional |
| **N** | nicht-funktional |
| **S** | sicherheitsrelevant |

Hinweis: „[offen]“ in einer Anforderung kennzeichnet einen inhaltlichen Bezug auf
eine noch offene Entscheidung (siehe Abschnitte 7 und 8). Die Anforderung bleibt
atomar und verifizierbar; der Zielwert wird nach Entscheidung fixiert. Bereits
fixierte Zielwerte sind explizit als solche benannt („fixiert“, siehe Abschnitt 7.1).

---

### 4.2 Funktionale Anforderungen (F)

| ID | Anforderung (atomar) | Kategorie | Verifikationskriterium | Trace |
|----|-----------------------|-----------|------------------------|-------|
| SYS-REQ-F-01 | **Betriebsspannung:** Das Pack versorgt die Referenzgeräte Gallagher BA30, BA40 und Koltec EC20 über den gesamten Entladebereich mit einer Klemmenspannung innerhalb des von den Geräten tolerierten Eingangsspannungsbereichs. **Festgelegtes Profil (LiFePO4 3S): nominal 9,6 V, Entladebereich ca. 7,5–10,95 V (2,5–3,65 V/Zelle)**. OQ-05 (Messung am realen Gerät) **BESTÄTIGT (2026-09-13, M1)** das Spannungsfenster. | F | Prüfstand: Entladezyklus am realen Referenzgerät; Kriterium: Gerät erzeugt über die gesamte Laufzeit bis zur Abschaltgrenze ununterbrochen Zaunimpulse, keine frühzeitige Unterspannungsabschaltung. | SR-01, SR-02 (Scope-Verankerung), SR-09, SR-10, SR-04; SR-11 (Zielwerte festgelegt) |
| SYS-REQ-F-02 | **Puls-Lastfähigkeit:** Das Pack liefert den pulsartigen Laststrom 9V-Weidezaun-typischer Ladebausteine (Effektivlast 30–60 mA, Spitzenströme mehrfach darüber) ohne frühzeitige Auslösung der Schutzschaltung; die Klemmenspannung bleibt während jedes Pulses über der Geräte-Abschaltgrenze. | F | Oszilloskop-Messung von Strom-/Spannungs-Zeitprofil am Gerät; Kriterium: Schutzschaltung löst während definierten Worst-Case-Pulsmusters (aus Reihenmessung, Zielwert [offen]) nicht aus; Spannung sinkt nicht unter Geräteschwelle. | SR-10, SR-04, SR-09 |
| SYS-REQ-F-03 | **Drop-in-Elektrik:** Kontaktierung und Polung entsprechen dem Anschlussbild der 9V-Trockenbatterie in den Zielgeräten; das Pack wird ohne Adapter oder Umbau elektrisch angeschlossen und verriegelt im Batteriefach. | F | Passtest + Kontaktwiderstandsmessung an Referenzgeräten; Kriterium: Kontaktwiderstand ≤ 50 mΩ je Kontakt, reproduzierbare Verriegelung, keine Kabelbrücken. | SR-01, SR-12, SR-09 |
| SYS-REQ-F-04 | **Drop-in-Mechanik (strikt):** Das Pack passt mechanisch formschlüssig in das Batteriefach eines Referenzgeräts (Gallagher BA30/BA40 oder Koltec EC20); Einbau ohne Adapter, Umbau oder Kabelbrücke. Ein extern verlegtes Pack ist als Abweichung **nicht zulässig**. **Maximalmaße Pack: B 185 mm × H 155 mm × T 125 mm (3,58 l Gesamtvolumen)** — dies ist die verbindliche Obergrenze für das Pack (inkl. Gehäuse). Die finale Größen-/Gewichtsfixierung erfolgt je Zelltopologie in Phase P. Die Fachgeometrie-Vermessung der Referenzgeräte (OQ-12) **BESTÄTIGT (2026-09-13, M2)**, dass das Pack in das jeweilige Referenzgerät passt. | F | CAD-Passung und physischer Passtest am Referenzgerät; Kriterium: vollständige formschlüssige Einpassung ins Batteriefach ohne Adapter/Kabelbrücken, Deckel/Fach schließt plan, kein Kontaktverbau. Pack-Maße ≤ 185 × 155 × 125 mm. | SR-01, SR-12, SR-09 |
| SYS-REQ-F-05 | **Netzladung:** Das Pack ist mit dem mitgelieferten 230V-Netzteil vollständig aufladbar (bis Erreichen des Ladeendekriteriums), ohne dass das Weidezaungerät eingriffen oder entfernt werden muss (Modus „Laden bei montiertem Pack“ [offen: OQ-04]). | F | Vollständiger Ladezyklus mit Seriennetzteil; Kriterium: Ladung erreicht ≥ 95 % der Nennkapazität (Coulomb-Messung bzw. Ladeendekriterium), Ladezeit ≤ Zielwert [offen]. | SR-05 |
| SYS-REQ-F-06 | **Solarladung (Option, fixiert als Variante):** In der optionalen Variante (R5) ist das Pack über ein anzugebendes Solarmodul aufladbar; die Laderegelung verträgt das wechselnde Solar-Lastprofil (Einstrahlung, Temperatur) ohne Schutzabschaltung oder Zellschädigung. Der Kompatibilitätsnachweis ist für diese Variante verpflichtend; die Panel-Spezifikation bleibt offen [offen: OQ-06]. | F | Solar-Ladeprüfung mit definiertem Panel-Modell unter definiertem Einstrahlungs-/Temperaturprofil; Kriterium: Ladung auf ≥ 90 % Nennkapazität innerhalb des Definitionstags, keine Fehlabschaltung. | SR-06 |
| SYS-REQ-F-07 | **Impuls-/Störfestigkeit (Funkenwirkung):** Das Pack ist unempfindlich gegenüber den durch das Weidezaungerät erzeugten elektrischen Impulsen und Funken (kapazitive/induktive Rückwirkungen an den Batterieklemmen); das Pack bleibt betriebs- und lagerungsfähig und verliert keinen spezifizierten Ladezustand durch Störimpulse. | F | Dauerbetrieb am realen Gerät über definierte Betriebszeit (Zielwert [offen], Vorschlag ≥ 72 h) mit Klemmenspannungs-/Strom-Messung; Kriterium: keine Fehlfunktion, keine Schutzabschaltung, Ladezustandsverlust ≤ Toleranz. | SR-04, SR-09, R4 (Funkenwirkung) |

---

### 4.3 Nicht-funktionale Anforderungen (N)

| ID | Anforderung (atomar) | Kategorie | Verifikationskriterium | Trace |
|----|-----------------------|-----------|------------------------|-------|
| SYS-REQ-N-01 | **Laufzeit ohne Nachladen:** Das Pack liefert nach voller Ladung bei fixierter Referenzlast BA40 (43 mA, 9V-Profil) eine Betriebsdauer von mindestens 3 Wochen (≥ 504 h) bis zum Erreichen der Abschaltgrenze (Tiefentladeschutz). **Fixierter Zielwert: Nennkapazität ≥ 31 Ah** (Herleitung 43 mA × 504 h = 21,7 Ah nutzbar ÷ 0,7 Nutzungsfaktor ≈ 31 Ah). | N | Zeitmessung im Entladeprüfstand mit konstanter 43-mA-Referenzlast (BA40-Profil) bis Schutzabschaltung; Kriterium: Betriebsdauer ≥ 504 h bzw. entnommene Kapazität ≥ 21,7 Ah bis Abschaltung. | SR-03, SR-04, SR-10 |
| SYS-REQ-N-02 | **Betriebstemperaturbereich (Deutschland):** Entladen ist im Bereich −10…+40 °C, Laden im Bereich +0…+40 °C funktionsfähig; **Grenzwerte für LiFePO4 3S bestätigt (Entladen −10…+40 °C, Laden +0…+40 °C)**. | N | Klimakammer-Test: Entlade-/Ladeversuche an den Grenztemperaturen; Kriterium: volle spezifizierte Leistung ohne Schutzabschaltung, keine bleibende Kapazitätsminderung > Toleranz. | SR-03, SR-07, R4; SR-11 (Zielwerte festgelegt) |
| SYS-REQ-N-03 | **Witterungsschutz / IP-Klassifikation:** Das Pack ist für Outdoor-Betrieb in Deutschland geeignet; Gehäuse und Anschlüsse sind gegen Eindringen von Regen-/Spritzwasser und Staub geschützt. **Festgelegte IP-Klasse: IPx4 (DIN EN 60529), verbindlich.** | N | IP-Prüfung nach DIN EN 60529 (IPx4) am Muster; Kriterium: Bestehen der IPx4-Prüfung, Funktionskontrolle nach Prüfung. | SR-03, SR-07, R4 |
| SYS-REQ-N-04 | **Selbstentladung / Ladezustandserhaltung:** Ohne Last hält das Pack einen spezifizierten Mindestladezustand über eine Lagersaison (≥ 6 Monate), sodass nach winterlicher Lagerung ohne Nachladen ein Einsatz möglich ist. **Festgelegter Zielwert (LiFePO4): Selbstentladung ≤ 5 %/Monat ⇒ Mindestladezustand nach ≥ 6 Monaten ≥ 70 % (≥ 21,7 Ah nutzbar ≈ 3 Wochen Betrieb analog N-01)**. | N | Langzeitlagerungstest (≥ 6 Monate); Kriterium: Ladezustand nach Lagerung ≥ definierter Mindestwert, danach Betrieb möglich. | SR-07; SR-11 (Zielwerte festgelegt) |
| SYS-REQ-N-05 | **Wartungsarmut:** Das Pack erfordert über eine Saison hinweg keine Wartungsmaßnahmen (kein Wasser/Ausgleich nachfüllen, kein planmäßiges Entladen). **Für LiFePO4 3S bestätigt: wartungsfrei, keine Pflegeausnahme.** | N | Praxistest über ≥ 3 Monate mit Wartungsprotokoll; Kriterium: keine präventiven Wartungseingriffe nötig, Funktion erhalten. | SR-07 |
| SYS-REQ-N-06 | **Robustheit (Sturz, Vibration, Handling):** Das Pack übersteht typische landwirtschaftliche Belastungen (Sturz aus 1 m auf Beton, Vibration am Gerät/Transport, mechanisches Handling) ohne Funktionsverlust und ohne Sicherheitsgefährdung. | N | Falltest nach DIN EN 60068-2-31, Vibrationstest nach DIN EN 60068-2-6; Kriterium: keine Risse/Deformation, Funktion und Isolation vollständig erhalten — Messung der Boden-/Stoßbeanspruchung nach Norm. | SR-03, SR-07 |
| SYS-REQ-N-07 | **Zyklenlebensdauer / Amortisation:** Das Pack erreicht **mindestens 6 Nutzsaisons** (fixiert). **Verkaufspreis-Obergrenze: 120 EUR** (fixiert, Bezug 2026). Die Amortisation ist gegenüber 9V-Einweg-Trockenbatterien (30–80 EUR/Saison, §2 der Marktanalyse) nachzuweisen. **Festgelegtes Zyklusziel (LiFePO4): ≥ 2000 Zyklen bis 80 % Restkapazität bei 80 % DoD → 6 Saisons erreichbar.** | N | Zyklentest unter definierter Entladetiefe bis Erreichen von 80 % Restkapazität; Kriterium: Lebensdauer ≥ 6 Nutzsaisons im Prüfszenario; Amortisationsrechnung gegen Einweg-Referenzkosten; Zielkostenprüfung ≤ 120 EUR (Business Case). | SR-08, SR-12; SR-11 (Zielwerte festgelegt) |
| SYS-REQ-N-08 | **Ladezeit (Netz):** Die Volladung über das 230V-Netzteil erfolgt innerhalb von höchstens **12 h** („über Nacht“, fixiert). **Für LiFePO4 3S mit CC/CV-Ladung (C/10 ≈ 3,1 A, CV 3,65 V/Zelle) verträglich bestätigt; der 12-h-Nachweis ist inkl. CV-Abschlussblock und Temperatur-Derating zu führen (N-08, siehe docs/03).** | N | Ladedauer-Messung bis Ladeendekriterium; Kriterium: Ladedauer ≤ 12 h bei Nennstartbedingungen. | SR-05 |

---

### 4.4 Sicherheitsrelevante Anforderungen (S)

| ID | Anforderung (atomar) | Kategorie | Verifikationskriterium | Trace |
|----|-----------------------|-----------|------------------------|-------|
| SYS-REQ-S-01 | **Tiefentladeschutz (zwingend):** Das Pack verhindert Entladung unterhalb der maximalen nutzbaren Entladetiefe; bei Erreichen der Abschaltgrenze wird die Last getrennt bzw. der Strom auf vernachlässigbaren Bereich begrenzt, ohne dauerhafte Zellschädigung entlang der Zyklenlebensdauer. **Festgelegte DoD-Grenze (LiFePO4): Abschaltgrenze ≤ 2,5 V/Zelle (7,5 V Pack unter Last) ≈ 80 % DoD als Zyklen-Referenz.** | S | Entladeversuch bis zur Schutzabschaltung + Folgeladung; Kriterium: Abschaltgrenze eingehalten, Kapazitätsrückkehr nach Folgezyklus ≥ 95 %, Reproduzierbarkeit über ≥ 10 Zyklen. | SR-04, SR-07, R8; SR-11 (Zielwerte festgelegt) |
| SYS-REQ-S-02 | **Überladeschutz / Laderegelung:** Das Pack begrenzt die Zellspannung und beendet die Ladung bei Erreichen der vollen Ladung; eine Überschreitung der maximal zulässigen Zellspannung und eine Schädigung durch Überladung werden sicher verhindert. | S | Ladeprüfung mit verlängerter Ladung (≥ +10 % Dauerstrom) über dem Ladeende; Kriterium: Spannung bleibt unter dem Zellgrenzwert, keine Delamination/Deformation/Übertemperatur; Abbruchkriterium greift zuverlässig. | SR-05, SR-07 |
| SYS-REQ-S-03 | **Kurzschlussfestigkeit:** Das Pack übersteht einen Kurzschluss an den Ausgangsklemmen ohne Brand-, Verletzungs- oder Sachgefahr; Schutzschaltung unterbricht oder begrenzt den Fehlerstrom. | S | Kurzschlusstest mit Leistungseinspeisung; Kriterium: keine Entzündung, keine Glut, Gehäuse-/Klemmentemperatur unter Grenzwert, Funktion nach Entfernen des Fehlers wiederherstellbar. | SR-07 |
| SYS-REQ-S-04 | **Verpolungsschutz:** Eine verpolte Verbindung am Weidezaungerät bzw. Ladepfad führt zu keiner Zellschädigung, keiner Brandgefahr und keiner Fehlfunktion; das Pack bleibt nach korrekter Wiederanschaltung funktionsfähig. | S | Verpolungstest (kurzzeitige, definierte Fehlschaltung); Kriterium: keine irreversiblen Schäden, Schutz greift, Funktion danach intakt. | SR-07, SR-09 |
| SYS-REQ-S-05 | **Thermomanagement / Übertemperaturschutz:** Das Pack stoppt Laden und Entladen bei Übertemperatur und verhindert thermisches Durchgehen unter Einfachfehler-Bedingungen (Überladung, äußerer Wärmeeinfluss, Kurzschluss). **LiFePO4-Referenz-Schwellen: Ladestopp ≥ 50 °C Zelltemperatur, Entladestopp ≥ 60 °C (Fixierung je Zell-Datenblatt in Phase P)**; Normen-/Testernahmen folgt OQ-11. | S | Thermoschutzprüfung (Aufheizversuch, Temperaturmessung); Missbrauchstest nach geeigneter Zellnorm; Kriterium: Temperatur bleibt unter kritischem Schwellwert, kein thermisches Durchgehen (LiFePO4 intrinsisch stabil). | SR-07; (OQ-11 offen) |
| SYS-REQ-S-06 | **Elektrische Sicherheit Netzladung:** Zwischen 230V-Netzpfad und Niederspannungs-/Zellseite besteht sichere Trennung (Isolation/Isolationsbarriere); das Pack und das Netzteil erfüllen die einschlägigen Anforderungen (VDE/EN, Schutzklasse, Berührschutz). | S | Isolationsspannungs-/Isolationswiderstandsprüfung, Benetzungs-/Berührschutzprüfung; Kriterium: bestanden nach definierter Norm (Normauswahl [offen: OQ-11]), keine durchgängige 230V-Verbindung zur Klemmenseite. | SR-05 |
| SYS-REQ-S-07 | **Kennzeichnung & Dokumentation:** Das Pack trägt dauerhafte Kennzeichnung (Nennspannung, Polung/Anschlussschema, Warn- und Entsorgungshinweise, CE-Kennzeichen, Batteriegesetz/Recyclingkennzeichnung) und wird mit deutschsprachiger Anleitung und Sicherheitshinweisen geliefert. | S | Kennzeichnungs-Audit und Dokumentenprüfung; Kriterium: alle geforderten Kennzeichnungselemente vorhanden und dauerhaft lesbar, Dokumentation vollständig und verständlich (deutsch). | SR-07, SR-08 |
| SYS-REQ-S-08 | **Fehlerverhalten bei mechanischer Beschädigung / Einfachfehler:** Bei Beschädigung (Druck, Stoß, Eindringen) oder Einfachfehler in der Schutzschaltung darf das Pack keine gefährlichen Stoffe freisetzen (Brand, Gas, Säure/Flüssigkeit, berstende Teile) und keine gefährliche Form verändern; Entsorgungs-/Transporteigenschaften der Technologie sind zu berücksichtigen [offen: OQ-11]. | S | Abuise-/Missbrauchsprüfung (Quetsch-, Stoß-, Nadel-/Penetrationstest nach relevanter Zellnorm); Kriterium: keine gefährlichen Freisetzungen, Gehäuseintaktheit, Grenztemperatur eingehalten. | SR-07; (OQ-11 offen; SR-11: LiFePO4 intrinsisch stabil) |

---

## 5. Prüfung der Auswirkungen

### 5.1 Umgebung (Kontext / Bedingungen in Deutschland)

- **Klima:** Jahreszeiten mit Frost, Hitze bis über 40 °C, UV-Strahlung, Feuchtigkeit/Niederschlag — abgedeckt durch N-02, N-03; Winterlagerung → N-04.
- **Outdoor-Weide:** Staub, Schmutz, Bewuchs und mechanische Belastung im Feld; **Tierkontakt (Nagen an Gehäuse/Kabel)** als ungelöstes Risiko → offene Frage OQ-09.
- **Elektromagnetische Umgebung:** Die Hochspannungsimpulse des Weidezaungeräts wirken auf das im Gerät verbaute Pack → F-07.
- **Entsorgung/Recycling:** Batteriegesetz, Recyclingkennzeichnung, ggf. Gefahrgutklassifikation beim Transport (LiFePO4: UN3480/UN3481; AGM: Säure) → S-07, offene Frage OQ-11.

### 5.2 Sicherheit

- **Elektrische Sicherheit:** Netzladepfad erfordert sichere Trennung → S-06; Fehlerfälle Überladung/Kurzschluss/Verpolung → S-02, S-03, S-04.
- **Thermisches Missbrauchsverhalten:** Technologiespezifisch stark unterschiedlich (LiFePO4 intrinsisch stabil — entschieden (OQ-08/SR-11); NiMH/AGM verworfen) → S-05, S-08; Normbezug OQ-11 offen.
- **Funktionale Sicherheit / Tierwohl:** Der Zaun dient dem Tierschutz. Ein **Akku-Ausfall führt nicht zu Personengefährdung**, aber zu **Zaunstrom-Verlust und damit Tiergefährdung** → Laufzeit (N-01), Schutz vor Fehlabschaltung (F-02, F-07) und ggf. Ladezustandsanzeige (OQ-10) sind hier die relevanten Stellhebel. Kein ASIL-Bezug erforderlich, aber Betriebszuverlässigkeit priorisiert.
- **Transport/Logistik:** Gefahrgut-/Transportklassifikation der Zelltechnologie beachten (OQ-11).

### 5.3 Machbarkeit

**Kapazitäts-Herleitung aus dem Laufzeitziel (Rechengang, Marktanalyse §4):**

Nutz-Lasten über das Laufzeitziel 2–4 Wochen (336 h / 672 h):

| Last | 2 Wochen (336 h) | 4 Wochen (672 h) |
|------|------------------|------------------|
| EC20 (30 mA) | 10,1 Ah | 20,2 Ah |
| BA30 (34 mA) | 11,4 Ah | 22,8 Ah |
| BA40 (43 mA) | 14,4 Ah | 28,9 Ah |
| 60 mA (Rahmen-Obergrenze) | 20,2 Ah | 40,3 Ah |

Mit nutzbarer Kapazität ≈ 70 % der Nennkapazität (Marktanalyse §4):

| Betriebspunkt | benötigte Nennkapazität |
|----------------|--------------------------|
| 4 Wochen @ 30 mA | ≈ 29 Ah |
| 4 Wochen @ 43 mA | ≈ 41 Ah |
| 4 Wochen @ 60 mA | ≈ 58 Ah |
| 2 Wochen @ 60 mA | ≈ 29 Ah |
| 3 Wochen @ 43 mA (Praxismitte) — fixierter Betriebspunkt | ≈ 31 Ah |

→ **Fixierter Zielwert (Auftraggeber-Entscheid): Nennkapazität ≥ 31 Ah** auf Basis des
Betriebspunkts BA40 @ 43 mA, 3 Wochen: 43 mA × 504 h = 21,7 Ah nutzbar ÷ 0,7
Nutzungsfaktor ≈ 31 Ah (N-01). Die übrige Tabelle dient nur noch der Dokumentation
des Rahmenbandes. Spannung: 3S-LiFePO4 ≈ 9,6 V nominal; AGM 8V ist spannungsseitig
marginal (F-01-Konflikt).

**Machbarkeits-Konflikt (Formfaktor):** Geltend ist die strikte Drop-in-Anforderung
(F-04) — ein extern verlegtes Pack ist nicht zulässig. Das verfügbare Volumen
**im Pack selbst ist auf B 185 × H 155 × T 125 mm (3,58 l, Innenvolumen
≈ 3,0–3,2 l) fixiert**; darin muss der Technologie- und Zellblock für ≥ 31 Ah
untergebracht werden. **Bestätigt (LiFePO4 3S):** Zellblock ≥ 31 Ah ≈ 1,0–1,2 l /
≈ 2,8–3,3 kg ist im Packvolumen darstellbar — Machbarkeit bejaht (docs/03 §5,
docs/01 §8.1/8.2).

**Technologie-Trade-off (aus Marktanalyse §6) — Entscheidung gefallen: LiFePO4 3S**

**Entscheidung 2026-09-13:** LiFePO4-3S ist die fixierte Referenzrealisierung.
Mit dem Pack-Budget (185 × 155 × 125 mm = 3,58 l, ≤ 3,8 kg) scheidet AGM/Vlies
volumen- und gewichtsbedingt aus (~3,3–5 l / 5–8 kg für 31 Ah); NiMH wäre
volumenmäßig darstellbar, scheitert aber an Selbstentladung (> 20 %/Monat → N-04),
Kälteverhalten (N-02) und Zyklenzahl (N-07, ~500–1000). LiFePO4-3S erfüllt als
einzige Option alle fixierten Zielwerte (N-01, N-04, N-07, F-01, N-03).

**Vergleich (Dokumentationsbasis):**

| Kriterium | LiFePO4 (3S) — **gewählt** | AGM/Vlies | NiMH |
|-----------|--------------|-----------|------|
| Spannung passend 9V | 9,6 V ✓ | 8 V (marginal) | 8,4–9,6 V ✓ |
| Zyklenzahl | 2000+ ✓ | ~500 ✗ | ~500–1000 ✗ |
| Gewicht/Volumen (31 Ah im 3,58-l-Pack) | ✓ (1,0–1,2 l) | ✗ (3,3–5 l) | ◐ (1,5–2,0 l) |
| Selbstentladung (N-04) | gering ✓ | gering ✓ | hoch ✗ |
| Kälteverhalten (N-02) | gut ✓ | gut ✓ | schlecht ✗ |
| Ladung/BMS | BMS zwingend | simpel | simpel |
| Preis/Wh | hoch | niedrig | mittel |
| Missbrauchssicherheit | intrinsisch stabil ✓ | wässrige Säure | nasszell-/gasend |

Die technologiespezifischen Zielwerte sind mit der Entscheidung für LiFePO4-3S
festgelegt (F-01 Spannungsfenster 7,5–10,95 V; N-02 −10…+40/+0…+40 °C; N-04
≤ 5 %/Monat; N-07 ≥ 2000 Zyklen @ 80 % DoD; S-01 Abschaltgrenze 2,5 V/Zelle;
S-05 Thermoschwellen). OQ-05 (Messung am realen Gerät) ist **bestätigt (2026-09-13, M1)**;
OQ-11 (Normrahmen) verbleibt als Bestätigungs-/Normaufgabe.

---

## 6. Traceability-Matrix (Stakeholder → System)

| Stakeholder-Anforderung | Systemanforderungen |
|--------------------------|----------------------|
| SR-01 (Drop-in-Ersatz) | SYS-REQ-F-01, F-03, F-04 |
| SR-02 (Pack speist Gerät) | SYS-REQ-F-01 (Scope-Verankerung) |
| SR-03 (Zielgruppe/Landwirt) | SYS-REQ-N-01, N-02, N-03, N-06 |
| SR-04 (Laufzeit 2–4 Wochen) | SYS-REQ-N-01, F-01, F-02, F-07, S-01 |
| SR-05 (230V-Netzteil) | SYS-REQ-F-05, N-08, S-02, S-06 |
| SR-06 (optional Solarmodul) | SYS-REQ-F-06 |
| SR-07 (robust/wetterfest/wartungsarm) | SYS-REQ-N-02, N-03, N-04, N-05, N-06, S-01, S-02, S-03, S-04, S-05, S-07, S-08 |
| SR-08 (Amortisation) | SYS-REQ-N-07, S-07 |
| SR-09 (ohne Gerätekauf betreibbar) | SYS-REQ-F-01, F-02, F-03, F-04, F-07, S-04 |
| SR-10 (Verbrauchsspektrum 30–43 mA) | SYS-REQ-F-01, F-02, N-01 |
| SR-11 (Technologieentscheidung) | **entschieden (LiFePO4 3S)**; wirkt über den Trace-Vermerk „SR-11 (Zielwerte festgelegt)“ auf die Zielwerte von SYS-REQ-F-01, N-02, N-04, N-07, S-01, S-05 → Zielwerte in SYS.3 fixiert |
| SR-12 (Marktlücke/Differenzierung) | SYS-REQ-F-03, F-04, N-07 |

**Statistik:** 12 Stakeholder-Anforderungen → 23 Systemanforderungen
(F 7 / N 8 / S 8). Jede Stakeholder-Anforderung ist auf mindestens eine
Systemanforderung abgebildet; SR-11 ist über die SYS.3-Technologieentscheidung
geschlossen (siehe Trace-Vermerke „SR-11 (Zielwerte festgelegt)“).

---

## 7. Konsistenzlücken / offene Punkte für menschliches Review

Die nachfolgende Liste ist aufgeteilt in **geschlossene Punkte (durch
Auftraggeber-Entscheid fixiert)** und **noch offene Punkte** (menschlicher Review
bzw. Technologieentscheidung).

### 7.1 Geschlossene Punkte (Auftraggeber-Entscheid, fixiert)

| Punkt | Entscheidung |
|---|---|
| Betriebspunkt Laufzeit (vormals OQ-01) | BA40 @ 43 mA, 3 Wochen → **Nennkapazität ≥ 31 Ah** (N-01, Abschnitt 5.3) |
| Formfaktor (vormals OQ-12) | striktes Drop-in; externes Pack nicht zulässig; **Pack-Maximalmaße B 185 × H 155 × T 125 mm (3,58 l) fixiert**; Fach-Vermessung der Referenzgeräte als Bestätigung = SYS.3-Milestone |
| IP-Klasse (vormals OQ-03) | **IPx4** nach DIN EN 60529, verbindlich (N-03) |
| Amortisation / Preisziel (vormals OQ-02, OQ-13) | **mindestens 6 Nutzsaisons**; **Verkaufspreis-Obergrenze 120 EUR** (N-07) |
| Ladezeit Netz (Folgeentscheid) | Volladung **≤ 12 h** („über Nacht“) (N-08) |
| Solar-Variante (R5) | bleibt optionale Variante; **Kompatibilitätsnachweis verpflichtend** (F-06) |

### 7.2 Noch offene Punkte (Review / Rest-Verifikation)

> **Hinweis:** Die Zelltechnologie **LiFePO4 3S** ist seit 2026-09-13
> **entschieden** (R14, §7.1/§8.1); alle technologiespezifischen Zielwerte sind
> festgelegt (F-01 7,5–10,95 V; N-02 −10…+40/+0…+40 °C; N-04 ≤ 5 %/Monat &
> ≥ 70 % nach Lagersaison; N-07 ≥ 2000 Zyklen @ 80 % DoD; N-08 CC/CV mit CV
> 3,65 V/Zelle; S-01 Abschaltgrenze 2,5 V/Zelle; S-05 Ladestopp ≥ 50 °C / Entladestopp
> ≥ 60 °C). Volumenmachbarkeit der ≥ 31 Ah im 3,58-l-Pack ist gegeben
> (Zellblock ≈ 1,0–1,2 l). → Verbleibende offene Punkte:

1. **Eingangsspannungsfenster der Zielgeräte (OQ-05):** oem-seitig nicht öffentlich; **Messung
   am realen Gerät (BA30/BA40/EC20) BESTÄTIGT (2026-09-13, M1)** das 3S-Profil (F-01) —
   geschlossen.
2. **Laden bei montiertem/angeschlossenem Gerät (OQ-04):** Betriebsmodus „Laden am
   Weidezaungerät“ spezifizieren ja/nein (F-05).
3. **Solar-Panel-Spezifikation (OQ-06):** Panel-Modell (Leerlaufspannung,
   Leistungsklasse) für den verpflichtenden Kompatibilitätsnachweis; einschließlich
   Varianten-Stückliste „mit/ohne Solar“.
4. **Fach-Vermessung der Referenzgeräte (OQ-12, M2):** Pack-Maximalmaße
   B 185 × H 155 × T 125 mm (3,58 l) sind fixiert; die Vermessung von Fach,
   Anschlussterminals und Verriegelung der Referenzgeräte **BESTÄTIGT die Passung**
   (F-03, F-04; bestätigt am 2026-09-13).
5. **Prüf-/Normenrahmen (OQ-11):** Zellnorm/Abuse (EN 62133 o. ä.), Netzteilsicherheit
   (VDE/EN), IP (IEC 60529), Gefahrgut/Transport (z. B. UN3480/3481 bei LiFePO4) —
   Auswahl offen (wirkt auf S-05, S-06, S-08, N-03).
6. **Tierkontakt/Nageschutz (OQ-09):** Anforderung „nagesicher“ (Gehäuse/Kabel)
   ja/nein?
7. **Ladezustandsanzeige (OQ-10):** nicht in den Stakeholder-Anforderungen enthalten;
   Wunsch als Feature (Kostentreiber, tierwohlrelevant wegen Zaunstrom-Ausfall)
   klären.

---

## 8. Produktentscheidungen (Entscheider)

Die folgenden Punkte wurden dem menschlichen Entscheider vorgelegt. Ein Teil ist
**entschieden (fixierte Werte, siehe Abschnitt 7.1/8.1)**, die verbleibenden sind
**bewusst NICHT entschieden** und bleiben offen:

### 8.1 Zelltechnologie — ENTSCHIEDEN: LiFePO4 3S (2026-09-13)

Die Technologieentscheidung im Rahmen SYS.3 fiel auf **LiFePO4 3S (9,6 V nominal,
Entladebereich 7,5–10,95 V)** als fixierte Referenzrealisierung (SR-11):

- **LiFePO4 (3S, 9,6 V) — gewählt:** BMS zwingend erforderlich, ≥ 2000 Zyklen,
  Selbstentladung ≤ 5 %/Monat, gut im Winter (−10…+40 °C), intrinsisch
  missbrauchsstabil; Zellblock ≈ 1,0–1,2 l / ≈ 2,8–3,3 kg als einzige Option
  vollständig im 3,58-l-Pack-Budget (F-04) und Gewichtsbudget (3,0–3,8 kg).
- **AGM/Vlies — verworfen:** 8 V spannungsseitig marginal, ~500 Zyklen (N-07),
  31 Ah ≈ 3,3–5 l / 5–8 kg → volumen- und gewichtsbedingt nicht im Pack-Budget
  darstellbar; Säureaustritt bei Beschädigung (S-08).
- **NiMH — verworfen:** volumenmäßig (≈ 1,5–2,0 l) darstellbar, aber hohe
  Selbstentladung (> 20 %/Monat → N-04 verletzt), schlechtes Kälteverhalten (N-02)
  und ~500–1000 Zyklen (N-07) — für deutschen Winter und Saisonbetrieb nachteilig.

**Damit festgelegte Zielwerte:** F-01 Spannungsfenster 7,5–10,95 V; N-02
−10…+40 °C Entladen / +0…+40 °C Laden; N-04 Selbstentladung ≤ 5 %/Monat,
Mindestladezustand nach ≥ 6 Monaten ≥ 70 %; N-07 ≥ 2000 Zyklen @ 80 % DoD;
N-08 CC/CV-Ladung ≤ 12 h inkl. CV-Abschlussblock und Derating; S-01 Abschaltgrenze
≤ 2,5 V/Zelle (7,5 V Pack unter Last); S-05 Ladestopp ≥ 50 °C / Entladestopp ≥ 60 °C
(Fixierung je Zell-Datenblatt in Phase P).

**Verbleibende Bestätigungs-/Normaufgaben (nicht mehr entscheidungsblockierend):**
OQ-11 Normenrahmen (EN 62133, VDE/EN, IEC 60529, UN3480/3481).
OQ-05/M1 (realer Spannungsfenster-Nachweis) ist **erledigt — 3S-Profil bestätigt (2026-09-13)**;
OQ-12/M2 (Fachpassung) ebenso **erledigt — bestätigt (2026-09-13)**.

### 8.2 Kapazität / Formfaktor (entschieden — Machbarkeit bestätigt)

- **Entschieden (fixiert):** Betriebspunkt BA40 @ 43 mA, 3 Wochen → Nennkapazität
  ≥ 31 Ah (N-01); striktes Drop-in in das Batteriefach eines Referenzgeräts,
  externes Pack nicht zulässig (F-04); **Pack-Maximalmaße B 185 × H 155 × T 125 mm
  (3,58 l Außenvolumen, Innen ≈ 3,0–3,2 l)**; Fach-Vermessung als SYS.3-Milestone
  verankert.
- **Bestätigt (LiFePO4 3S):** Zellblock ≥ 31 Ah ≈ 1,0–1,2 l / ≈ 2,8–3,3 kg →
  im Pack-Budget klar darstellbar (Reserve ≈ 2 l bzw. ≥ 0,5 kg für Topologie-
  Varianten). Fach-Vermessung der Referenzgeräte bestätigt die Formpassung.
- **Offen (Rest):** finale Packgröße/-gewicht je Zelltopologie in Phase P;
  keine Kapazitäts-Neuabstimmung mehr nötig.

### 8.3 Preisziel / Zielkosten (teilentschieden)

- **Entschieden (fixiert):** Verkaufspreis-Obergrenze **120 EUR**;
  Amortisationsnachweis über **mindestens 6 Nutzsaisons** gegenüber
  Einweg-Trockenbatterien (30–80 EUR/Saison) — Basis N-07.
- **Noch offen:** Zyklusziel-Machbarkeit vs. Zielkosten (LiFePO4-BOM-Kontrolle);
  geplante Stückzahlen/Preisstruktur (Set inkl. Netzteil, Solar-Variante
  mit Aufpreis) — beeinflusst die Zielkosten.

---

## 9. Formale Hinweise

- Anforderungen sind **atomar** formuliert (eine Anforderung je ID).
- Jede Anforderung besitzt ein **mess-/testbares Verifikationskriterium**.
- Bidirektionale Traceability zur Vorgänger-Ebene ist in Abschnitt 6 hergestellt.
- [offen]-Kennzeichnungen verweisen auf die Abschnitte 7 und 8 und dürfen erst
  nach menschlicher Freigabe konkrete Zielwerte erhalten.
- Nachfolgeprozess SYS.3: Elementtypen Hardware, Software (BMS), Mechanik.