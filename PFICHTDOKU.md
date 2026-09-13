# Pflichtspezifikation

Am Ende der Entwicklung müssen folgende Spezifikationen im Projekt vorhanden sein:

| Spezifikation | Inhalt | Prozess |
|---------------|--------|---------|
| `docs/01_spezifikation_anforderungen.md` | Anforderungsspezifikation (SYS.2/SWE.1) | requirements-agent |
| `docs/02_spezifikation_architektur_logisch.md` | Logische Architektur (technologie-unabhängig) | architecture-agent (Phase L) |
| `docs/03_spezifikation_architektur_physisch.md` | Physische Architektur (konkrete Realisierung) | architecture-agent (Phase P) |
| `docs/04_verifikation.md` | Teststrategie, Testfälle, Ergebnisse | verification-agent |
| `docs/05_traceability.md` | Bidirektionale Rückverfolgbarkeit | traceability-agent |
| `specs-cmp/*.md` | Komponenten-Spezifikationen (K1, K2-Zellen, K2-BMS, K3) zur **Komponenten-Auswahl** | komponenten-agent (SYS-Ebene) |
| `build/*.md` | **Umsetzung:** Aufbau (`AUFBAU.md`), Verdrahtung (`VERDRAHTUNG.md`), Datenblätter (`datenblatt/`) der beschafften Komponenten | umsetzungs-ablage (Parallel zu docs/specs) |

> **Kein SWE-Entwicklungsprojekt:** Die SWE-Phase (SWE.1…SWE.5 inkl. `docs/03a`
> SWE-Architektur/Detaildesign, SWE-Verifikation) wird **bewusst nicht** durchgeführt.
> Stattdessen werden die Komponenten **nach Spezifikation ausgewählt** — Grundlage
> sind die SYS-Dokumente (`docs/01`–`docs/06`) und die Komponentenspezifikationen
> im Verzeichnis `specs-cmp/`. Embedded-Anteile (Laderegler-Parametrierung,
> BMS-Schwellwerte) werden als Parametrierung, nicht als eigenständige SW gemeint.

## Inhalt je Spezifikation

### 01_spezifikation_anforderungen.md
- Stakeholder-Anforderungen (falls Systemebene)
- Software-Anforderungen (SWR-XX)
- Verifikationskriterien je Anforderung

### 02_spezifikation_architektur_logisch.md
- Logische Elemente und deren Funktion
- Schnittstellen zwischen Elementen
- Verhalten (Sequenzen, Zustände)
- Traceability zu Anforderungen

### 03_spezifikation_architektur_physisch.md
- Technologiezuordnung
- Physische Schnittstellen
- Ressourcenbedarf
- Architektur-Bewertung
- Zweistufige Traceability

### 04_verifikation.md
- Teststrategie
- Testfallspezifikation
- Testausführungen (mit Output)
- Abdeckungsnachweis

### 05_traceability.md
- Anforderung → Element → Test
- Prüfung der Bidirektionalität
- Lücken/Inkonsistenzen
