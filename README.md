# Team 5 – Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

**Modul:** Data Management & BIM – BFH
**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen
**Team:** 5
**Version:** MVP 0.1
**Stand:** 28.10.2026

## 1. Projektidee und Ziel

Bei der Planung von Holzrahmenbauwänden müssen verschiedene Informationen im BIM-Modell vorhanden sein. Fehlen beispielsweise Wandbezeichnungen, Wanddicken oder Materialangaben, kann dies bei der Werkplanung zu Problemen führen.

Mit unserem Projekt möchten wir prüfen, wie sich solche Informationen in einem IFC-Modell automatisch kontrollieren lassen.

Der Check soll vor allem die Holzbauplanung, Arbeitsvorbereitung und BIM-Koordination unterstützen.

Für unser erstes MVP haben wir uns auf die **automatische Prüfung der Wanddicke** konzentriert.

## 2. Unsere drei Anforderungen

Wir haben folgende Informationsanforderungen definiert:

| ID | Anforderung       | Prüfregel                    | Status    |
| -- | ----------------- | ---------------------------- | --------- |
| A1 | Wandbezeichnung   | `Name` darf nicht leer sein  | Geplant   |
| A2 | Wanddicke         | `Width > 0`                  | Umgesetzt |
| A3 | Materialzuordnung | Material muss vorhanden sein | Geplant   |

Die Anforderungen sind im Ordner `requirements/` genauer beschrieben.

## 3. Verwendete IFC-Modelle

Unser ursprüngliches Arbeitsmodell ist:

`Team_5_BIMcollab_Example_STR.ifc` (IFC2X3)

Es befindet sich im Ordner `models/`.

Für die automatische Prüfung haben wir zwei IFC4-Lehrmodelle aus den Unterrichtsunterlagen verwendet:

| Modell                           | Verwendung     |
| -------------------------------- | -------------- |
| `Demo_Holzrahmen_fehlerhaft.ifc` | Negativer Test |
| `Demo_Holzrahmen_korrigiert.ifc` | Positiver Test |

Die beiden Lehrmodelle werden vom Colab-Notebook automatisch heruntergeladen.

**Wichtig:** Der bisherige Check wurde mit den beiden Lehrmodellen getestet, noch nicht mit unserem ursprünglichen IFC2X3-Arbeitsmodell.

Weitere Informationen zu den Modellen stehen in `models/README.md`.

## 4. So funktioniert unser Check

Wir haben einen automatisierten Wanddickencheck mit Python und IfcOpenShell erstellt.

Der Check läuft wie folgt ab:

1. IFC-Modell einlesen.
2. Relevante Wandobjekte auswählen.
3. Wanddicke auslesen.
4. Wert mit der Prüfregel vergleichen.
5. Ergebnisse als CSV-Datei ausgeben.

**Unsere Prüfregel A2:**

`Qto_WallBaseQuantities.Width > 0 m`

Geprüft werden `IfcWall`-Objekte inklusive Unterklassen mit folgendem Filter:

`CH_Lehre.Scope = Holzrahmen-MVP`

Die Wanddicke wird unter Berücksichtigung der IFC-Längeneinheit in Meter umgerechnet.

Die Bewertung erfolgt so:

| Wanddicke        | Ergebnis      |
| ---------------- | ------------- |
| Grösser als 0 m  | Bestanden     |
| 0 m oder negativ | Durchgefallen |
| Wert fehlt       | Fehlend       |

Wenn keine passende Wand gefunden wird, bricht der Check mit einer Fehlermeldung ab.

## 5. Check selbst ausführen

Für die Prüfung verwenden wir Google Colab.

**Verwendete Software beim Testlauf:**

| Werkzeug     | Version     |
| ------------ | ----------- |
| Google Colab | Online      |
| Python       | 3.13.18     |
| IfcOpenShell | 0.9.0.post1 |
| pandas       | 2.2.3       |

Die Python- und pandas-Versionen können bei einem späteren Colab-Lauf abweichen. IfcOpenShell wird im Notebook auf die angegebene Version festgelegt.

### Schritt-für-Schritt-Anleitung

**1. Notebook öffnen**

[Team 5 – Wanddickencheck in Google Colab öffnen](https://colab.research.google.com/github/beckr5/bxmid03-hs26-team-5/blob/main/checks/Team_5_Wanddickencheck.ipynb)

Alternativ befindet sich das Notebook im Repository unter:

`checks/Team_5_Wanddickencheck.ipynb`

**2. Prüfung starten**

In Google Colab auf **Laufzeit → Alle ausführen** klicken.

Falls nötig, mit einem Google-Konto anmelden und die Ausführung bestätigen.

**3. Automatische Verarbeitung**

Das Notebook installiert die benötigten Bibliotheken, lädt die beiden Lehrmodelle herunter und prüft die Wanddicke.

Die beiden Tests verwenden dieselbe Objektauswahl und dieselbe Prüfregel.

**4. Ergebnisse kontrollieren**

Für beide Modelle werden die geprüften Wandobjekte und ihre Ergebnisse angezeigt.

Am Ende kontrolliert das Notebook automatisch, ob der negative Test durchfällt und der positive Test besteht.

**5. Ergebnisse herunterladen**

Das Notebook erstellt folgende Dateien:

* `Team_5_A2_Wanddicke_negativ.csv`
* `Team_5_A2_Wanddicke_positiv.csv`

Beide Dateien werden als ZIP-Datei zum Herunterladen bereitgestellt.

## 6. Unsere Testergebnisse

Wir haben den Check am **27.10.2026** in Google Colab ausgeführt.

| Kriterium      | Negativer Test | Positiver Test |
| -------------- | -------------- | -------------- |
| Geprüfte Wände | 1              | 1              |
| Wanddicke      | 0 m            | 0.1143 m       |
| Prüfregel      | Width > 0 m    | Width > 0 m    |
| Ergebnis       | Durchgefallen  | Bestanden      |

**GlobalId der geprüften Wand:**

`2ucZRLBGP4uxZW$9i1VAZ8`

Bei beiden Tests wurde dieselbe Wand mit derselben Prüfregel kontrolliert. Nur die Wanddicke wurde im Testmodell verändert.

Die Ergebnisse zeigen, dass unser Check eine ungültige Wanddicke erkennt und eine positive Wanddicke akzeptiert.

Die tatsächlichen CSV-Ergebnisse sind im Ordner `results/` gespeichert. Sie enthalten unter anderem die GlobalId, den Ist-Wert, die Prüfregel, das Ergebnis und eine Begründung.

## 7. Grenzen und nächste Schritte

Unser erster Check funktioniert mit den beiden IFC4-Lehrmodellen.

Aktuell gibt es aber noch einige Einschränkungen:

* Bisher wird nur die Wanddicke (A2) geprüft.
* Die Wandbezeichnung (A1) und Materialzuordnung (A3) sind noch nicht automatisiert.
* Unser ursprüngliches IFC2X3-Modell wurde noch nicht mit diesem Check getestet.
* Eine positive Wanddicke bedeutet nicht automatisch, dass die Wand konstruktiv richtig aufgebaut ist.

Als Nächstes möchten wir den Check auf weitere Wandinformationen und unser ursprüngliches Arbeitsmodell erweitern.

Die technische Spezifikation, die AIA-BEP-Matrix und der Ist-/Sollprozess befinden sich in den Ordnern `requirements/` und `workflow/`.

Die Verwendung von KI während der Projektbearbeitung ist in `KI_USAGE.md` dokumentiert.

---

**Team 5 – Data Management & BIM**
**GitHub:** https://github.com/beckr5/bxmid03-hs26-team-5
