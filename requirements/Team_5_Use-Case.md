# Projektwahl – Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

**Modul:** Data Management & BIM – BFH
**Team:** 5
**Stand:** 28.10.2026

## 1. Projektidee

Bei der Planung von Holzrahmenbauwänden müssen wichtige Informationen im IFC-Modell vorhanden sein.

Wir möchten untersuchen, wie sich diese Informationen automatisch prüfen lassen. Dadurch sollen Fehler vor der Werkplanung erkannt und manuelle Kontrollen reduziert werden.

## 2. Zielgruppe und Nutzen

Unser Projekt richtet sich hauptsächlich an die Holzbauplanung, Arbeitsvorbereitung und BIM-Koordination.

Durch eine automatisierte Prüfung können fehlende oder ungültige Angaben schneller erkannt werden. Die betroffenen Bauteile können anschliessend gezielt korrigiert werden.

## 3. Unsere Informationsanforderungen

Wir haben drei Anforderungen festgelegt:

| ID | Anforderung       | Prüfung                               |
| -- | ----------------- | ------------------------------------- |
| A1 | Wandbezeichnung   | `IfcWall.Name` darf nicht leer sein   |
| A2 | Wanddicke         | `Width` muss grösser als 0 sein       |
| A3 | Materialzuordnung | Materialzuordnung muss vorhanden sein |

Diese Anforderungen bilden die Grundlage für unseren automatisierten Qualitätscheck.

## 4. Umsetzung im MVP 0.1

Für das erste MVP haben wir uns auf **A2 – Wanddicke** konzentriert.

Wir haben dafür einen Python-Check mit Google Colab und IfcOpenShell erstellt.

Der Check liest die Wanddicke aus dem IFC-Modell und kontrolliert, ob der Wert grösser als 0 ist.

Für den Test verwendeten wir zwei IFC4-Lehrmodelle:

* **Fehlerhaftes Modell:** Wanddicke 0 m → durchgefallen
* **Korrigiertes Modell:** Wanddicke 0.1143 m → bestanden

Beide Testfälle wurden mit derselben Prüfregel durchgeführt.

Die Ergebnisse werden als CSV-Dateien gespeichert und enthalten unter anderem die GlobalId der geprüften Wand.

Unser ursprüngliches IFC2X3-Arbeitsmodell wurde mit diesem Check noch nicht geprüft.

## 5. Weitere Entwicklung

Als Nächstes möchten wir auch die Wandbezeichnung (A1) und die Materialzuordnung (A3) automatisch prüfen.

Ausserdem möchten wir den Check auf unser ursprüngliches IFC-Modell übertragen.

## 6. Verworfene Ideen

Zu Beginn haben wir auch andere Projektideen betrachtet:

* **Automatische Mengenermittlung:** höherer Aufwand durch benötigte Mengen- und Materialdaten.
* **Automatisierte Kostenschätzung:** zusätzliche Preislisten und Berechnungsannahmen erforderlich.

Wir haben uns für die Qualitätsprüfung entschieden, weil wir damit eine konkrete BIM-Aufgabe mit überschaubarem Aufwand automatisieren können.

---

**Team 5 – Data Management & BIM**
