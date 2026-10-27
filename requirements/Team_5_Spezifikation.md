# Spezifikation – Team 5

## Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

**Projekt:** Data Management & BIM
**Team:** Team 5
**Version:** 0.1
**Datum:** 27.10.2026
**Status:** Entwurf – technische Validierung teilweise ausstehend

---

## 1. Ziel der Spezifikation

Ziel dieses Projekts ist die automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen.

Die Prüfung soll sicherstellen, dass relevante Wandbauteile die für die weitere Planung und Werkvorbereitung notwendigen Informationen enthalten.

Fehlende oder ungültige Angaben sollen automatisch erkannt und in einem Prüfbericht dokumentiert werden.

Dadurch soll der bisher manuelle Prüfaufwand reduziert und die Datenqualität verbessert werden.

---

## 2. Anwendungsbereich

### 2.1 Zielgruppe

Die Spezifikation richtet sich an folgende Projektbeteiligte:

* Holzbauplanung
* Arbeitsvorbereitung
* BIM-Koordination
* Fachplanung und Modellautoren

### 2.2 Prüfobjekte

Die Qualitätsprüfung bezieht sich auf relevante Wandbauteile innerhalb eines IFC-Modells.

**IFC-Entitäten:**

* `IfcWall`
* `IfcWallStandardCase`

Die fachliche Zielgruppe sind Holzrahmenbauwände.

Für den ersten technischen Test wird das vorhandene IFC-Beispielmodell verwendet. Die eindeutige Auswahl von Holzrahmenbauwänden muss für die spätere fachliche Anwendung noch festgelegt werden.

### 2.3 Prüfzeitpunkt

Die Prüfung erfolgt bei der IFC-Modellabgabe vor der Werkplanung.

---

## 3. Informationsanforderungen

Für die automatisierte Qualitätsprüfung wurden drei Informationsanforderungen definiert.

| ID | Anforderung       | Beschreibung                                              | Prüfkriterium                 |
| -- | ----------------- | --------------------------------------------------------- | ----------------------------- |
| A1 | Wandbezeichnung   | Jede relevante Wand benötigt eine eindeutige Bezeichnung. | Name vorhanden und nicht leer |
| A2 | Wanddicke         | Jede relevante Wand benötigt eine gültige Wanddicke.      | Wanddicke > 0                 |
| A3 | Materialzuordnung | Jeder relevanten Wand muss ein Material zugeordnet sein.  | Materialzuordnung vorhanden   |

Die Anforderungen basieren auf der Projektwahl und der AIA-BEP-Anforderungsmatrix von Team 5.

---

## 4. Technische Spezifikation der Prüfregeln

### 4.1 Regel A1 – Wandbezeichnung

**Ziel:** Sicherstellen, dass relevante Wandbauteile eine gültige Bezeichnung besitzen.

| Eigenschaft | Spezifikation                 |
| ----------- | ----------------------------- |
| Regel-ID    | A1                            |
| IFC-Entität | IfcWall / IfcWallStandardCase |
| Attribut    | Name                          |
| Datenpfad   | `IfcWall.Name`                |
| Datentyp    | Text                          |
| Pflichtfeld | Ja                            |
| Bedingung   | Wert vorhanden und nicht leer |
| Prüfmethode | Automatische Attributprüfung  |

**Prüflogik:**

Eine Wand besteht die Prüfung, wenn das Attribut `Name` einen Text enthält, der nicht ausschliesslich aus Leerzeichen besteht.

**Positiver Testfall:**

`Name = "Holzrahmenwand EG"`

Ergebnis: BESTANDEN

**Negativer Testfall:**

`Name = ""`

Ergebnis: NICHT BESTANDEN

---

### 4.2 Regel A2 – Wanddicke

**Ziel:** Sicherstellen, dass jede relevante Wand eine gültige, positive Wanddicke besitzt.

| Eigenschaft                | Spezifikation                  |
| -------------------------- | ------------------------------ |
| Regel-ID                   | A2                             |
| IFC-Entität                | IfcWall / IfcWallStandardCase  |
| Quantity                   | Width                          |
| Datenpfad IFC4             | `Qto_WallBaseQuantities.Width` |
| Möglicher Datenpfad IFC2X3 | `BaseQuantities.Width`         |
| Datentyp                   | Länge                          |
| Pflichtfeld                | Ja                             |
| Bedingung                  | Wanddicke > 0                  |
| Prüfmethode                | Automatische Mengenprüfung     |

**Prüflogik:**

Eine Wand besteht die Prüfung, wenn:

1. Eine Wanddicke vorhanden ist.
2. Der Wert numerisch auswertbar ist.
3. Die Wanddicke grösser als 0 ist.

Die Einheit der Wanddicke muss aus dem IFC-Modell berücksichtigt werden.

**Beispiel:**

Eine Wanddicke von 100 mm entspricht 0.100 m.

**Positiver Testfall:**

`Width = 100 mm`

Ergebnis: BESTANDEN

**Negativer Testfall:**

`Width = 0 mm`

Ergebnis: NICHT BESTANDEN

**Fehlender Wert:**

Wenn keine Wanddicke vorhanden ist, wird das Ergebnis als FEHLEND dokumentiert.

**Hinweis:**

Der genaue Quantity-Pfad muss anhand des verwendeten IFC-Modells überprüft werden. Insbesondere können sich die Bezeichnungen zwischen IFC-Versionen unterscheiden.

---

### 4.3 Regel A3 – Materialzuordnung

**Ziel:** Sicherstellen, dass relevanten Wandbauteilen Materialinformationen zugeordnet sind.

| Eigenschaft             | Spezifikation                                    |
| ----------------------- | ------------------------------------------------ |
| Regel-ID                | A3                                               |
| IFC-Entität             | IfcWall / IfcWallStandardCase                    |
| Information             | Materialzuordnung                                |
| Möglicher IFC-Datenpfad | `IfcRelAssociatesMaterial`                       |
| Pflichtfeld             | Ja                                               |
| Bedingung               | Materialzuordnung vorhanden                      |
| Prüfmethode             | Automatische Prüfung der IFC-Materialbeziehungen |

**Prüflogik:**

Eine Wand besteht die Prüfung, wenn eine gültige Materialzuordnung entsprechend dem festgelegten Materialpfad vorhanden ist.

Materialinformationen können sich auf unterschiedlichen Ebenen befinden:

* Direkt am Wandobjekt
* Am zugeordneten Wandtyp
* An einzelnen Bestandteilen oder Materialschichten

Der verbindliche Materialpfad muss anhand des tatsächlichen IFC-Modells festgelegt werden.

**Positiver Testfall:**

Einer Wand ist ein Material zugeordnet.

Ergebnis: BESTANDEN

**Negativer Testfall:**

Eine Wand besitzt keine Materialzuordnung.

Ergebnis: NICHT BESTANDEN

---

## 5. IFC-Datenmodell

### 5.1 Verwendetes Beispielmodell

Für die technische Entwicklung und Validierung wird folgendes Beispielmodell verwendet:

`Team_5_BIMcollab_Example_STR.ifc`

Die Datei dient als Grundlage für die Untersuchung von Wandobjekten und deren Eigenschaften.

### 5.2 Relevante IFC-Informationen

| Information                      | IFC-Struktur                                               |
| -------------------------------- | ---------------------------------------------------------- |
| Eindeutige Bauteilidentifikation | `GlobalId`                                                 |
| Wandbezeichnung                  | `IfcWall.Name`                                             |
| Wanddicke                        | `BaseQuantities.Width` bzw. `Qto_WallBaseQuantities.Width` |
| Material                         | IFC-Materialzuordnung                                      |
| Bauteilklasse                    | `IfcWall` / `IfcWallStandardCase`                          |

Jedes geprüfte Bauteil wird über seine `GlobalId` identifiziert.

Dadurch können Prüfresultate eindeutig einem Bauteil zugeordnet werden.

---

## 6. Automatisierte Qualitätsprüfung

### 6.1 Input

Die automatisierte Prüfung verwendet folgende Eingangsdaten:

1. IFC-Modell
2. Definierte Informationsanforderungen
3. Maschinenlesbare Prüfregeln (IDS, IDS-Light oder eigene Prüflogik)

### 6.2 Verarbeitung

Der automatisierte Prüfprozess umfasst folgende Schritte:

1. IFC-Modell einlesen.
2. Relevante Wandobjekte identifizieren.
3. Benötigte IFC-Attribute und Eigenschaften auslesen.
4. Werte mit den definierten Anforderungen vergleichen.
5. Prüfergebnisse pro Bauteil dokumentieren.
6. Zusammenfassenden Prüfbericht erzeugen.

### 6.3 Output

Als Ergebnis soll ein strukturierter Prüfbericht erzeugt werden.

Das bevorzugte Ausgabeformat ist CSV.

Der Bericht enthält mindestens:

| Feld      | Beschreibung                          |
| --------- | ------------------------------------- |
| GlobalId  | Eindeutige Bauteilkennung             |
| Bauteil   | IFC-Entität                           |
| Regel-ID  | A1, A2 oder A3                        |
| Ist-Wert  | Tatsächlich vorhandener Wert          |
| Soll-Wert | Geforderter Wert bzw. Bedingung       |
| Ergebnis  | Bestanden, durchgefallen oder fehlend |
| Grund     | Beschreibung der Abweichung           |

Zusätzlich wird eine Zusammenfassung erstellt mit:

* Anzahl geprüfter Objekte
* Anzahl bestandener Prüfungen
* Anzahl nicht bestandener Prüfungen
* Anzahl fehlender Pflichtangaben

---

## 7. Validierung der Prüfregeln

Die Funktionsfähigkeit der automatisierten Qualitätsprüfung wird anhand eines positiven und eines negativen Testfalls überprüft.

### 7.1 Positiver Testfall

Ein Wandobjekt enthält alle für die ausgewählte Prüfregel notwendigen Informationen.

**Beispiel Regel A2:**

Wanddicke = 100 mm

Bedingung = Wanddicke > 0

**Erwartetes Ergebnis: BESTANDEN**

### 7.2 Negativer Testfall

Ein Wandobjekt enthält einen fehlerhaften oder fehlenden Wert.

**Beispiel Regel A2:**

Wanddicke = 0 mm

Bedingung = Wanddicke > 0

**Erwartetes Ergebnis: NICHT BESTANDEN**

### 7.3 Vergleichbarkeit

Für beide Testfälle muss dieselbe Prüfregel verwendet werden.

Die Auswahl der Prüfobjekte und die Prüfbedingungen dürfen zwischen dem positiven und dem negativen Test nicht verändert werden.

Nur die zugrunde liegenden Testdaten werden gezielt verändert.

Die tatsächlichen Prüfergebnisse werden nach Durchführung der Tests im Repository dokumentiert.

---

## 8. Prüfwerkzeuge

Für die Umsetzung der automatisierten Qualitätsprüfung kommen folgende Werkzeuge infrage:

| Werkzeug          | Verwendung                                  |
| ----------------- | ------------------------------------------- |
| IFC-Lite          | IFC-Modell analysieren und prüfen           |
| IDS / IDS-Light   | Maschinenlesbare Prüfregeln definieren      |
| IFC Model Checker | Automatisierte Modellprüfung                |
| GitHub            | Versionsverwaltung und Projektdokumentation |

Das endgültig verwendete Werkzeug und seine Version werden nach der technischen Umsetzung dokumentiert.

---

## 9. Dokumentation der Ergebnisse

Die Prüfregeln und Ergebnisse werden im GitHub-Repository von Team 5 gespeichert.

**Repository:**

`bxmid03-hs26-team-5`

Vorgesehene Ablagestruktur:

| Ordner          | Inhalt                                        |
| --------------- | --------------------------------------------- |
| `requirements/` | Projektwahl, AIA-BEP-Matrix und Spezifikation |
| `workflow/`     | Ist- und Sollprozess                          |
| `checks/`       | Maschinenlesbare Prüfregeln und Prüflogik     |
| `results/`      | Positive und negative Prüfergebnisse          |
| `models/`       | Dokumentation der Modellquelle                |

Die genaue Anleitung zur Wiederholung des Prüflaufs wird in der zentralen `README.md` dokumentiert.

---

## 10. Grenzen der Qualitätsprüfung

Die automatisierte Prüfung kontrolliert, ob die definierten Informationen vorhanden sind und die festgelegten Bedingungen erfüllen.

Sie bestätigt jedoch nicht automatisch die fachliche Richtigkeit aller Angaben.

Beispielsweise bedeutet eine positive Wanddicke nicht, dass die Wand konstruktiv korrekt dimensioniert ist.

Ebenso beweist eine vorhandene Materialzuordnung nicht, dass das richtige Material verwendet wurde.

Eine zusätzliche fachliche Prüfung kann deshalb weiterhin notwendig sein.

---

## 11. Offene Punkte und nächste Schritte

Folgende Punkte müssen im weiteren Projektverlauf geklärt werden:

1. Eindeutige Auswahl der relevanten Holzrahmenbauwände.
2. Verbindlichen Datenpfad für die Wanddicke festlegen.
3. IFC-Einheiten korrekt berücksichtigen.
4. Materialinformationen auf Instanz-, Typ- oder Bestandteilebene untersuchen.
5. Eine zentrale Prüfregel technisch umsetzen.
6. Positiven und negativen Testfall mit derselben Regel durchführen.
7. Tatsächliche Prüfergebnisse mit GlobalId dokumentieren.
8. Prüfanleitung in der `README.md` ergänzen.

---

## 12. Fazit

Die Spezifikation definiert drei grundlegende Informationsanforderungen für die automatisierte Qualitätsprüfung von Holzrahmenbauwänden.

Durch die Prüfung von Wandbezeichnung, Wanddicke und Materialzuordnung sollen fehlende oder ungültige Informationen frühzeitig erkannt werden.

Im ersten MVP wird eine zentrale Prüfregel umgesetzt und anhand eines positiven und eines negativen Testfalls validiert.

Die Ergebnisse bilden die Grundlage für eine spätere Erweiterung zu einem umfassenderen automatisierten Qualitätsprüfprozess.

---

## 13. Grundlagen

Diese Spezifikation basiert auf folgenden Projektunterlagen:

* Projektwahl Team 5 – Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
* Projekt Matrix – Team 5
* Teamkarte Team 5 – Holzrahmenwand-QA
* BPMN Ist-/Sollprozess – Team 5
* Unterrichtsunterlagen Data Management & BIM

**Verantwortlich:** Team 5
