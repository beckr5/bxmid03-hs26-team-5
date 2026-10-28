# Team 5 – Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

**Modul:** Data Management & BIM – BFH
**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen
**Team:** 5
**Version:** MVP 0.1
**Stand:** 28.10.2026

## 1. Projektidee und Ziel

Bei der Planung von Holzrahmenbauwänden müssen wichtige Informationen im BIM-Modell vorhanden sein. Fehlende Wandbezeichnungen, Wanddicken oder Materialangaben können bei der Werkplanung zu Problemen führen.

Wir möchten untersuchen, wie sich solche Informationen in IFC-Modellen automatisch prüfen lassen.

Für unser erstes MVP haben wir die **automatische Prüfung der Wanddicke** umgesetzt.

## 2. Unsere drei Anforderungen

Wir haben folgende Informationsanforderungen definiert:

| ID | Anforderung       | Prüfregel                    | Status    |
| -- | ----------------- | ---------------------------- | --------- |
| A1 | Wandbezeichnung   | `Name` darf nicht leer sein  | Geplant   |
| A2 | Wanddicke         | `Width > 0 m`                | Umgesetzt |
| A3 | Materialzuordnung | Material muss vorhanden sein | Geplant   |

Die Anforderungen sind im Ordner `requirements/` genauer beschrieben.

## 3. Verwendete IFC-Modelle

Wir haben unsere Prüfung mit zwei unterschiedlichen IFC-Versionen getestet.

| IFC-Modell                         | Version | Verwendung                       |
| ---------------------------------- | ------- | -------------------------------- |
| `Demo_Holzrahmen_fehlerhaft.ifc`   | IFC4    | Negativer Test                   |
| `Demo_Holzrahmen_korrigiert.ifc`   | IFC4    | Positiver Test                   |
| `Team_5_BIMcollab_Example_STR.ifc` | IFC2X3  | Zusätzlicher Test mit 114 Wänden |

Die beiden IFC4-Lehrmodelle stammen aus den Unterrichtsunterlagen.

Das BIMcollab-Beispielmodell befindet sich in unserem Repository unter `models/`.

Mit dem zusätzlichen IFC2X3-Test möchten wir zeigen, dass unsere Prüflogik auch auf ein anderes IFC-Modell übertragen werden kann.

## 4. So funktioniert unser Check

Wir haben die Prüfung mit **Python und IfcOpenShell** umgesetzt.

Der Ablauf ist bei beiden IFC-Versionen ähnlich:

1. IFC-Modell einlesen.
2. Wandobjekte auswählen.
3. Wanddicke auslesen und in Meter umrechnen.
4. Wert mit der Prüfregel vergleichen.
5. Ergebnisse als CSV-Datei speichern.

**Unsere Prüfregel A2 lautet:**

`Wanddicke > 0 m`

Die Datenpfade und die Auswahl der Wände unterscheiden sich je nach Modell:

|           | IFC4-Lehrmodelle                         | IFC2X3-BIMcollab-Modell      |
| --------- | ---------------------------------------- | ---------------------------- |
| Objekte   | `IfcWall` inkl. Unterklassen             | `IfcWall` inkl. Unterklassen |
| Auswahl   | Filter `CH_Lehre.Scope = Holzrahmen-MVP` | Alle Wandobjekte             |
| Wanddicke | `Qto_WallBaseQuantities.Width`           | `BaseQuantities.Width`       |

Die Ergebnisse werden als **bestanden**, **durchgefallen** oder **fehlend** ausgegeben.

Ein Wert grösser als 0 m gilt als bestanden. Fehlende, ungültige oder nicht positive Werte erfüllen unsere Anforderung nicht.

## 5. Checks selbst ausführen

Unsere beiden Notebooks befinden sich im Ordner `checks/`.

### IFC4 – Positiver und negativer Test

[Wanddickencheck IFC4 in Google Colab öffnen](https://colab.research.google.com/github/beckr5/bxmid03-hs26-team-5/blob/main/checks/Team_5_Wanddickencheck.ipynb)

Datei: `checks/Team_5_Wanddickencheck.ipynb`

Das Notebook lädt die beiden IFC4-Lehrmodelle automatisch herunter und prüft die Wanddicke mit derselben Regel.

### IFC2X3 – Ursprüngliches Arbeitsmodell

[Wanddickencheck IFC2X3 in Google Colab öffnen](https://colab.research.google.com/github/beckr5/bxmid03-hs26-team-5/blob/main/checks/Team_5_IFC2X3_Wanddickencheck.ipynb)

Datei: `checks/Team_5_IFC2X3_Wanddickencheck.ipynb`

Dieses Notebook lädt unser BIMcollab-Modell aus dem GitHub-Repository und prüft alle 114 Wandobjekte.

### Ausführung

1. Gewünschtes Notebook in Google Colab öffnen.
2. **Laufzeit → Alle ausführen** wählen.
3. Ergebnisse kontrollieren.
4. Die erzeugte CSV-Datei herunterladen.

Für die Ausführung verwenden wir IfcOpenShell `0.9.0.post1` und pandas.

## 6. Unsere Testergebnisse

### Test 1 – IFC4-Lehrmodelle

Der erste Check wurde am **27.10.2026** durchgeführt.

| Testfall            | Wanddicke | Ergebnis      |
| ------------------- | --------- | ------------- |
| Fehlerhaftes Modell | 0 m       | Durchgefallen |
| Korrigiertes Modell | 0.1143 m  | Bestanden     |

Bei beiden Tests wurde dieselbe Wand mit derselben Prüfregel kontrolliert.

**GlobalId:** `2ucZRLBGP4uxZW$9i1VAZ8`

### Test 2 – IFC2X3-BIMcollab-Modell

Zusätzlich haben wir unsere Wanddickenprüfung am **28.10.2026** auf das ursprüngliche IFC2X3-Modell übertragen.

| Ergebnis       | Anzahl |
| -------------- | -----: |
| Geprüfte Wände |    114 |
| Bestanden      |    114 |
| Durchgefallen  |      0 |
| Fehlende Werte |      0 |

Die Wanddicken verteilen sich wie folgt:

| Wanddicke |  Anzahl |
| --------- | ------: |
| 10 cm     |     102 |
| 37 cm     |      12 |
| **Total** | **114** |

Alle 114 Wandobjekte erfüllen unsere Prüfregel `Width > 0 m`.

### Gespeicherte Ergebnisse

Die CSV-Dateien liegen im Ordner `results/`:

* `Team_5_A2_Wanddicke_negativ.csv`
* `Team_5_A2_Wanddicke_positiv.csv`
* `Team_5_A2_IFC2X3_original.csv`

Die Dateien enthalten unter anderem die GlobalId, die geprüfte Wanddicke, die Prüfregel und das Ergebnis.

## 7. Grenzen und nächste Schritte

Mit unserem MVP haben wir die Wanddickenprüfung erfolgreich an IFC4- und IFC2X3-Modellen getestet.

Es gibt aber noch einige Einschränkungen:

* Bisher ist nur die Anforderung A2 automatisiert umgesetzt.
* Die Wandbezeichnung (A1) und Materialzuordnung (A3) sind noch nicht automatisiert.
* Das BIMcollab-Beispielmodell ist kein nachgewiesenes Holzrahmenbaumodell.
* Eine positive Wanddicke bedeutet nicht automatisch, dass eine Wand konstruktiv richtig aufgebaut ist.

Als Nächstes möchten wir weitere Anforderungen prüfen und die Lösung für zusätzliche IFC-Modelle erweitern.

Die Spezifikation und AIA-BEP-Matrix befinden sich in `requirements/`. Der Ist- und Sollprozess liegt in `workflow/`.

Unsere KI-Nutzung haben wir in `KI_USAGE.md` dokumentiert.

---

**Team 5 – Data Management & BIM**
**GitHub:** https://github.com/beckr5/bxmid03-hs26-team-5
