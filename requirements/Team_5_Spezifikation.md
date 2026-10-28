# Spezifikation – Team 5

## Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

**Modul:** Data Management & BIM
**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen
**Team:** Team 5
**Version:** MVP 0.1
**Stand:** 28.10.2026
**Status:** Prüfregel A2 erfolgreich umgesetzt und anhand positiver und negativer Testfälle validiert. Weitere Anforderungen in Entwicklung.

---

## 1. Ziel der Spezifikation

Ziel dieses Projekts ist die Entwicklung einer automatisierten Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen.

Die Prüfung soll sicherstellen, dass relevante Wandbauteile die für die weitere Planung und Werkvorbereitung notwendigen Informationen enthalten.

Fehlende oder ungültige Informationen sollen frühzeitig erkannt und in einem nachvollziehbaren Prüfbericht dokumentiert werden.

Dadurch sollen:

* die Datenqualität von BIM-Modellen verbessert werden,
* fehlerhafte oder unvollständige Bauteilinformationen frühzeitig erkannt werden,
* manuelle Prüfprozesse reduziert werden,
* Rückfragen während der Werkplanung vermieden werden,
* Modellverantwortliche gezielt auf notwendige Korrekturen hingewiesen werden.

Die technische Grundlage bilden definierte Informationsanforderungen, IFC-Datenstrukturen und automatisierte Prüfregeln.

### 1.1 Abgrenzung des MVP 0.1

Für das erste Minimum Viable Product (MVP 0.1) wird eine ausgewählte Informationsanforderung automatisiert umgesetzt.

Diese lautet:

**A2 – Jede ausgewählte Wand muss eine Wanddicke grösser als 0 m besitzen.**

Die Prüfung wird anhand eines positiven und eines negativen IFC-Testmodells validiert.

Die weiteren Anforderungen A1 (Wandbezeichnung) und A3 (Materialzuordnung) bleiben Bestandteil der Projektspezifikation, sind jedoch noch nicht Bestandteil des automatisierten MVP 0.1.

---

## 2. Anwendungsbereich

### 2.1 Zielgruppe

Die Spezifikation richtet sich an folgende Projektbeteiligte:

* Holzbauplanung
* Arbeitsvorbereitung
* BIM-Koordination
* Fachplanung und Modellautoren

Insbesondere soll die BIM-Koordination bei der IFC-Modellabgabe erkennen können, welche Bauteile die festgelegten Informationsanforderungen erfüllen und bei welchen Bauteilen eine Korrektur erforderlich ist.

Die Modellverantwortlichen können die Prüfergebnisse anschliessend zur gezielten Überarbeitung verwenden.

### 2.2 Prüfobjekte

Die fachliche Zielgruppe der Qualitätsprüfung sind Holzrahmenbauwände.

Relevante IFC-Entitäten sind:

* `IfcWall`
* `IfcWallStandardCase`

Für das MVP 0.1 erfolgt die Auswahl über die IFC-Entität `IfcWall` einschliesslich ihrer Unterklassen.

Zusätzlich wird ein Property-Filter verwendet, damit ausschliesslich die für den Test vorgesehenen Wandobjekte berücksichtigt werden.

**Objektauswahl des MVP 0.1:**

| Eigenschaft         | Festlegung                       |
| ------------------- | -------------------------------- |
| IFC-Entität         | `IfcWall` inklusive Unterklassen |
| Propertyset         | `CH_Lehre`                       |
| Property            | `Scope`                          |
| Erforderlicher Wert | `Holzrahmen-MVP`                 |

Eine Wand wird nur dann geprüft, wenn sie die definierte Objektauswahl erfüllt.

Die Auswahl dient der technischen Validierung mit den verwendeten IFC-Lehrmodellen.

Für eine spätere Anwendung auf anderen IFC-Modellen muss untersucht werden, wie die relevanten Holzrahmenbauwände zuverlässig identifiziert werden können.

### 2.3 Prüfzeitpunkt

Die Qualitätsprüfung erfolgt bei der IFC-Modellabgabe vor der Werkplanung.

Sie dient dazu, fehlende oder ungültige Informationen vor der weiteren Verarbeitung des Modells zu erkennen.

Bei festgestellten Fehlern werden die betreffenden Bauteile an die Modellverantwortlichen zur Korrektur zurückgemeldet.

Anschliessend kann eine erneute Prüfung erfolgen.

---

## 3. Informationsanforderungen

Für das Projekt wurden drei zentrale Informationsanforderungen definiert.

| ID | Anforderung       | Beschreibung                                             | Prüfkriterium                   | Umsetzungsstatus         |
| -- | ----------------- | -------------------------------------------------------- | ------------------------------- | ------------------------ |
| A1 | Wandbezeichnung   | Jede relevante Wand benötigt eine Bezeichnung.           | `Name` vorhanden und nicht leer | Noch nicht automatisiert |
| A2 | Wanddicke         | Jede relevante Wand benötigt eine gültige Wanddicke.     | `Width > 0`                     | MVP 0.1 umgesetzt        |
| A3 | Materialzuordnung | Jeder relevanten Wand muss ein Material zugeordnet sein. | Materialzuordnung vorhanden     | Noch nicht automatisiert |

Die Anforderungen basieren auf der Projektwahl und der AIA-BEP-Anforderungsmatrix von Team 5.

**Zugehörige Dokumente:**

* `requirements/Team_5_Use-Case.md`
* `requirements/Team_5_AIA-BEP-Matrix.md`

### 3.1 Zusammenhang zwischen AIA, BEP und automatisierter Prüfung

Die drei Anforderungen werden im Projekt nach folgendem Prinzip betrachtet:

**AIA – Auftraggeber-Informationsanforderungen**

Welche Informationen müssen für die weitere Planung und Werkvorbereitung vorhanden sein?

**BEP – BIM-Abwicklungsplan**

Wer stellt die benötigten Informationen bereit, zu welchem Zeitpunkt werden diese benötigt und wie werden sie geprüft?

**Automatisierter Check**

Wie lassen sich die definierten Anforderungen anhand der vorhandenen IFC-Daten maschinell überprüfen?

Für das erste MVP wird dieser Zusammenhang anhand der Anforderung A2 technisch umgesetzt.

Die zugehörige Prüfregel ist als Python-Logik dokumentiert.

---

## 4. Technische Spezifikation der Prüfregeln

### 4.1 Regel A1 – Wandbezeichnung

**Ziel:** Sicherstellen, dass relevante Wandbauteile eine gültige Bezeichnung besitzen.

| Eigenschaft          | Spezifikation                     |
| -------------------- | --------------------------------- |
| Regel-ID             | A1                                |
| IFC-Entität          | `IfcWall` / `IfcWallStandardCase` |
| Attribut             | `Name`                            |
| Datenpfad            | `IfcWall.Name`                    |
| Datentyp             | Text                              |
| Pflichtfeld          | Ja                                |
| Bedingung            | Wert vorhanden und nicht leer     |
| Geplante Prüfmethode | Automatische Attributprüfung      |
| Status               | Noch nicht automatisiert          |

#### Geplante Prüflogik

Eine Wand besteht die Prüfung, wenn das Attribut `Name` einen Text enthält, der nicht ausschliesslich aus Leerzeichen besteht.

**Positiver Testfall:**

`Name = "Holzrahmenwand EG"`

Erwartetes Ergebnis: BESTANDEN

**Negativer Testfall:**

`Name = ""`

Erwartetes Ergebnis: DURCHGEFALLEN

Ein fehlender Name soll als fehlende Pflichtangabe erkannt werden.

**Hinweis:** Diese Regel ist fachlich spezifiziert, wurde im MVP 0.1 jedoch noch nicht automatisiert umgesetzt oder anhand eigener positiver und negativer IFC-Testfälle validiert.

---

### 4.2 Regel A2 – Wanddicke

**Ziel:** Sicherstellen, dass jede relevante Wand eine vorhandene und positive Wanddicke besitzt.

Diese Regel wurde für das MVP 0.1 technisch umgesetzt und erfolgreich validiert.

#### 4.2.1 Technische Festlegung

| Eigenschaft                 | Spezifikation                           |
| --------------------------- | --------------------------------------- |
| Regel-ID                    | A2                                      |
| IFC-Version der Testmodelle | IFC4                                    |
| IFC-Entität                 | `IfcWall` inklusive Unterklassen        |
| Scope-Propertyset           | `CH_Lehre`                              |
| Scope-Property              | `Scope`                                 |
| Erforderlicher Scope-Wert   | `Holzrahmen-MVP`                        |
| Quantity-Set                | `Qto_WallBaseQuantities`                |
| Quantity                    | `Width`                                 |
| IFC-Datenpfad               | `Qto_WallBaseQuantities.Width`          |
| Datentyp                    | Länge                                   |
| Pflichtfeld                 | Ja                                      |
| Einheit der Auswertung      | Meter                                   |
| Grenzwert                   | `Width > 0 m`                           |
| Prüfmethode                 | Automatisierte Mengenprüfung mit Python |
| Verwendete Bibliothek       | IfcOpenShell 0.9.0.post1                |
| Status                      | Erfolgreich umgesetzt und validiert     |

#### 4.2.2 Prüfbedingung

Eine Wand besteht die Prüfung, wenn:

1. Sie zur festgelegten Objektauswahl gehört.
2. Die Quantity `Width` vorhanden ist.
3. Der Wert numerisch auswertbar ist.
4. Der Wert grösser als 0 ist.

Die Prüfregel lautet:

`Qto_WallBaseQuantities.Width > 0 m`

#### 4.2.3 Berücksichtigung der IFC-Einheiten

IFC-Modelle können unterschiedliche Längeneinheiten verwenden.

Deshalb darf ein ausgelesener Zahlenwert nicht ohne Berücksichtigung der Modell-Einheit interpretiert werden.

Im Python-Notebook wird die im IFC-Modell definierte Längeneinheit mit IfcOpenShell ermittelt.

Die Wanddicke wird für die Auswertung in Meter umgerechnet.

**Beispiele:**

| Ursprünglicher Wert | Wert in Metern |
| ------------------- | -------------- |
| 100 mm              | 0.100 m        |
| 114.3 mm            | 0.1143 m       |
| 0 mm                | 0 m            |

Im MVP 0.1 werden die ausgelesenen Wanddicken in Metern ausgewertet und dokumentiert.

#### 4.2.4 Bewertung der Wanddicke

| Bedingung                                   | Ergebnis      | Begründung                              |
| ------------------------------------------- | ------------- | --------------------------------------- |
| `Width > 0 m`                               | BESTANDEN     | Wanddicke ist positiv                   |
| `Width = 0 m`                               | DURCHGEFALLEN | Wanddicke muss grösser als 0 sein       |
| `Width < 0 m`                               | DURCHGEFALLEN | Wanddicke ist ungültig                  |
| `Width` fehlt                               | FEHLEND       | Erforderliche Wanddicke nicht vorhanden |
| Wert ist nicht als endliche Zahl auswertbar | DURCHGEFALLEN | Ungültiger Zahlenwert                   |

Eine fehlende Wanddicke wird nicht automatisch als Zahlenwert 0 interpretiert.

#### 4.2.5 Umgang mit der Objektauswahl

Das Notebook untersucht alle `IfcWall`-Objekte einschliesslich Unterklassen.

Anschliessend wird die Auswahl über folgende Eigenschaft eingeschränkt:

`CH_Lehre.Scope = Holzrahmen-MVP`

Nur Wandobjekte innerhalb dieses Scopes werden bewertet.

**Wichtig:** Wenn keine Wand die festgelegte Auswahl erfüllt, wird die Prüfung mit einer Fehlermeldung abgebrochen.

Dadurch wird verhindert, dass eine leere Objektauswahl fälschlicherweise als bestandene Qualitätsprüfung erscheint.

#### 4.2.6 Umsetzung in Python

Die wichtigsten Parameter der implementierten Prüfregel lauten:

```python
REGEL_ID = "A2"

ENTITY = "IfcWall"

SCOPE_SET = "CH_Lehre"
SCOPE_FELD = "Scope"
SCOPE_WERT = "Holzrahmen-MVP"

QTO_SET = "Qto_WallBaseQuantities"
QTO_FELD = "Width"

GRENZWERT_M = 0.0
```

Die eigentliche Bewertung erfolgt durch den Vergleich der ausgelesenen Wanddicke mit dem Grenzwert.

Die vollständige Implementierung befindet sich in:

`checks/Team_5_Wanddickencheck.ipynb`

#### 4.2.7 Besonderheiten bei IFC2X3

Das ursprüngliche Arbeitsmodell von Team 5 verwendet IFC2X3.

Bei solchen Modellen können die relevanten Quantity-Bezeichnungen anders organisiert sein.

Ein möglicher Datenpfad lautet:

`BaseQuantities.Width`

Der im MVP verwendete Datenpfad:

`Qto_WallBaseQuantities.Width`

wurde anhand der beiden IFC4-Lehrmodelle validiert.

**Eine automatische Prüfung des ursprünglichen IFC2X3-Modells wurde bisher nicht durchgeführt.**

Der geeignete Datenpfad und die Objektauswahl müssen vor einer Übertragung auf dieses Modell separat untersucht werden.

---

### 4.3 Regel A3 – Materialzuordnung

**Ziel:** Sicherstellen, dass relevanten Wandbauteilen Materialinformationen zugeordnet sind.

| Eigenschaft             | Spezifikation                       |
| ----------------------- | ----------------------------------- |
| Regel-ID                | A3                                  |
| IFC-Entität             | `IfcWall` / `IfcWallStandardCase`   |
| Information             | Materialzuordnung                   |
| Möglicher IFC-Datenpfad | `IfcRelAssociatesMaterial`          |
| Pflichtfeld             | Ja                                  |
| Bedingung               | Materialzuordnung vorhanden         |
| Geplante Prüfmethode    | Prüfung der IFC-Materialbeziehungen |
| Status                  | Noch nicht automatisiert            |

#### Geplante Prüflogik

Eine Wand besteht die Prüfung, wenn eine Materialzuordnung entsprechend dem noch festzulegenden Datenpfad vorhanden ist.

Materialinformationen können sich auf unterschiedlichen Ebenen befinden:

* Direkt am Wandobjekt
* Am zugeordneten Wandtyp
* An einzelnen Bestandteilen oder Materialschichten

Der verbindliche Datenpfad muss anhand eines geeigneten IFC-Modells untersucht und festgelegt werden.

**Positiver Testfall:**

Einer Wand ist eine Materialinformation zugeordnet.

Erwartetes Ergebnis: BESTANDEN

**Negativer Testfall:**

Eine Wand besitzt keine entsprechende Materialzuordnung.

Erwartetes Ergebnis: DURCHGEFALLEN

**Hinweis:** Diese Regel ist fachlich spezifiziert. Eine technische Umsetzung und Validierung wurde bisher noch nicht durchgeführt.

---

## 5. IFC-Datenmodell

### 5.1 Ursprüngliches Arbeitsmodell

Team 5 verwendet für die allgemeine Projektentwicklung folgendes IFC-Modell:

`Team_5_BIMcollab_Example_STR.ifc`

**IFC-Version:** IFC2X3

**Speicherort:**

`models/Team_5_BIMcollab_Example_STR.ifc`

Das Modell dient als Grundlage zur Untersuchung von Wandobjekten und den darin enthaltenen IFC-Informationen.

Es wurde für das MVP 0.1 noch nicht mit der aktuellen Python-Prüflogik validiert.

### 5.2 Verwendete Testmodelle

Für die technische Validierung des automatisierten Wanddickenchecks wurden zwei IFC4-Lehrmodelle verwendet.

| Testfall       | IFC-Modell                       | IFC-Version |
| -------------- | -------------------------------- | ----------- |
| Negativer Test | `Demo_Holzrahmen_fehlerhaft.ifc` | IFC4        |
| Positiver Test | `Demo_Holzrahmen_korrigiert.ifc` | IFC4        |

Die beiden Testmodelle werden vom Colab-Notebook automatisch aus einer festgelegten GitHub-Quelle heruntergeladen.

Die ursprüngliche Bezugsquelle stammt aus den Unterrichtsunterlagen.

**Quellenrepository der Lehrmodelle:**

`LTplus-AG/bxmid03-hs26`

**Verwendeter Commit:**

`b271ce09e369d73e248a3d11f7af1410d98f4eb7`

Diese Angabe bezeichnet ausschliesslich die technische Herkunft der Lehrmodelle und nicht das Abgabe-Repository von Team 5.

### 5.3 Relevante IFC-Informationen

| Information                      | IFC-Struktur                      |
| -------------------------------- | --------------------------------- |
| Eindeutige Bauteilidentifikation | `GlobalId`                        |
| Wandbezeichnung                  | `IfcWall.Name`                    |
| Wanddicke IFC4-Lehrmodell        | `Qto_WallBaseQuantities.Width`    |
| Mögliche Wanddicke IFC2X3        | `BaseQuantities.Width`            |
| Material                         | IFC-Materialzuordnung             |
| Bauteilklasse                    | `IfcWall` / `IfcWallStandardCase` |
| Scope des Lehrmodells            | `CH_Lehre.Scope`                  |

Jedes geprüfte Wandobjekt wird über seine `GlobalId` identifiziert.

Dadurch können Prüfergebnisse einzelnen Bauteilen eindeutig zugeordnet werden.

### 5.4 Identifikation der geprüften Wand

Die beiden IFC4-Testmodelle enthalten dieselbe für den Prüfprozess ausgewählte Wand.

**GlobalId:**

`2ucZRLBGP4uxZW$9i1VAZ8`

**IFC-Entität:**

`IfcWall`

**Bauteilname:**

`Wall #1`

Diese Identifikation ermöglicht einen direkten Vergleich der Ergebnisse aus dem positiven und negativen Test.

---

## 6. Automatisierte Qualitätsprüfung

### 6.1 Input

Die automatisierte Prüfung verwendet folgende Eingangsdaten:

1. IFC4-Testmodell
2. Definierte Prüfregel A2
3. Festgelegte Objektauswahl
4. Python-Code aus dem Google-Colab-Notebook

Die IFC-Dateien werden automatisch aus dem Unterrichtsrepository heruntergeladen.

### 6.2 Verarbeitung

Der implementierte Prüfprozess umfasst folgende Schritte:

1. Benötigte Python-Bibliotheken installieren.
2. IFC-Testmodelle herunterladen.
3. IFC-Dateien mit IfcOpenShell öffnen.
4. Im Modell definierte Längeneinheit ermitteln.
5. `IfcWall`-Objekte identifizieren.
6. Relevante Wandobjekte über `CH_Lehre.Scope` auswählen.
7. `Qto_WallBaseQuantities.Width` auslesen.
8. Wanddicke in Meter umrechnen.
9. Wanddicke mit der Bedingung `Width > 0 m` vergleichen.
10. Prüfergebnis und Begründung pro Wand dokumentieren.
11. Positiven und negativen Testfall automatisch validieren.
12. Ergebnisse als CSV-Dateien exportieren.

### 6.3 Output

Als Ergebnis werden strukturierte CSV-Prüfberichte erzeugt.

Die tatsächlich verwendeten Spalten lauten:

| Feld                   | Beschreibung                          |
| ---------------------- | ------------------------------------- |
| `GlobalId`             | Eindeutige Bauteilkennung             |
| `Klasse`               | IFC-Entität                           |
| `Name`                 | Bauteilbezeichnung                    |
| `Datenpfad`            | Geprüfte IFC-Information              |
| `Ist_Wert_m`           | Tatsächliche Wanddicke in Metern      |
| `Soll_Regel`           | Angewendete Prüfregel                 |
| `Regel_ID`             | Eindeutige Regelbezeichnung           |
| `Ergebnis`             | Bestanden, durchgefallen oder fehlend |
| `Grund`                | Begründung des Prüfergebnisses        |
| `Modell`               | Verwendete IFC-Datei                  |
| `Modell_SHA256`        | SHA-256-Prüfsumme der IFC-Datei       |
| `IFC_Schema`           | IFC-Version                           |
| `IfcOpenShell_Version` | Verwendete Bibliotheksversion         |
| `Pruefzeit_UTC`        | Zeitpunkt der Prüfung                 |

Zusätzlich werden folgende Kennzahlen ausgegeben:

* Anzahl geprüfter Objekte
* Anzahl bestandener Prüfungen
* Anzahl durchgefallener Prüfungen
* Anzahl fehlender Pflichtwerte

### 6.4 Ergebnisdateien

Die Prüfergebnisse werden unter folgenden Dateinamen gespeichert:

`results/Team_5_A2_Wanddicke_negativ.csv`

`results/Team_5_A2_Wanddicke_positiv.csv`

Das Notebook erstellt zusätzlich ein ZIP-Archiv:

`Team_5_A2_Ergebnisse.zip`

Dieses enthält beide CSV-Ergebnisdateien.

---

## 7. Validierung der Prüfregel A2

Die Funktionsfähigkeit des automatisierten Wanddickenchecks wurde anhand eines positiven und eines negativen Testfalls überprüft.

Beide Tests wurden am **27.10.2026** in Google Colab durchgeführt.

### 7.1 Negativer Testfall

**Modell:** `Demo_Holzrahmen_fehlerhaft.ifc`

**IFC-Version:** IFC4

**GlobalId:**

`2ucZRLBGP4uxZW$9i1VAZ8`

**Datenpfad:**

`Qto_WallBaseQuantities.Width`

**Ist-Wert:** 0 m

**Prüfregel:** `Width > 0 m`

**Erwartetes Ergebnis:** DURCHGEFALLEN

**Tatsächliches Ergebnis:** DURCHGEFALLEN

**Begründung:**

Die Wanddicke beträgt 0 m und erfüllt deshalb die definierte Informationsanforderung nicht.

### 7.2 Positiver Testfall

**Modell:** `Demo_Holzrahmen_korrigiert.ifc`

**IFC-Version:** IFC4

**GlobalId:**

`2ucZRLBGP4uxZW$9i1VAZ8`

**Datenpfad:**

`Qto_WallBaseQuantities.Width`

**Ist-Wert:** 0.1143 m

**Prüfregel:** `Width > 0 m`

**Erwartetes Ergebnis:** BESTANDEN

**Tatsächliches Ergebnis:** BESTANDEN

**Begründung:**

Die Wanddicke beträgt 0.1143 m beziehungsweise 114.3 mm und erfüllt damit die definierte Anforderung.

### 7.3 Vergleich der Prüfergebnisse

| Eigenschaft              | Negativer Test | Positiver Test |
| ------------------------ | -------------- | -------------- |
| IFC-Version              | IFC4           | IFC4           |
| Anzahl geprüfter Wände   | 1              | 1              |
| Wanddicke                | 0 m            | 0.1143 m       |
| Prüfregel                | Width > 0 m    | Width > 0 m    |
| Bestandene Prüfungen     | 0              | 1              |
| Durchgefallene Prüfungen | 1              | 0              |
| Fehlende Werte           | 0              | 0              |
| Ergebnis                 | DURCHGEFALLEN  | BESTANDEN      |

### 7.4 Automatische Validierung

Das Notebook führt zusätzliche Kontrollen durch.

Dabei wird überprüft, ob:

1. Beide Testmodelle jeweils genau eine Wand im definierten Scope enthalten.
2. Beide Prüfungen dieselbe GlobalId verwenden.
3. Die Prüfregel unverändert bleibt.
4. Der negative Testfall durchfällt.
5. Der positive Testfall besteht.
6. Der negative Ist-Wert 0 m beträgt.
7. Der positive Ist-Wert grösser als 0 m ist.

Die Validierung wurde erfolgreich durchgeführt.

Das Notebook gibt folgende Meldung aus:

> VALIDIERT: Gleiche GlobalId + gleicher Scope + dieselbe Regel; negativer Fall fällt durch, positiver besteht.

Damit konnte nachgewiesen werden, dass die implementierte Prüfregel A2 bei den beiden verwendeten IFC4-Lehrmodellen korrekt zwischen einem positiven und einem negativen Wanddickenwert unterscheidet.

### 7.5 Dokumentierte Ergebnisse

Die tatsächlichen Prüfergebnisse befinden sich im Repository:

* `results/Team_5_A2_Wanddicke_negativ.csv`
* `results/Team_5_A2_Wanddicke_positiv.csv`

Die Ergebnisse enthalten die GlobalId, den geprüften Datenpfad, den tatsächlichen Wert, die Prüfregel, das Ergebnis und die Begründung.

---

## 8. Prüfwerkzeuge und technische Umgebung

### 8.1 Tatsächlich verwendete Werkzeuge

Für das MVP 0.1 wurden folgende Werkzeuge verwendet:

| Werkzeug     | Verwendung                                  | Version                     |
| ------------ | ------------------------------------------- | --------------------------- |
| Google Colab | Ausführung des Prüfnotebooks                | Webbasierte Umgebung        |
| Python       | Programmierung der Prüfregel                | 3.13.18 beim Prüflauf       |
| IfcOpenShell | Einlesen und Auswerten der IFC-Daten        | 0.9.0.post1                 |
| pandas       | Datenverarbeitung und CSV-Export            | 2.2.3 beim Prüflauf         |
| GitHub       | Versionsverwaltung und Projektdokumentation | Webplattform                |
| CSV          | Dokumentation der Prüfergebnisse            | Semikolon-getrennte Dateien |

IfcOpenShell wird im Notebook auf die Version `0.9.0.post1` festgelegt.

Die Python- und pandas-Versionen entsprechen dem dokumentierten Testlauf und können sich bei späteren Colab-Ausführungen unterscheiden.

### 8.2 Technische Umsetzung

Die Qualitätsprüfung wurde als Python-Code umgesetzt.

Die benötigten Informationen werden mit IfcOpenShell aus den IFC-Modellen gelesen.

Die Bewertung erfolgt durch eine dokumentierte Python-Funktion.

Die Ergebnisse werden mithilfe von pandas ausgewertet und als CSV-Dateien exportiert.

### 8.3 Weitere Werkzeuge

Im Rahmen der Projektentwicklung wurden auch folgende Möglichkeiten betrachtet:

* IFC-Lite
* IDS / IDS-Light
* IFC Model Checker

Für das MVP 0.1 wurde jedoch die Umsetzung mit Google Colab und IfcOpenShell verwendet.

**Eine eigenständige IDS-Datei wurde bisher nicht erstellt oder ausgeführt.**

Die spätere Umsetzung der Prüfregeln in einem maschinenlesbaren IDS-Format bleibt eine mögliche Erweiterung.

### 8.4 Reproduzierbare Ausführung

Die vollständige Anleitung zur Wiederholung der Prüfung befindet sich in der zentralen:

`README.md`

Das ausführbare Notebook befindet sich unter:

`checks/Team_5_Wanddickencheck.ipynb`

Die Prüfung kann in Google Colab ausgeführt werden.

Beim Ausführen werden die benötigten Lehrmodelle automatisch heruntergeladen und die Prüfergebnisse neu berechnet.

---

## 9. Dokumentation der Ergebnisse

Die Prüfregeln und Ergebnisse werden im GitHub-Repository von Team 5 gespeichert.

**Repository:**

`beckr5/bxmid03-hs26-team-5`

### 9.1 Ablagestruktur

| Ordner          | Inhalt                                                           |
| --------------- | ---------------------------------------------------------------- |
| `requirements/` | Projektwahl, AIA-BEP-Matrix und technische Spezifikation         |
| `workflow/`     | BPMN-Darstellung des Ist- und Sollprozesses                      |
| `checks/`       | Ausführbares Colab-Notebook mit der Prüfregel                    |
| `results/`      | Positive und negative Prüfergebnisse                             |
| `models/`       | Ursprüngliches Arbeitsmodell und Dokumentation der Modellquellen |

### 9.2 Nachvollziehbarkeit

Die Nachvollziehbarkeit der Prüfung wird durch folgende Angaben sichergestellt:

* Eindeutige Bauteilidentifikation mittels GlobalId
* Dokumentierter IFC-Datenpfad
* Festgelegter Grenzwert
* Dokumentierte Objektauswahl
* Berücksichtigung der IFC-Längeneinheit
* Tatsächliche Ist-Werte
* Positive und negative Prüfergebnisse
* Dokumentierte Softwareversion
* SHA-256-Prüfsumme der IFC-Testmodelle
* Zeitstempel der Prüfung

Dadurch können die Ergebnisse wiederholt und mit späteren Prüfläufen verglichen werden.

---

## 10. Grenzen der Qualitätsprüfung

Die automatisierte Prüfung kontrolliert, ob die ausgewählten IFC-Informationen vorhanden sind und die definierten Bedingungen erfüllen.

Sie bestätigt jedoch nicht automatisch die fachliche oder konstruktive Richtigkeit aller Bauteilangaben.

### 10.1 Grenzen der Regel A2

Ein bestandener Wanddickencheck bestätigt lediglich, dass:

* der geprüfte Wert vorhanden ist,
* der Wert numerisch auswertbar ist,
* die Wanddicke grösser als 0 ist.

Die Prüfung bestätigt nicht, dass:

* die Wand konstruktiv korrekt dimensioniert ist,
* die Wanddicke den Anforderungen des jeweiligen Bauprojekts entspricht,
* die Wand statisch oder bauphysikalisch geeignet ist.

### 10.2 Grenzen der Objektauswahl

Im MVP 0.1 werden ausschliesslich Wandobjekte geprüft, welche den definierten Lehrmodell-Scope erfüllen.

Andere Wandobjekte werden nicht berücksichtigt.

Die Vollständigkeit aller tatsächlich vorhandenen Holzrahmenbauwände eines Bauprojekts wird dadurch nicht nachgewiesen.

### 10.3 Grenzen der Modellkompatibilität

Der implementierte Check wurde anhand zweier IFC4-Lehrmodelle validiert.

Die Übertragbarkeit auf andere IFC-Versionen und Datenstrukturen muss separat geprüft werden.

Insbesondere wurde das ursprüngliche IFC2X3-Arbeitsmodell von Team 5 noch nicht mit der aktuellen Prüflogik validiert.

### 10.4 Grenzen der Informationsanforderungen

Im MVP 0.1 wird ausschliesslich A2 automatisiert geprüft.

Die Anforderungen A1 und A3 sind fachlich dokumentiert, jedoch noch nicht technisch umgesetzt.

Eine umfassende Prüfung aller Wandinformationen ist deshalb noch nicht Bestandteil des MVP 0.1.

---

## 11. Offene Punkte und nächste Schritte

### 11.1 Bereits abgeschlossene Arbeiten

Folgende Aufgaben wurden im MVP 0.1 umgesetzt:

| Aufgabe                                    | Status        |
| ------------------------------------------ | ------------- |
| Drei Informationsanforderungen definieren  | Abgeschlossen |
| Prüfregel A2 technisch festlegen           | Abgeschlossen |
| IFC4-Datenpfad für A2 bestimmen            | Abgeschlossen |
| Längeneinheiten im Check berücksichtigen   | Umgesetzt     |
| Python-Prüflogik erstellen                 | Abgeschlossen |
| Positiven Test durchführen                 | Erfolgreich   |
| Negativen Test durchführen                 | Erfolgreich   |
| Beide Testfälle mit derselben Regel prüfen | Erfolgreich   |
| Prüfergebnisse mit GlobalId dokumentieren  | Abgeschlossen |
| CSV-Ergebnisse exportieren                 | Abgeschlossen |
| Ausführungsanleitung in README ergänzen    | Abgeschlossen |

### 11.2 Offene Aufgaben

Folgende Punkte bleiben für die weitere Projektentwicklung bestehen:

1. Automatisierte Prüfung der Wandbezeichnung (A1) umsetzen.
2. Materialzuordnung und geeigneten IFC-Datenpfad für A3 untersuchen.
3. Automatisierte Materialprüfung (A3) entwickeln.
4. Prüflogik auf das ursprüngliche IFC2X3-Arbeitsmodell übertragen.
5. Objektauswahl für weitere reale Holzrahmenbauwände festlegen.
6. Die drei Anforderungen in einem gemeinsamen Prüfablauf zusammenführen.
7. Ergebnisdarstellung und Rückmeldung an Modellverantwortliche weiterentwickeln.
8. Den vollständigen Ablauf durch eine andere Person unabhängig wiederholen lassen.

Die Bearbeitung dieser Punkte erfolgt schrittweise im weiteren Projektverlauf.

---

## 12. Fazit

Die vorliegende Spezifikation definiert drei grundlegende Informationsanforderungen für die automatisierte Qualitätsprüfung von Holzrahmenbauwänden.

Diese betreffen:

* A1 – Wandbezeichnung
* A2 – Wanddicke
* A3 – Materialzuordnung

Für das erste MVP 0.1 wurde die Anforderung A2 erfolgreich als automatisierter Python-Check umgesetzt.

Die Prüfung wurde anhand zweier IFC4-Lehrmodelle durchgeführt.

Dabei wurde nachgewiesen, dass dieselbe Wand unter derselben Prüfregel bei einer Wanddicke von 0 m durchfällt und bei einer Wanddicke von 0.1143 m besteht.

Die Ergebnisse wurden mit GlobalId, Ist-Wert, Prüfregel und Begründung dokumentiert.

Damit wurde eine funktionierende technische Grundlage für die weitere Entwicklung der automatisierten Qualitätsprüfung geschaffen.

Die Prüfung weiterer Informationsanforderungen und die Anwendung auf das ursprüngliche IFC2X3-Arbeitsmodell bleiben Bestandteil der nächsten Entwicklungsschritte.

---

## 13. Grundlagen und Projektunterlagen

Diese Spezifikation basiert auf folgenden Unterlagen:

* Projektwahl Team 5 – Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
* AIA-BEP-Anforderungsmatrix von Team 5
* BPMN Ist-/Sollprozess von Team 5
* Unterrichtsunterlagen Data Management & BIM
* IFC-Lehrmodelle aus dem Unterricht
* Automatisiertes Google-Colab-Notebook von Team 5
* Tatsächliche positive und negative Prüfergebnisse
* Zentrale README des GitHub-Repositories

**Weiterführende Projektdokumentation:**

`README.md`

`requirements/Team_5_Use-Case.md`

`requirements/Team_5_AIA-BEP-Matrix.md`

`workflow/Team_5_BPMN_Ist_Soll.pdf`

`checks/Team_5_Wanddickencheck.ipynb`

`results/Team_5_A2_Wanddicke_negativ.csv`

`results/Team_5_A2_Wanddicke_positiv.csv`

`models/README.md`

`KI_USAGE.md`

---

**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
**Team:** Team 5
**Modul:** Data Management & BIM
**Version:** MVP 0.1
**Stand:** 28.10.2026

**GitHub-Repository:**
https://github.com/beckr5/bxmid03-hs26-team-5
