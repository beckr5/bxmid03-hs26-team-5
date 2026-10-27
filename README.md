# Team 5 – Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

**Modul:** Data Management & BIM
**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen
**Team:** Team 5
**Version:** MVP 0.1
**Stand:** 27.10.2026
**Status:** In Bearbeitung

---

## 1. Projektbeschreibung

### Ausgangslage und Problemstellung

Bei der Planung von Holzrahmenbauwänden müssen verschiedene Bauteilinformationen vollständig und korrekt im BIM-Modell hinterlegt sein.

Fehlende Wandbezeichnungen, ungültige Wanddicken oder fehlende Materialzuordnungen können zu Rückfragen, Planungsfehlern und zusätzlichem Aufwand in der Werkplanung führen.

Die Überprüfung dieser Informationen erfolgt häufig manuell und ist entsprechend zeitaufwendig.

### Projektziel

Ziel des Projekts ist die Entwicklung eines automatisierten Qualitätschecks für Wandbauteile in IFC-Modellen.

Der Check soll fehlende oder ungültige Informationen erkennen und die Ergebnisse nachvollziehbar dokumentieren.

Dadurch sollen die Datenqualität verbessert und manuelle Prüfprozesse reduziert werden.

### Zielgruppe

* Holzbauplanung
* Arbeitsvorbereitung
* BIM-Koordination
* Fachplanung und Modellautoren

---

## 2. Informationsanforderungen

Im Rahmen des Projekts wurden drei zentrale Informationsanforderungen definiert:

| ID | Anforderung       | Prüfkriterium                       |
| -- | ----------------- | ----------------------------------- |
| A1 | Wandbezeichnung   | `Name` vorhanden und nicht leer     |
| A2 | Wanddicke         | `Width` vorhanden und grösser als 0 |
| A3 | Materialzuordnung | Materialzuordnung vorhanden         |

Die vollständigen Anforderungen sind im Ordner `requirements/` dokumentiert.

Für das erste MVP wird zunächst die automatisierte Prüfung der Wanddicke umgesetzt.

---

## 3. MVP – Minimum Viable Product

Das MVP umfasst einen einfachen, wiederholbaren Qualitätscheck.

### Input

* IFC-Gebäudemodell
* Definierte Prüfregel
* Festgelegte IFC-Objektauswahl

**Verwendetes Arbeitsmodell:**

`Team_5_BIMcollab_Example_STR.ifc`

**IFC-Version:** IFC2X3

### Verarbeitung

Der automatisierte Prüfprozess umfasst folgende Schritte:

1. IFC-Modell einlesen.
2. Relevante Wandobjekte identifizieren.
3. Wanddicke aus den IFC-Daten auslesen.
4. Vorhandenen Wert mit der Prüfregel vergleichen.
5. Prüfergebnis pro Wand dokumentieren.

### Output

Der Qualitätscheck soll einen strukturierten Prüfbericht erzeugen.

Dieser enthält mindestens:

* GlobalId des geprüften Bauteils
* Tatsächlicher Wert (Ist-Wert)
* Angewendete Prüfregel
* Prüfergebnis
* Begründung bei fehlerhaften oder fehlenden Angaben

Zusätzlich werden die Anzahl geprüfter, bestandener und nicht bestandener Objekte dokumentiert.

---

## 4. Technische Umsetzung

### Verwendete Werkzeuge

| Werkzeug        | Verwendung                                          |
| --------------- | --------------------------------------------------- |
| IFC-Lite        | Analyse und automatisierte Prüfung von IFC-Modellen |
| IDS / IDS-Light | Definition maschinenlesbarer Prüfregeln             |
| GitHub          | Versionsverwaltung und Projektdokumentation         |
| CSV             | Dokumentation der Prüfergebnisse                    |

### Prüfregel A2 – Wanddicke

Für den ersten automatisierten Check wird folgende Regel verwendet:

**Jede ausgewählte Wand muss eine Wanddicke grösser als 0 besitzen.**

| Parameter                  | Wert                   |
| -------------------------- | ---------------------- |
| IFC-Entität                | `IfcWallStandardCase`  |
| Quantity-Set               | `BaseQuantities`       |
| Quantity                   | `Width`                |
| Datenpfad                  | `BaseQuantities.Width` |
| Datentyp                   | Länge                  |
| Bedingung                  | `Width > 0`            |
| Status bei fehlendem Wert  | FEHLEND                |
| Status bei ungültigem Wert | DURCHGEFALLEN          |
| Status bei gültigem Wert   | BESTANDEN              |

Die Einheit wird entsprechend den Angaben des IFC-Modells berücksichtigt.

---

## 5. Setup und Ausführung

### Voraussetzungen

* Aktueller Webbrowser
* Zugriff auf das IFC-Modell
* Zugriff auf IFC-Lite
* Definierte Prüfregel

Eine lokale Softwareinstallation ist für den vorgesehenen browserbasierten Ablauf nicht erforderlich.

### Geplanter Prüfablauf

1. Webseite https://ifclite.com öffnen.
2. IFC-Modell laden.
3. Den Bereich `Analyze` öffnen.
4. `Flow` auswählen.
5. Wandobjekte über `By type` auswählen.
6. Wanddicke über `Quantity` auslesen.
7. Mit `Compare` den Wert auf `> 0` prüfen.
8. Ergebnisse über `Filter` und `Count` auswerten.
9. Prüfergebnisse dokumentieren.
10. Workflow im Ordner `checks/` speichern.

**Wichtig:** Die endgültige Werkzeugversion, der genaue Klickpfad und die exportierte Prüfdatei werden nach dem erfolgreichen Testlauf ergänzt.

Damit soll sichergestellt werden, dass andere Personen den Check selbstständig wiederholen können.

---

## 6. Validierung und Testfälle

Die Funktionsfähigkeit des Checks wird anhand eines positiven und eines negativen Testfalls nachgewiesen.

### Positiver Testfall

Eine Wand besitzt eine gültige Wanddicke.

**Beispiel:**

* Ist-Wert: 100 mm
* Prüfregel: Wanddicke > 0
* Erwartetes Ergebnis: BESTANDEN

### Negativer Testfall

Eine Wand besitzt eine ungültige oder fehlende Wanddicke.

**Beispiel:**

* Ist-Wert: 0 mm
* Prüfregel: Wanddicke > 0
* Erwartetes Ergebnis: DURCHGEFALLEN

### Vergleichbarkeit

Für beide Testfälle wird dieselbe Prüfregel verwendet.

Die Objektselektion und die Prüfbedingungen bleiben unverändert. Nur die Testdaten werden gezielt verändert.

Die tatsächlichen Ergebnisse werden nach Durchführung der Tests im Ordner `results/` dokumentiert.

---

## 7. Repository-Struktur

Das GitHub-Repository ist folgendermassen aufgebaut:

| Datei / Ordner     | Inhalt                                        |
| ------------------ | --------------------------------------------- |
| `README.md`        | Projektbeschreibung, Setup und Ausführung     |
| `requirements/`    | Projektwahl, AIA-BEP-Matrix und Spezifikation |
| `workflow/`        | Darstellung des Ist- und Sollprozesses        |
| `checks/`          | Automatisierte Prüfregeln und Workflows       |
| `results/`         | Positive und negative Prüfergebnisse          |
| `models/README.md` | Informationen zum verwendeten IFC-Modell      |
| `poster/`          | Digitales Projektposter                       |
| `KI_USAGE.md`      | Dokumentation der KI-Nutzung                  |

---

## 8. Prozessbeschreibung

Der bisherige Ist-Prozess basiert auf einer manuellen Überprüfung der Wandinformationen.

Im geplanten Soll-Prozess werden diese Informationen automatisiert kontrolliert.

**Soll-Prozess:**

1. Fachplanung erstellt das IFC-Modell.
2. Modellautor stellt das Modell bereit.
3. Automatisierte Qualitätsprüfung wird durchgeführt.
4. Prüfergebnisse werden dokumentiert.
5. Fehlerhafte Angaben werden an den Modellautor zurückgemeldet.
6. Modellautor korrigiert das Modell.
7. Erneute Prüfung und anschliessende Freigabe.

Die detaillierten BPMN-Prozessdarstellungen befinden sich im Ordner `workflow/`.

---

## 9. Einschränkungen und offene Punkte

Das verwendete IFC-Arbeitsmodell dient zunächst zur technischen Validierung der Prüfregeln.

Die eindeutige Identifikation von Holzrahmenbauwänden und die Prüfung eines geeigneten Holzbaumodells sind noch zu klären.

Ausserdem bestätigt ein bestandener Informationscheck nicht automatisch die fachliche oder konstruktive Richtigkeit eines Bauteils.

Folgende Arbeiten sind noch offen:

* Prüfregel technisch fertigstellen
* Positiven und negativen Testfall durchführen
* Prüfergebnisse dokumentieren
* Exakte Anleitung und Werkzeugversion ergänzen
* Materialzuordnung und Wandbezeichnung in weiteren Prüfungen berücksichtigen

---

## 10. Weiterentwicklung

Nach erfolgreicher Umsetzung des ersten MVP soll die Qualitätsprüfung erweitert werden.

Geplant sind:

* Automatisierte Prüfung der Wandbezeichnung
* Automatisierte Prüfung der Materialzuordnung
* Zusammenführung aller drei Anforderungen
* Verbesserte Darstellung der Prüfergebnisse
* Unterstützung eines strukturierten BIM-Qualitätssicherungsprozesses

---

## 11. Projektunterlagen

Die Dokumentation basiert auf folgenden Grundlagen:

* Projektwahl Team 5
* Projektmatrix Team 5
* Spezifikation Team 5
* BPMN Ist-/Sollprozess
* IFC-Beispielmodell
* Unterrichtsunterlagen Data Management & BIM

---

**Projektverantwortung:** Team 5
**Repository:** `LTplus-AG/bxmid03-hs26-team-5`
