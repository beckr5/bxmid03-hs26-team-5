
# Projektwahl – Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

## 1. Projekttitel und Use Case
**Projekttitel:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen  
**Use Case:** Ein Holzbauunternehmen benötigt vor der Werkplanung vollständige und prüfbare Wandinformationen; ein automatisierter IFC-Check soll fehlende Angaben frühzeitig erkennen.

## 2. Zielgruppe und Nutzen
**Zielgruppe:** Holzbauplanung, Arbeitsvorbereitung und BIM-Koordination.  
**Nutzen:** Die bisher manuelle Kontrolle ausgewählter Wandinformationen wird teilweise automatisiert. Fehlende Angaben werden früh erkannt und gezielt an die Modellverantwortlichen zurückgemeldet. Dadurch sollen Rückfragen und Nacharbeiten reduziert werden.

## 3. IFC-Modell und Evidenz
**Modell und Evidenz:**
Als Grundlage dienen die im Unterricht bereitgestellten IFC-Gebäudemodelle. Die Auswahl eines geeigneten Modells erfolgt im weiteren Projektverlauf. Untersucht werden Wandbauteile hinsichtlich ihrer Bauteilbezeichnungen, Wanddicken und Materialinformationen. Ziel ist es, fehlende oder unvollständige Informationen automatisiert zu erkennen und zu dokumentieren. Die konkrete Untersuchung einzelner Bauteile sowie die Dokumentation der Ergebnisse anhand von GlobalIds erfolgen nach der Projektwahl.

## 4. Drei Informationsanforderungen (AIA → BEP → Check)
| Nr. | Informationsanforderung | Datenbasis und Prüfung |
| --- | --- | --- |
| A1 | Jede relevante Wand hat eine nicht leere Bezeichnung. | `IfcWall.Name`; Prüfung auf vorhandenen Text. |
| A2 | Jede relevante Wand hat eine angegebene Wanddicke > 0. | Z. B. `Qto_WallBaseQuantities.Width`; Prüfung auf Vorhandensein und positiven Wert. |
| A3 | Jeder relevanten Wand ist ein Material zugeordnet. | IFC-Materialzuordnung (z. B. `IfcRelAssociatesMaterial`); Prüfung auf Vorhandensein. |

**AIA:** Welche Informationen werden benötigt? **BEP:** Wer liefert sie wann, und wie erfolgt die Prüfung? Die Regeln werden nach Möglichkeit in IDS/IDS-Light und bei Bedarf als dokumentierte eigene Prüflogik umgesetzt.

## 5. MVP: Automatisierung, Input und Output
**Input:** IFC-Modell und definierte Prüfkriterien (IDS/IDS-Light bzw. Konfiguration).  
**Automatisierung:** Relevante Wände identifizieren, drei Anforderungen prüfen, Ergebnisse pro Bauteil mit `GlobalId` dokumentieren.  
**Output:** CSV-Prüfbericht mit Wand-ID, Prüfergebnis je Anforderung und Fehlerbeschreibung; optional eine Zusammenfassung der bestandenen und nicht bestandenen Prüfungen.  
**Validierung:** Ein positiver und ein gezielt erzeugter negativer Testfall zeigen, dass der Check korrekt reagiert.

## 6. Verworfene Ideen
- **Automatische Mengenermittlung:** höherer Aufwand durch benötigte Mengen- und Materialdaten.
- **Automatisierte Kostenschätzung:** zusätzliche Preislisten und Berechnungsannahmen erforderlich.

**Entscheid:** Die Qualitätsprüfung hat einen direkten Praxisbezug, klar messbare Anforderungen und einen überschaubaren MVP-Umfang bis Dezember.
