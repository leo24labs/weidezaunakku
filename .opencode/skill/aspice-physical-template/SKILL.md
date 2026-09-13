---
name: aspice-physical-template
description: Base Practices für die physische Architektur (RLP-Phase P, konkrete Realisierung). Nutzen, nachdem die logische Architektur (Phase L) abgeschlossen ist.
---

1. Jedes logische Element auf ein physisches Element abbilden (Technologie, HW/SW/Bibliothek)
2. Physische Schnittstellen (Protokolle, Busse, APIs) konkretisieren
3. Ressourcenbedarf je physischem Element abschätzen (Rechenzeit, Speicher, Bandbreite)
4. Konsistenz zur logischen Architektur prüfen — jede physische Entscheidung muss auf ein logisches Element rückführbar sein
5. Physische Architektur anhand von Kriterien bewerten (Wartbarkeit, Testbarkeit, Wiederverwendbarkeit, Austauschbarkeit der Technologie)
6. Traceability logisches Element ↔ physisches Element herstellen
7. Uneindeutige oder mehrdeutige Zuordnungen als offene Frage markieren, nicht selbst entscheiden
