# Team 5 – Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

**Modul:** Data Management & BIM
**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen
**Team:** Team 5
**Version:** MVP 0.1
**Stand:** 28.10.2026
**Status:** Erster automatisierter Prüfcheck erfolgreich durchgeführt – weitere Anforderungen in Entwicklung

---

## 1. Projektbeschreibung

### 1.1 Ausgangslage und Problemstellung

Bei der Planung von Holzrahmenbauwänden müssen verschiedene Bauteilinformationen vollständig und korrekt im BIM-Modell hinterlegt sein.

Fehlende Wandbezeichnungen, ungültige Wanddicken oder fehlende Materialzuordnungen können zu Rückfragen, Planungsfehlern und zusätzlichem Aufwand während der Werkplanung führen.

Die Überprüfung dieser Informationen erfolgt häufig manuell und ist entsprechend zeitaufwendig.

### 1.2 Projektziel

Ziel des Projekts ist die Entwicklung eines automatisierten Qualitätschecks für Wandbauteile in IFC-Modellen.

Der Check soll fehlende oder ungültige Informationen erkennen und die Ergebnisse nachvollziehbar dokumentieren.

Dadurch sollen die Datenqualität verbessert, Fehler frühzeitig erkannt und manuelle Prüfprozesse reduziert werden.

Für das erste Minimum Viable Product (MVP 0.1) wird eine ausgewählte Informationsanforderung automatisiert geprüft und anhand positiver und negativer Testdaten validiert.

### 1.3 Zielgruppe

Das Projekt richtet sich an folgende Beteiligte:

* Holzbauplanung
* Arbeitsvorbereitung
* BIM-Koordination
* Fachplanung und Modellautoren

Die BIM-Koordination soll anhand des Prüfberichts erkennen können, welche Wandinformationen den definierten Anforderungen entsprechen und bei welchen Bauteilen eine Korrektur erforderlich ist.

### 1.4 Prüfzeitpunkt

Die Qualitätsprüfung erfolgt bei der IFC-Modellabgabe vor der Werkplanung.

Fehlerhafte Angaben sollen vor der Weiterverarbeitung des Modells erkannt und an die Modellverantwortlichen zurückgemeldet werden.

---

## 2. Informationsanforderungen

Für das Projekt wurden drei zentrale Informationsanforderungen definiert.

| ID | Anforderung       | Prüfkriterium                       | Status MVP 0.1           |
| -- | ----------------- | ----------------------------------- | ------------------------ |
| A1 | Wandbezeichnung   | `Name` vorhanden und nicht leer     | Noch nicht automatisiert |
| A2 | Wanddicke         | `Width` vorhanden und grösser als 0 | Erfolgreich umgesetzt    |
| A3 | Materialzuordnung | Materialzuordnung vorhanden         | Noch nicht automatisiert |

Die Anforderungen sind in der AIA-BEP-Matrix und der technischen Spezifikation dokumentiert.

**Dokumente:**

* `requirements/Team_5_Use-Case.md`
* `requirements/Team_5_AIA-BEP-Matrix.md`
* `requirements/Team_5_Spezifikation.md`

Für das erste MVP wurde die Anforderung **A2 – Wanddicke > 0 m** ausgewählt.

Die weiteren Anforderungen werden in späteren Entwicklungsschritten berücksichtigt.

---

## 3. MVP 0.1 – Automatisierter Wanddickencheck

### 3.1 Ziel des MVP

Das MVP soll automatisch überprüfen, ob ausgewählte Holzrahmenbauwände eine gültige Wanddicke besitzen.

Die Prüfung erfolgt anhand einer definierten Regel, die für einen positiven und einen negativen Testfall unverändert bleibt.

### 3.2 Input

Der automatisierte Check benötigt folgende Eingangsdaten:

1. IFC-Modell mit Wandbauteilen
2. Definierte Prüfregel
3. Festgelegte Objektauswahl
4. Google-Colab-Notebook mit Python-Prüflogik

Die beiden IFC-Testmodelle werden beim Ausführen des Notebooks automatisch aus dem Unterrichtsrepository heruntergeladen.

### 3.3 Verarbeitung

Der Prüfprozess umfasst folgende Schritte:

1. IFC-Modell einlesen.
2. Relevante Wandobjekte anhand der IFC-Klasse identifizieren.
3. Objektauswahl über eine zusätzliche Scope-Eigenschaft einschränken.
4. Wanddicke aus den IFC-Quantities auslesen.
5. Längeneinheit des Modells berücksichtigen.
6. Wanddicke mit dem Grenzwert vergleichen.
7. Prüfergebnis pro Bauteil bestimmen.
8. Ergebnisse als CSV-Datei exportieren.

### 3.4 Output

Als Ergebnis werden zwei CSV-Prüfberichte erzeugt:

* `Team_5_A2_Wanddicke_negativ.csv`
* `Team_5_A2_Wanddicke_positiv.csv`

Jede Ergebniszeile enthält unter anderem:

| Feld                   | Beschreibung                               |
| ---------------------- | ------------------------------------------ |
| `GlobalId`             | Eindeutige Identifikation des IFC-Bauteils |
| `Klasse`               | IFC-Entität                                |
| `Name`                 | Bauteilbezeichnung                         |
| `Datenpfad`            | Geprüfte IFC-Information                   |
| `Ist_Wert_m`           | Tatsächlich ermittelte Wanddicke in Metern |
| `Soll_Regel`           | Angewendete Prüfregel                      |
| `Regel_ID`             | Identifikation der Anforderung             |
| `Ergebnis`             | Bestanden, durchgefallen oder fehlend      |
| `Grund`                | Begründung des Prüfergebnisses             |
| `Modell`               | Verwendete IFC-Datei                       |
| `Modell_SHA256`        | Prüfsumme der Eingabedatei                 |
| `IFC_Schema`           | IFC-Version                                |
| `IfcOpenShell_Version` | Verwendete Bibliotheksversion              |
| `Pruefzeit_UTC`        | Zeitpunkt der Prüfung                      |

Die Ergebnisse werden im Ordner `results/` gespeichert.

---

## 4. Verwendete IFC-Modelle

### 4.1 Ursprüngliches Arbeitsmodell

Für die ursprüngliche Projektentwicklung wurde folgendes IFC-Modell verwendet:

**Dateiname:** `Team_5_BIMcollab_Example_STR.ifc`

**IFC-Version:** IFC2X3

**Speicherort:** `models/Team_5_BIMcollab_Example_STR.ifc`

Das Modell wurde zur Untersuchung von Wandobjekten und deren Eigenschaften verwendet.

Es bildet eine Grundlage für die weitere Projektentwicklung.

**Wichtig:** Der aktuelle automatisierte MVP-Check wurde noch nicht auf diesem IFC2X3-Arbeitsmodell ausgeführt.

### 4.2 Testmodelle für das MVP 0.1

Für den automatisierten Nachweis der Prüfregel wurden zwei IFC4-Lehrmodelle aus den Unterrichtsunterlagen verwendet.

| Testfall       | IFC-Modell                       | IFC-Version |
| -------------- | -------------------------------- | ----------- |
| Negativer Test | `Demo_Holzrahmen_fehlerhaft.ifc` | IFC4        |
| Positiver Test | `Demo_Holzrahmen_korrigiert.ifc` | IFC4        |

Beide Modelle enthalten dieselbe für den Test relevante Wand mit identischer GlobalId.

Der Unterschied liegt in der angegebenen Wanddicke.

**Quelle der Testmodelle:**

GitHub-Repository `LTplus-AG/bxmid03-hs26`

Unterordner: `modelle/`

Die Dateien werden im Notebook automatisch heruntergeladen.

Für eine reproduzierbare Prüfung wird folgender GitHub-Commit verwendet:

`b271ce09e369d73e248a3d11f7af1410d98f4eb7`

Damit wird sichergestellt, dass bei wiederholten Prüfungen derselbe Datenstand verwendet wird.

Weitere Informationen zu den IFC-Modellen befinden sich in `models/README.md`.

---

## 5. Technische Spezifikation der Prüfregel

### 5.1 Prüfregel A2 – Wanddicke

Die zentrale Anforderung lautet:

**Jede ausgewählte Wand muss eine Wanddicke grösser als 0 m besitzen.**

| Parameter                   | Festlegung                       |
| --------------------------- | -------------------------------- |
| Regel-ID                    | A2                               |
| IFC-Version der Testmodelle | IFC4                             |
| IFC-Entität                 | `IfcWall` inklusive Unterklassen |
| Scope-Propertyset           | `CH_Lehre`                       |
| Scope-Property              | `Scope`                          |
| Erforderlicher Scope-Wert   | `Holzrahmen-MVP`                 |
| Quantity-Set                | `Qto_WallBaseQuantities`         |
| Quantity                    | `Width`                          |
| Datenpfad                   | `Qto_WallBaseQuantities.Width`   |
| Datentyp                    | Länge                            |
| Einheit                     | Meter                            |
| Grenzwert                   | `Width > 0`                      |
| Regelumsetzung              | Python mit IfcOpenShell          |

### 5.2 Objektauswahl

Es werden ausschliesslich Wandobjekte ausgewählt, welche folgende Bedingungen erfüllen:

**IFC-Klasse:**

`IfcWall`

**Zusätzlicher Filter:**

`CH_Lehre.Scope = Holzrahmen-MVP`

Dadurch wird die Prüfung auf die im Lehrmodell gekennzeichneten relevanten Wandobjekte beschränkt.

Eine leere Objektauswahl wird nicht als bestandene Prüfung gewertet.

### 5.3 Prüflogik

Die Wanddicke wird aus dem IFC-Modell gelesen und anhand der definierten Bedingung ausgewertet.

| Ist-Wert    | Ergebnis      | Begründung                              |
| ----------- | ------------- | --------------------------------------- |
| Width > 0 m | BESTANDEN     | Wanddicke ist positiv                   |
| Width = 0 m | DURCHGEFALLEN | Wanddicke muss grösser als 0 sein       |
| Width < 0 m | DURCHGEFALLEN | Wanddicke ist ungültig                  |
| Width fehlt | FEHLEND       | Erforderliche Wanddicke nicht vorhanden |

Die Prüfung berücksichtigt die im IFC-Modell hinterlegte Längeneinheit und rechnet die Werte für die Auswertung in Meter um.

**Hinweis:** Die Prüfregel wird als dokumentierte Python-Prüflogik umgesetzt. Für dieses MVP wird keine eigenständige IDS-Datei ausgeführt.

---

## 6. Setup – Voraussetzungen und Software

### 6.1 Benötigte Werkzeuge

Für die Ausführung wird folgende Umgebung benötigt:

| Werkzeug     | Verwendung                            | Version                              |
| ------------ | ------------------------------------- | ------------------------------------ |
| Google Colab | Ausführung des Notebooks              | Webbasierte Umgebung                 |
| Python       | Programmiersprache                    | 3.13.18 beim dokumentierten Prüflauf |
| IfcOpenShell | IFC-Dateien einlesen und analysieren  | 0.9.0.post1                          |
| pandas       | Auswertung und CSV-Export             | 2.2.3 beim dokumentierten Prüflauf   |
| GitHub       | Bereitstellung und Versionsverwaltung | Webplattform                         |

Die Version von IfcOpenShell wird im Notebook festgelegt.

Die Python- und pandas-Versionen entsprechen dem dokumentierten Prüflauf und können bei einer späteren Colab-Ausführung abweichen.

### 6.2 Voraussetzungen

Benötigt werden:

* Computer mit Internetverbindung
* Aktueller Webbrowser
* Google-Konto zur Ausführung in Google Colab
* Zugriff auf das GitHub-Repository von Team 5

Es ist keine lokale Installation von Python erforderlich.

Die benötigten Bibliotheken werden innerhalb der Google-Colab-Umgebung installiert.

### 6.3 Installationsschritt

Das Notebook enthält folgenden Befehl:

```python
%pip -q install ifcopenshell==0.9.0.post1 pandas
```

Dieser wird beim Ausführen der ersten Codezelle automatisch ausgeführt.

Anschliessend werden die tatsächlich verwendeten Versionen angezeigt.

---

## 7. Reproduzierbare Anleitung – Check ausführen

Dieser Abschnitt beschreibt den vollständigen Ablauf, damit eine andere Person den Qualitätscheck selbstständig wiederholen kann.

### Schritt 1 – Google Colab öffnen

Öffne folgende Adresse:

https://colab.research.google.com/

Melde dich mit einem Google-Konto an.

### Schritt 2 – Notebook aus GitHub öffnen

Das benötigte Notebook befindet sich im Team-Repository:

`checks/Team_5_Wanddickencheck.ipynb`

**Direktlink zum Notebook:**

https://colab.research.google.com/github/beckr5/bxmid03-hs26-team-5/blob/main/checks/Team_5_Wanddickencheck.ipynb

Alternativ:

1. In Google Colab auf **Datei → Notebook öffnen** klicken.
2. Den Bereich **GitHub** auswählen.
3. Das Repository `beckr5/bxmid03-hs26-team-5` auswählen oder suchen.
4. Die Datei `checks/Team_5_Wanddickencheck.ipynb` öffnen.

### Schritt 3 – Colab-Laufzeit starten

Nach dem Öffnen des Notebooks:

1. Im Menü **Laufzeit** auswählen.
2. Auf **Alle ausführen** klicken.
3. Falls erforderlich, die Ausführung des Notebooks bestätigen.

Die einzelnen Codezellen werden der Reihe nach ausgeführt.

Eine normale Python-Laufzeit auf der CPU ist ausreichend.

### Schritt 4 – Bibliotheken installieren

Das Notebook installiert automatisch:

`ifcopenshell==0.9.0.post1`

Ausserdem wird pandas verwendet.

Nach der Installation werden die tatsächlich verwendeten Versionen angezeigt.

### Schritt 5 – IFC-Testmodelle laden

Das Notebook lädt automatisch folgende Dateien herunter:

`Demo_Holzrahmen_fehlerhaft.ifc`

`Demo_Holzrahmen_korrigiert.ifc`

Die Modelle werden innerhalb der Colab-Laufzeit im Ordner `testdaten/` gespeichert.

Ein manueller Upload ist für diese beiden Testmodelle nicht notwendig.

Anschliessend werden die SHA-256-Prüfsummen angezeigt.

### Schritt 6 – Prüfregel anwenden

Das Notebook führt die definierte Prüfung aus.

Dabei werden folgende Einstellungen verwendet:

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

Die Bedingung lautet:

`Qto_WallBaseQuantities.Width > 0 m`

**Wichtig:** Die Prüfregel und die Objektauswahl dürfen zwischen dem positiven und negativen Test nicht verändert werden.

### Schritt 7 – Ergebnisse kontrollieren

Nach der Prüfung gibt das Notebook für beide Modelle die Anzahl geprüfter, bestandener, durchgefallener und fehlender Objekte aus.

Die erwarteten Ergebnisse lauten:

**Negativer Test:**

* 1 Wand geprüft
* 0 bestanden
* 1 durchgefallen
* 0 fehlend

**Positiver Test:**

* 1 Wand geprüft
* 1 bestanden
* 0 durchgefallen
* 0 fehlend

Anschliessend führt das Notebook einen automatisierten Vergleich durch.

Dabei wird überprüft, ob:

* Beide Testfälle dieselbe GlobalId verwenden.
* Beide Testfälle dieselbe Objektauswahl verwenden.
* Die Prüfregel unverändert bleibt.
* Der negative Fall durchfällt.
* Der positive Fall besteht.

Bei erfolgreicher Validierung erscheint die Meldung:

> VALIDIERT: Gleiche GlobalId + gleicher Scope + dieselbe Regel; negativer Fall fällt durch, positiver besteht.

### Schritt 8 – CSV-Dateien exportieren

Am Ende des Notebooks werden die beiden Prüfergebnisse als CSV-Dateien gespeichert.

Die Dateien werden zusätzlich in einem ZIP-Archiv zusammengefasst:

`Team_5_A2_Ergebnisse.zip`

Das ZIP-Archiv wird zum Download bereitgestellt.

Nach dem Herunterladen kann es auf dem Computer entpackt werden.

### Schritt 9 – Ergebnisse vergleichen

Die neu erzeugten CSV-Dateien können mit den bereits dokumentierten Ergebnissen verglichen werden.

Diese befinden sich im GitHub-Repository unter:

`results/Team_5_A2_Wanddicke_negativ.csv`

`results/Team_5_A2_Wanddicke_positiv.csv`

Beim Vergleich sollten insbesondere folgende Felder übereinstimmen:

* `GlobalId`
* `Klasse`
* `Datenpfad`
* `Ist_Wert_m`
* `Soll_Regel`
* `Ergebnis`
* `Grund`

Die Zeitangabe `Pruefzeit_UTC` wird bei jedem neuen Prüflauf aktualisiert und muss deshalb nicht identisch sein.

**Damit ist die automatisierte Qualitätsprüfung reproduzierbar dokumentiert.**

---

## 8. Tatsächliche Prüfergebnisse

Der dokumentierte Prüfablauf wurde am **27.10.2026** in Google Colab ausgeführt.

Die Ergebnisse wurden als CSV-Dateien exportiert und im GitHub-Repository abgelegt.

### 8.1 Übersicht

| Kriterium        | Negativer Test                   | Positiver Test                   |
| ---------------- | -------------------------------- | -------------------------------- |
| Modell           | `Demo_Holzrahmen_fehlerhaft.ifc` | `Demo_Holzrahmen_korrigiert.ifc` |
| IFC-Version      | IFC4                             | IFC4                             |
| Geprüfte Objekte | 1                                | 1                                |
| Wanddicke        | 0 m                              | 0.1143 m                         |
| Regel            | Width > 0 m                      | Width > 0 m                      |
| Bestanden        | 0                                | 1                                |
| Durchgefallen    | 1                                | 0                                |
| Fehlend          | 0                                | 0                                |
| Gesamtergebnis   | DURCHGEFALLEN                    | BESTANDEN                        |

### 8.2 Identifikation des geprüften Bauteils

Beide Testmodelle enthalten dieselbe geprüfte Wand.

**GlobalId:**

`2ucZRLBGP4uxZW$9i1VAZ8`

**IFC-Entität:**

`IfcWall`

**Bauteilbezeichnung:**

`Wall #1`

**Datenpfad:**

`Qto_WallBaseQuantities.Width`

### 8.3 Negativer Testfall

**Modell:** `Demo_Holzrahmen_fehlerhaft.ifc`

**Ist-Wert:** 0 m

**Soll-Wert:** Width > 0 m

**Ergebnis:** DURCHGEFALLEN

**Begründung:**

Die angegebene Wanddicke beträgt 0 m und erfüllt deshalb die definierte Anforderung nicht.

### 8.4 Positiver Testfall

**Modell:** `Demo_Holzrahmen_korrigiert.ifc`

**Ist-Wert:** 0.1143 m

**Soll-Wert:** Width > 0 m

**Ergebnis:** BESTANDEN

**Begründung:**

Die Wanddicke beträgt 0.1143 m beziehungsweise 114.3 mm und erfüllt damit die definierte Anforderung.

### 8.5 Validierung

Die automatische Validierung wurde erfolgreich durchgeführt.

Dabei wurde bestätigt, dass dieselbe Wand unter derselben Prüfregel im negativen Fall durchfällt und im positiven Fall besteht.

**Ergebnis: Der automatisierte Check für die Anforderung A2 funktioniert für die beiden verwendeten Lehrmodelle.**

---

## 9. Repository-Struktur

Das GitHub-Repository von Team 5 ist folgendermassen aufgebaut:

```text
bxmid03-hs26-team-5/
│
├── README.md
├── KI_USAGE.md
│
├── requirements/
│   ├── Team_5_Use-Case.md
│   ├── Team_5_AIA-BEP-Matrix.md
│   └── Team_5_Spezifikation.md
│
├── workflow/
│   └── Team_5_BPMN_Ist_Soll.pdf
│
├── checks/
│   ├── README.md
│   └── Team_5_Wanddickencheck.ipynb
│
├── results/
│   ├── README.md
│   ├── Team_5_A2_Wanddicke_negativ.csv
│   └── Team_5_A2_Wanddicke_positiv.csv
│
└── models/
    ├── README.md
    └── Team_5_BIMcollab_Example_STR.ifc
```

### Beschreibung der Ordner

| Ordner          | Inhalt                                                               |
| --------------- | -------------------------------------------------------------------- |
| `requirements/` | Projektwahl, AIA-BEP-Matrix und Spezifikation                        |
| `workflow/`     | BPMN-Darstellung des Ist- und Sollprozesses                          |
| `checks/`       | Automatisierter Qualitätscheck als Colab-Notebook                    |
| `results/`      | Tatsächliche positive und negative Prüfergebnisse                    |
| `models/`       | Ursprüngliches IFC-Arbeitsmodell und Dokumentation der Modellquellen |

Die Datei `KI_USAGE.md` dokumentiert den Einsatz künstlicher Intelligenz während des Projekts.

---

## 10. Prozessbeschreibung

### 10.1 Ist-Prozess

Im bisherigen Prozess werden die Wandinformationen manuell kontrolliert.

Der Ablauf umfasst:

1. Auftraggeber legt die Informationsanforderungen fest.
2. Informationsmanagement gibt die Anforderungen weiter.
3. Fachplanung erstellt das IFC-Modell.
4. IFC-Modell wird zur Prüfung übergeben.
5. Wandinformationen werden manuell kontrolliert.
6. Fehlerhafte Informationen werden zurückgemeldet.
7. Modellautor korrigiert die Angaben.
8. Nach erfolgreicher Prüfung erfolgt die Freigabe.

### 10.2 Soll-Prozess

Im geplanten Soll-Prozess wird die manuelle Prüfung durch einen automatisierten Qualitätscheck unterstützt.

Der Ablauf umfasst:

1. Auftraggeber legt die Informationsanforderungen fest.
2. Informationsmanagement definiert die Prüfregeln.
3. Fachplanung erstellt und liefert das IFC-Modell.
4. Automatisierte Qualitätsprüfung wird durchgeführt.
5. Ergebnisse werden als Prüfbericht dokumentiert.
6. Fehlerhafte Bauteile werden identifiziert.
7. Befunde werden an die Modellverantwortlichen zurückgemeldet.
8. Modellautor korrigiert die fehlerhaften Angaben.
9. Erneute Prüfung und anschliessende Freigabe.

Die Rückmeldung von Befunden über BCF ist Bestandteil des geplanten Soll-Prozesses, jedoch noch nicht Bestandteil des automatisierten MVP 0.1.

Die vollständige Prozessdarstellung befindet sich in:

`workflow/Team_5_BPMN_Ist_Soll.pdf`

---

## 11. Qualitätssicherung und Grenzen

### 11.1 Was wurde nachgewiesen?

Für das MVP 0.1 wurde erfolgreich nachgewiesen, dass:

* Die definierte Wanddickenprüfung automatisch ausgeführt werden kann.
* Die ausgewählten Wandobjekte identifiziert werden.
* Die Wanddicke aus dem IFC-Modell gelesen wird.
* Die Einheit korrekt berücksichtigt wird.
* Ein positiver Wanddickenwert akzeptiert wird.
* Eine Wanddicke von 0 m als Fehler erkannt wird.
* Ergebnisse pro Bauteil mit GlobalId dokumentiert werden.
* Dieselbe Regel für beide Testfälle verwendet wird.

### 11.2 Grenzen der aktuellen Umsetzung

Der bisherige Check prüft ausschliesslich die Anforderung A2 – Wanddicke.

Folgende Aspekte werden noch nicht überprüft:

* Vollständigkeit der Wandbezeichnung (A1)
* Materialzuordnung (A3)
* Konstruktive Plausibilität der Wanddicke
* Fachliche Richtigkeit der Wandkonstruktion
* Vollständigkeit aller Holzrahmenbauwände ausserhalb des definierten Lehrmodell-Scopes

Die bestandene Prüfung bestätigt lediglich, dass die definierte Informationsanforderung erfüllt ist.

Sie bestätigt nicht automatisch, dass das Bauteil konstruktiv korrekt geplant wurde.

### 11.3 Einschränkung des Arbeitsmodells

Das ursprüngliche IFC2X3-Arbeitsmodell von Team 5 wurde noch nicht mit der aktuellen Colab-Prüflogik validiert.

Bei IFC2X3-Modellen können andere IFC-Klassen, Quantity-Bezeichnungen und Datenstrukturen vorkommen.

Beispielsweise:

`IfcWallStandardCase`

oder:

`BaseQuantities.Width`

Für die Anwendung auf das ursprüngliche Arbeitsmodell sind die Objektauswahl und der Datenpfad entsprechend zu untersuchen und zu testen.

---

## 12. Weiterentwicklung

Nach dem erfolgreichen MVP 0.1 sind folgende Entwicklungsschritte vorgesehen:

1. Automatisierte Prüfung der Wandbezeichnung (A1).
2. Automatisierte Prüfung der Materialzuordnung (A3).
3. Zusammenführung der drei Informationsanforderungen.
4. Erweiterung des Checks auf das ursprüngliche IFC-Arbeitsmodell.
5. Verbesserung der Ergebnisdarstellung und Auswertung.
6. Strukturierte Rückmeldung erkannter Fehler.
7. Weiterentwicklung zu einem umfassenderen BIM-Qualitätssicherungsprozess.

Die Ergebnisse des MVP dienen als technische Grundlage für die weitere Umsetzung.

---

## 13. KI-Nutzung

Während der Projektbearbeitung wurde ChatGPT (OpenAI) unterstützend eingesetzt.

Die Unterstützung umfasste insbesondere:

* Strukturierung der Projektdokumentation
* Übertragung der AIA-BEP-Matrix in Markdown
* Formulierung der technischen Spezifikation
* Vorbereitung der automatisierten Prüfregel
* Erstellung und Anpassung des Google-Colab-Notebooks
* Unterstützung bei der Auswertung und Dokumentation der Prüfergebnisse

Die technische Prüfung wurde durch Ausführen des Notebooks in Google Colab überprüft.

Die Verantwortung für die fachliche Richtigkeit und die Ergebnisse liegt beim Projektteam.

Die detaillierte Dokumentation befindet sich in:

`KI_USAGE.md`

---

## 14. Projektverantwortung und Repository

**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
**Team:** Team 5
**Modul:** Data Management & BIM

**GitHub-Repository:**

https://github.com/beckr5/bxmid03-hs26-team-5

Die Beiträge der Teammitglieder werden über die GitHub-Commits nachvollziehbar dokumentiert.

**Aktueller Stand:** Der automatisierte Wanddickencheck A2 wurde erfolgreich anhand eines positiven und eines negativen Testfalls durchgeführt.

Die weiteren Anforderungen A1 und A3 befinden sich in Entwicklung.
