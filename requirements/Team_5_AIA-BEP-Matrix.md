# Projektmatrix – Team 5

## Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

### AIA-BEP-Anforderungsmatrix

**Modul:** Data Management & BIM
**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen
**Team:** Team 5
**Version:** 0.2 – Aktualisierung nach dem MVP-Test
**Projektstand:** MVP 0.1
**Stand:** 28.10.2026
**Status:** Drei Informationsanforderungen definiert; Anforderung A2 automatisiert umgesetzt und erfolgreich validiert.

---

## 1. Ziel und Zweck der Projektmatrix

Die AIA-BEP-Anforderungsmatrix dokumentiert die Informationsanforderungen, welche für die Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen relevant sind.

Bei der Planung und Werkvorbereitung von Holzrahmenbauwänden müssen bestimmte Bauteilinformationen vollständig und nachvollziehbar vorhanden sein.

Fehlende Wandbezeichnungen, ungültige Wanddicken oder fehlende Materialzuordnungen können zu Rückfragen, Planungsfehlern und zusätzlichem Aufwand führen.

Ziel der Projektmatrix ist es, diese Anforderungen strukturiert zu definieren und die Grundlage für eine automatisierte Überprüfung zu schaffen.

Dabei wird zwischen drei Ebenen unterschieden:

**AIA – Auftraggeber-Informationsanforderungen**

Welche Informationen werden für die weitere Planung und Werkvorbereitung benötigt?

**BEP – BIM-Abwicklungsplan**

Welche Projektbeteiligten stellen die Informationen bereit, zu welchem Zeitpunkt werden sie benötigt und wie erfolgt die Prüfung?

**Automatisierter Qualitätscheck**

Wie können die definierten Informationsanforderungen anhand der vorhandenen IFC-Daten automatisch überprüft werden?

Die Matrix bildet die fachliche Grundlage für die technische Umsetzung der Qualitätsprüfung.

### 1.1 Aktualisierung gegenüber der ursprünglichen Projektmatrix

Die ursprüngliche AIA-BEP-Anforderungsmatrix wurde bereits im Rahmen des Unterrichts eingereicht.

Die vorliegende Version ergänzt die ursprünglichen Anforderungen um die Erkenntnisse aus der technischen Umsetzung des MVP 0.1.

Die drei Informationsanforderungen bleiben unverändert:

* A1 – Wandbezeichnung
* A2 – Wanddicke
* A3 – Materialzuordnung

Neu dokumentiert werden insbesondere die tatsächlich verwendeten IFC-Datenpfade, die Objektauswahl, das Prüfwerkzeug sowie der technische Umsetzungs- und Validierungsstand.

Für das MVP 0.1 wurde ausschliesslich die Anforderung A2 automatisiert umgesetzt.

---

## 2. AIA-BEP-Anforderungsmatrix

| Anforderung             | 1. Wandbezeichnung (A1)                                               | 2. Wanddicke (A2)                                                               | 3. Materialzuordnung (A3)                                             |
| ----------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Zweck**               | Wände eindeutig ansprechen und fehlende Bezeichnungen erkennen        | Vorhandensein einer gültigen Wanddicke sicherstellen                            | Sicherstellen, dass jeder relevanten Wand ein Material zugeordnet ist |
| **Meilenstein**         | IFC-Modellabgabe vor der Werkplanung                                  | IFC-Modellabgabe vor der Werkplanung                                            | IFC-Modellabgabe vor der Werkplanung                                  |
| **Objekte**             | Relevante `IfcWall`-Objekte                                           | Relevante `IfcWall`-Objekte                                                     | Relevante `IfcWall`-Objekte                                           |
| **Property / Quantity** | `IfcWall.Name`                                                        | IFC4: `Qto_WallBaseQuantities.Width`                                            | IFC-Materialzuordnung, z. B. `IfcRelAssociatesMaterial`               |
| **Werte / Anforderung** | Text vorhanden, nicht leer und nicht ausschliesslich Leerzeichen      | Numerischer Wert vorhanden und grösser als 0; IFC-Längeneinheit berücksichtigen | Materialzuordnung vorhanden                                           |
| **Prüfung**             | Automatisch prüfen, ob eine nicht leere Wandbezeichnung vorhanden ist | Wanddicke auslesen, in Meter umrechnen und auf `Width > 0 m` prüfen             | Automatisch prüfen, ob eine Materialzuordnung vorhanden ist           |
| **Wer liefert?**        | Fachplanung / Modellautor                                             | Fachplanung / Modellautor                                                       | Fachplanung / Modellautor                                             |
| **Wer prüft?**          | Prüfende Stelle / BIM-Koordination gemäss Projektorganisation         | Prüfende Stelle / BIM-Koordination gemäss Projektorganisation                   | Prüfende Stelle / BIM-Koordination gemäss Projektorganisation         |
| **Quelle**              | Eigene Projektanforderung gemäss Projektwahl                          | Eigene Projektanforderung gemäss Projektwahl                                    | Eigene Projektanforderung gemäss Projektwahl                          |
| **Technischer Status**  | Noch nicht automatisiert                                              | Im MVP 0.1 umgesetzt                                                            | Noch nicht automatisiert                                              |
| **Validierung**         | Noch nicht durchgeführt                                               | Positiver und negativer IFC4-Testfall erfolgreich geprüft                       | Noch nicht durchgeführt                                               |

**Hinweis:** Die drei Anforderungen sind fachlich definiert. Die technische Umsetzung und Validierung erfolgten bisher ausschliesslich für A2 – Wanddicke.

Die angegebene prüfende Rolle beschreibt die vorgesehene Verantwortlichkeit im BIM-Prozess. Die konkrete organisatorische Zuständigkeit muss im jeweiligen Projekt festgelegt werden.

---

## 3. Konkretisierung der Informationsanforderungen

### 3.1 A1 – Wandbezeichnung

**Ziel:** Sicherstellen, dass jede relevante Wand eine vorhandene und nicht leere Bezeichnung besitzt.

| Eigenschaft          | Festlegung                        |
| -------------------- | --------------------------------- |
| Regel-ID             | A1                                |
| IFC-Entität          | `IfcWall` / `IfcWallStandardCase` |
| Attribut             | `Name`                            |
| Datenpfad            | `IfcWall.Name`                    |
| Datentyp             | Text                              |
| Pflichtfeld          | Ja                                |
| Bedingung            | Text vorhanden und nicht leer     |
| Geplante Prüfmethode | Automatische Attributprüfung      |
| Status               | Noch nicht automatisiert          |

Die Wandbezeichnung soll verhindern, dass relevante Wandbauteile ohne beschreibende Angaben weiterverarbeitet werden.

**Geplante Prüfregel:**

Das Attribut `Name` muss einen Text enthalten, der nicht ausschliesslich aus Leerzeichen besteht.

**Positiver Beispielfall:**

`Name = "Holzrahmenwand EG"`

Erwartetes Ergebnis: BESTANDEN

**Negativer Beispielfall:**

`Name = ""`

Erwartetes Ergebnis: DURCHGEFALLEN

Die Regel prüft die Vollständigkeit der Bezeichnung, nicht deren projektweite Einzigartigkeit.

**Umsetzungsstand:** Die Anforderung ist definiert, wurde jedoch noch nicht als automatisierter IFC-Check umgesetzt oder anhand eigener IFC-Testfälle validiert.

---

### 3.2 A2 – Wanddicke

**Ziel:** Sicherstellen, dass jede relevante Wand eine vorhandene und positive Wanddicke besitzt.

Diese Anforderung wurde für das MVP 0.1 technisch umgesetzt.

#### Technische Festlegung

| Eigenschaft                 | Festlegung                               |
| --------------------------- | ---------------------------------------- |
| Regel-ID                    | A2                                       |
| IFC-Version der Testmodelle | IFC4                                     |
| IFC-Entität                 | `IfcWall` inklusive Unterklassen         |
| Quantity-Set                | `Qto_WallBaseQuantities`                 |
| Quantity                    | `Width`                                  |
| Datenpfad                   | `Qto_WallBaseQuantities.Width`           |
| Datentyp                    | Länge                                    |
| Einheit der Auswertung      | Meter                                    |
| Pflichtfeld                 | Ja                                       |
| Bedingung                   | `Width > 0 m`                            |
| Prüfmethode                 | Automatisierte Mengenprüfung             |
| Werkzeug                    | Google Colab mit Python und IfcOpenShell |
| Status                      | Erfolgreich umgesetzt und validiert      |

#### Objektauswahl

Für das MVP 0.1 werden die relevanten Wände über folgende Bedingungen ausgewählt:

**IFC-Entität:**

`IfcWall`

**Zusätzlicher Filter:**

`CH_Lehre.Scope = Holzrahmen-MVP`

Die Auswahl umfasst `IfcWall` und deren Unterklassen.

Der zusätzliche Scope-Filter stellt sicher, dass im Lehrmodell nur die für den Test gekennzeichneten Wandobjekte berücksichtigt werden.

Diese Auswahl wurde für die verwendeten Lehrmodelle festgelegt und ist nicht automatisch auf andere Bauprojekte übertragbar.

#### Prüfregel

Die automatisierte Prüfung untersucht, ob die Wanddicke vorhanden, numerisch auswertbar und grösser als 0 m ist.

Die Regel lautet:

`Qto_WallBaseQuantities.Width > 0 m`

Die im IFC-Modell definierte Längeneinheit wird berücksichtigt.

Der ausgelesene Wert wird für den Vergleich in Meter umgerechnet.

#### Bewertung

| Bedingung                               | Ergebnis      |
| --------------------------------------- | ------------- |
| Wanddicke > 0 m                         | BESTANDEN     |
| Wanddicke = 0 m                         | DURCHGEFALLEN |
| Wanddicke < 0 m                         | DURCHGEFALLEN |
| Wanddicke fehlt                         | FEHLEND       |
| Wanddicke ist kein endlicher Zahlenwert | DURCHGEFALLEN |

Wenn keine Wand die festgelegte Objektauswahl erfüllt, wird die Prüfung mit einer Fehlermeldung abgebrochen.

Dadurch wird verhindert, dass eine leere Objektauswahl fälschlicherweise als bestandene Prüfung interpretiert wird.

#### Besonderheit bei IFC2X3

Das ursprüngliche Arbeitsmodell von Team 5 verwendet IFC2X3.

Bei IFC2X3-Modellen kann die Wanddicke beispielsweise über folgenden Datenpfad bereitgestellt werden:

`BaseQuantities.Width`

Dieser mögliche Datenpfad wurde im aktuellen MVP 0.1 noch nicht automatisch validiert.

Die bisherige technische Validierung bezieht sich auf den IFC4-Datenpfad der verwendeten Lehrmodelle.

---

### 3.3 A3 – Materialzuordnung

**Ziel:** Sicherstellen, dass jeder relevanten Wand eine Materialinformation zugeordnet ist.

| Eigenschaft             | Festlegung                                       |
| ----------------------- | ------------------------------------------------ |
| Regel-ID                | A3                                               |
| IFC-Entität             | `IfcWall` / `IfcWallStandardCase`                |
| Information             | Materialzuordnung                                |
| Möglicher IFC-Datenpfad | `IfcRelAssociatesMaterial`                       |
| Pflichtfeld             | Ja                                               |
| Bedingung               | Materialzuordnung vorhanden                      |
| Geplante Prüfmethode    | Automatische Prüfung der IFC-Materialbeziehungen |
| Status                  | Noch nicht automatisiert                         |

Die Materialzuordnung kann in einem IFC-Modell auf unterschiedlichen Ebenen hinterlegt sein.

Mögliche Zuordnungen sind:

* Direkt am Wandobjekt
* Am zugehörigen Wandtyp
* An einzelnen Materialschichten oder Bestandteilen

Vor der technischen Umsetzung muss der relevante Materialpfad anhand eines geeigneten IFC-Modells festgelegt werden.

**Geplante Prüfregel:**

Für jede relevante Wand muss eine Materialzuordnung vorhanden sein.

**Positiver Beispielfall:**

Eine Wand besitzt eine Materialzuordnung.

Erwartetes Ergebnis: BESTANDEN

**Negativer Beispielfall:**

Eine Wand besitzt keine Materialzuordnung.

Erwartetes Ergebnis: DURCHGEFALLEN

**Umsetzungsstand:** Die Anforderung ist definiert, wurde jedoch noch nicht automatisiert umgesetzt oder validiert.

---

## 4. Technische Umsetzung des MVP 0.1

### 4.1 Verwendetes Prüfwerkzeug

Für das erste MVP wurde die Anforderung A2 als automatisierter Python-Check umgesetzt.

| Werkzeug     | Verwendung                           | Version                              |
| ------------ | ------------------------------------ | ------------------------------------ |
| Google Colab | Ausführung des Notebooks             | Webbasierte Umgebung                 |
| Python       | Ausführung der Prüflogik             | 3.13.18 beim dokumentierten Prüflauf |
| IfcOpenShell | Einlesen und Auswerten der IFC-Daten | 0.9.0.post1                          |
| pandas       | Verarbeitung und CSV-Export          | 2.2.3 beim dokumentierten Prüflauf   |
| GitHub       | Versionsverwaltung und Dokumentation | Webplattform                         |

Die Prüfung wird als eigene Python-Prüflogik durchgeführt.

Eine eigenständige IDS-Datei wurde für das MVP 0.1 nicht erstellt oder ausgeführt.

### 4.2 Ausführbare Prüfdatei

Der automatisierte Check befindet sich unter:

`checks/Team_5_Wanddickencheck.ipynb`

Das Notebook führt folgende Schritte aus:

1. Benötigte Bibliotheken installieren.
2. IFC-Testmodelle herunterladen.
3. Relevante Wandobjekte auswählen.
4. Wanddicke über den definierten IFC-Datenpfad auslesen.
5. IFC-Längeneinheit berücksichtigen.
6. Wanddicke mit dem Grenzwert vergleichen.
7. Ergebnisse pro Wand dokumentieren.
8. Positiven und negativen Testfall validieren.
9. Prüfergebnisse als CSV-Dateien exportieren.

Die genaue Anleitung befindet sich in der zentralen `README.md`.

---

## 5. Validierung der Anforderung A2

### 5.1 Verwendete IFC-Testmodelle

Für die Validierung wurden zwei IFC4-Lehrmodelle verwendet.

| Testfall       | Modell                           |
| -------------- | -------------------------------- |
| Negativer Test | `Demo_Holzrahmen_fehlerhaft.ifc` |
| Positiver Test | `Demo_Holzrahmen_korrigiert.ifc` |

Die Modelle werden vom Colab-Notebook aus der in den Unterrichtsunterlagen angegebenen Modellquelle heruntergeladen.

Das ursprüngliche Arbeitsmodell von Team 5:

`Team_5_BIMcollab_Example_STR.ifc`

wurde noch nicht mit der aktuellen Prüflogik validiert.

### 5.2 Tatsächliche Prüfergebnisse

Die Prüfung wurde am **27.10.2026** in Google Colab durchgeführt.

| Eigenschaft                | Negativer Test                 | Positiver Test                 |
| -------------------------- | ------------------------------ | ------------------------------ |
| IFC-Version                | IFC4                           | IFC4                           |
| IFC-Entität                | `IfcWall`                      | `IfcWall`                      |
| Anzahl geprüfter Wände     | 1                              | 1                              |
| Geprüfter Datenpfad        | `Qto_WallBaseQuantities.Width` | `Qto_WallBaseQuantities.Width` |
| Wanddicke                  | 0 m                            | 0.1143 m                       |
| Prüfregel                  | `Width > 0 m`                  | `Width > 0 m`                  |
| Bestandene Prüfungen       | 0                              | 1                              |
| Durchgefallene Prüfungen   | 1                              | 0                              |
| Fehlende Werte             | 0                              | 0                              |
| **Tatsächliches Ergebnis** | **DURCHGEFALLEN**              | **BESTANDEN**                  |

### 5.3 GlobalId der geprüften Wand

Beide Testmodelle enthalten dieselbe für den Test ausgewählte Wand.

**GlobalId:**

`2ucZRLBGP4uxZW$9i1VAZ8`

**IFC-Entität:**

`IfcWall`

**Bauteilname:**

`Wall #1`

Dadurch können die beiden Testfälle direkt miteinander verglichen werden.

### 5.4 Vergleichbarkeit der Testfälle

Für beide Testfälle wurden folgende Einstellungen unverändert beibehalten:

* IFC-Entität: `IfcWall`
* Objektauswahl: `CH_Lehre.Scope = Holzrahmen-MVP`
* Datenpfad: `Qto_WallBaseQuantities.Width`
* Prüfregel: `Width > 0 m`

Nur die zugrunde liegenden Testdaten unterscheiden sich.

Der negative Testfall enthält eine Wanddicke von 0 m.

Der positive Testfall enthält eine Wanddicke von 0.1143 m.

Die automatische Validierung wurde erfolgreich durchgeführt.

**Damit wurde nachgewiesen, dass die Prüfregel A2 anhand der beiden verwendeten IFC4-Lehrmodelle korrekt zwischen einem positiven und einem negativen Wanddickenwert unterscheidet.**

### 5.5 Dokumentation der Ergebnisse

Die tatsächlich erzeugten Prüfergebnisse befinden sich im GitHub-Repository unter:

`results/Team_5_A2_Wanddicke_negativ.csv`

`results/Team_5_A2_Wanddicke_positiv.csv`

Die CSV-Dateien enthalten unter anderem:

* GlobalId
* IFC-Klasse
* Bauteilname
* Geprüfter Datenpfad
* Tatsächliche Wanddicke in Metern
* Prüfregel
* Ergebnis
* Begründung
* Verwendetes IFC-Modell
* Softwareversion
* Prüfzeitpunkt

---

## 6. Verantwortlichkeiten und Informationsfluss

Die Informationsanforderungen werden im Rahmen der BIM-Projektorganisation verschiedenen Aufgaben zugeordnet.

| Rolle                                 | Vorgesehene Aufgabe                                                                 |
| ------------------------------------- | ----------------------------------------------------------------------------------- |
| Auftraggeber / Informationsmanagement | Informationsbedarf und Anforderungen festlegen                                      |
| Fachplanung / Modellautor             | IFC-Modell erstellen und erforderliche Bauteilinformationen bereitstellen           |
| Prüfende Stelle / BIM-Koordination    | Informationsanforderungen anhand des IFC-Modells kontrollieren                      |
| Modellverantwortliche                 | Erkannte Fehler korrigieren                                                         |
| Projektverantwortliche                | Prüfung der Ergebnisse und weitere Freigabe gemäss Projektorganisation koordinieren |

Die konkrete Rollenverteilung ist abhängig von der jeweiligen Projektorganisation und muss projektspezifisch bestätigt werden.

Im MVP 0.1 wurde die technische Prüfregel durch Team 5 umgesetzt und getestet.

### 6.1 Informationsfluss

Der vorgesehene Informationsfluss umfasst folgende Schritte:

1. Informationsanforderungen werden definiert.
2. Fachplanung erstellt das IFC-Modell.
3. Das IFC-Modell wird zur Qualitätsprüfung bereitgestellt.
4. Die relevanten Bauteilinformationen werden automatisch geprüft.
5. Prüfergebnisse werden dokumentiert.
6. Fehlerhafte Angaben werden an die Modellverantwortlichen zurückgemeldet.
7. Das Modell wird korrigiert.
8. Die Qualitätsprüfung wird erneut durchgeführt.

Die Prozessdarstellung befindet sich unter:

`workflow/Team_5_BPMN_Ist_Soll.pdf`

---

## 7. Grenzen und offene Punkte

### 7.1 Grenzen des MVP 0.1

Die bisherige technische Umsetzung beschränkt sich auf die Prüfung der Wanddicke.

Ein bestandener Check bestätigt, dass der geprüfte Wert vorhanden und grösser als 0 m ist.

Er bestätigt nicht automatisch, dass:

* die Wand konstruktiv korrekt dimensioniert ist,
* die Wanddicke den Anforderungen des Bauprojekts entspricht,
* sämtliche Holzrahmenbauwände des Modells berücksichtigt wurden,
* die Wandbezeichnung vollständig ist,
* eine gültige Materialzuordnung vorhanden ist.

Die beiden letztgenannten Punkte werden durch die geplanten Anforderungen A1 und A3 abgedeckt.

### 7.2 Offene Aufgaben

Für die weitere Projektentwicklung bestehen folgende Aufgaben:

1. Automatisierte Prüfung der Wandbezeichnung A1 umsetzen.
2. IFC-Materialzuordnung untersuchen und Prüfregel A3 entwickeln.
3. Die drei Anforderungen in einem gemeinsamen Prüfablauf zusammenführen.
4. Das ursprüngliche IFC2X3-Arbeitsmodell mit einer entsprechend angepassten Prüflogik überprüfen.
5. Eine geeignete Objektauswahl für weitere Holzrahmenbauwände festlegen.
6. Die Ergebnisse in einen strukturierten BIM-Qualitätssicherungsprozess integrieren.

---

## 8. Fazit

Die AIA-BEP-Anforderungsmatrix bildet die Grundlage für die automatisierte Qualitätsprüfung von Holzrahmenbauwänden.

Die drei ursprünglichen Informationsanforderungen wurden beibehalten:

**A1 – Wandbezeichnung**

**A2 – Wanddicke**

**A3 – Materialzuordnung**

Mit dem MVP 0.1 wurde die Anforderung A2 erfolgreich technisch umgesetzt und validiert.

Die Prüfung konnte anhand eines positiven und eines negativen IFC4-Testfalls zeigen, dass eine ungültige Wanddicke automatisch erkannt und eine positive Wanddicke akzeptiert wird.

Die Prüfergebnisse wurden nachvollziehbar dokumentiert.

Die Anforderungen A1 und A3 bleiben Bestandteil der weiteren Projektentwicklung.

Damit stellt die aktualisierte Matrix den Zusammenhang zwischen Informationsbedarf, Informationsbereitstellung und technischer Qualitätsprüfung dar.

---

## 9. Zugehörige Projektunterlagen

Die Matrix ist Bestandteil der Projektdokumentation von Team 5.

| Datei                                     | Inhalt                                            |
| ----------------------------------------- | ------------------------------------------------- |
| `README.md`                               | Projektbeschreibung und reproduzierbare Anleitung |
| `requirements/Team_5_Use-Case.md`         | Projektwahl und Use Case                          |
| `requirements/Team_5_Spezifikation.md`    | Technische Spezifikation der Prüfregeln           |
| `workflow/Team_5_BPMN_Ist_Soll.pdf`       | Darstellung des Ist- und Sollprozesses            |
| `checks/Team_5_Wanddickencheck.ipynb`     | Automatisierter Wanddickencheck                   |
| `results/Team_5_A2_Wanddicke_negativ.csv` | Negatives Prüfergebnis                            |
| `results/Team_5_A2_Wanddicke_positiv.csv` | Positives Prüfergebnis                            |
| `models/README.md`                        | Dokumentation der IFC-Modelle                     |
| `KI_USAGE.md`                             | Dokumentation der KI-Nutzung                      |

---

**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
**Team:** Team 5
**Modul:** Data Management & BIM
**Version:** 0.2
**Projektstand:** MVP 0.1
**Stand:** 28.10.2026

**GitHub-Repository:**
https://github.com/beckr5/bxmid03-hs26-team-5
