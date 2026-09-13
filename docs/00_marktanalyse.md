# Marktanalyse: Wiederaufladbarer 9V-Weidezaun-Akku

Stand: 2026 · Basis: Webrecherche (Hersteller, Fachhändler, Tests)

## 1. Aufgabenstellung

Wiederaufladbarer Akku als **direkter Ersatz für 9V-Trockenbatterien**
(Zink-Kohle/Alkaline) in batteriebetriebenen Weidezaungeräten. Zielgruppe:
Landwirte mit mittleren Weiden.

## 2. Status quo im 9V-Segment

9V-Weidezaugeräte (Gallagher BA20/BA30/BA40/BA80/B100, Koltec EC20, AKO)
werden mit **Einmal-Trockenbatterien** (55–210 Ah) betrieben:

- nicht wiederaufladbar, Saisonware
- Luft-Sauerstoff-Zelle: Selbstentladung startet bei Aktivierung sofort
  (Versiegelung wird entfernt), auch ohne Stromabnahme
- Chemische Zersetzung trotz Lagerung möglich
- Kosten pro Saison ca. 30–80 EUR
- Entsorgungsaufwand

## 3. Wiederaufladbare Alternativen heute

| Option | Beschreibung | Einschränkung |
|--------|--------------|---------------|
| 12V-Akku + Kabelset | AGM/Gel/LiFePO4, externe Box | passt nicht ins 9V-Gehäuse, Adapterkabel nötig |
| Solargeräte | z. B. Gallagher S16li mit LiFePO4 3,2V/6Ah | LiFePO4 **fest integriert**, nicht als tauschbarer 9V-Akku |
| 12V/12Ah mit Laderegler | für 9V-Geräte geeignet, extern | kein 9V-Formfaktor, meist AGM |

**Erkenntnis:** Es existiert kein wiederaufladbares 9V-Akku-Pack als
Drop-in-Ersatz für die klassische 9V-Trockenbatterie. Das ist eine Marktlücke.

## 4. Referenzgeräte & Verbrauchskennzahlen

| Gerät | Verbrauch | Batterie | Laufzeit |
|-------|-----------|----------|----------|
| Gallagher BA30 | 34 mA | 9V-55Ah | 65 Tage |
| Gallagher BA40 | 43 mA | 9V-175Ah | 160 Tage |
| Koltec EC20 | 30 mA | 9V-200Ah | 231 Tage |

Rechenregel: nutzbare Kapazität ≈ 70 % der Nenn-Ah.
Energieäquivalent einer 9V-120Ah-Einweg-Batterie ≈ 1080 Wh (ca. 1 kWh).

## 5. Anforderungen der Zielgruppe (Landwirt, mittlere Weiden)

- Zaunlängen ca. 1–3 km, Bewuchs mittel
- Betrieb mit vorhandenen 9V-Weidezaungeräten (kein Gerätekauf nötig)
- Laufzeit ohne Nachladen: Ziel 2–4 Wochen
- Wiederaufladbar über 230V-Netzteil und optional Solarmodul
- Robust, wetterfest (Outdoor), wartungsarm
- Kosten: Amortisation gegenüber Einweg-Batterien wichtig

## 6. Technologische Optionen (Bewertungsrahmen für Anforderungsanalyse)

| Kriterium | LiFePO4 (3S=9,6V) | AGM/Vlies (8V-Gebilde) | NiMH |
|-----------|-------------------|------------------------|------|
| Spannung passend 9V | 9,6V (3S) ✓ | 8V (marginal) | 8,4–9,6V digital/3S ✓ |
| Zyklenzahl | 2000+ | ~500 | ~500–1000 |
| Gewicht | sehr leicht | schwer | mittel |
| Leistungsdichte | hoch | gering | mittel |
| Ladung/BMS | zwingend (BMS) | simpel | simpel |
| Preis/Wh | hoch | niedrig | mittel |
| Kälteverhalten | gut | gut | schlecht |

## 7. Fazit für die Entwicklung

- Zieltechnologie: **LiFePO4** (3S) als vorausgewählte Variante, in der
  Anforderungsanalyse gegen AGM/NiMH bewerten (Entscheidung offen —
  **update 2026-09-13:** in SYS.3 **entschieden: LiFePO4 3S**, siehe
  `01_spezifikation_anforderungen.md` §8.1).
- Drop-in-Formfaktor und Verträglichkeit mit bestehenden 9V-Geräten
  sind die entscheidenden Produktmerkmale.
- Kernherausforderungen: Spannungsprofil (9,6V nominal), minimal
  gehaltene Selbstentladung, Laderegelung mit Über-/Tiefentladeschutz,
  robustes Outdoor-Gehäuse.

## 8. Wettbewerbsumfeld

- Gallagher, AKO, Patura, Horizont, VOSS.farming, Kerbl, Koltec (EU-marktführend)
- Kein Anbieter hat derzeit ein tauschbares wiederaufladbares 9V-Pack
- Differenzierung über: Drop-in-Kompatibilität, Zyklenfestigkeit, Preis/Laufzeit