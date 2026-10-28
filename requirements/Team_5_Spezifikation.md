# Spezifikation – Team 5

**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
**Modul:** Data Management & BIM – BFH
**Team:** 5
**Version:** MVP 0.1
**Stand:** 28.10.2026

## 1. Ziel

Wir möchten ausgewählte Informationen von Wandbauteilen in IFC-Modellen automatisch prüfen.

Damit sollen fehlende oder ungültige Angaben frühzeitig erkannt werden. Die Prüfung ist vor der Werkplanung vorgesehen.

Für unser erstes MVP haben wir die **Wanddicke (A2)** als automatisierten Check umgesetzt. Die Wandbezeichnung (A1) und Materialzuordnung (A3) sind für die weitere Entwicklung vorgesehen.

Zusätzlich haben wir untersucht, ob unsere Prüfregel bei unterschiedlichen IFC-Versionen funktioniert.

## 2. Informationsanforderungen

Wir haben drei Anforderungen definiert:

| ID | Anforderung       | IFC-Information                  | Prüfregel                     | Status    |
| -- | ----------------- | -------------------------------- | ----------------------------- | --------- |
| A1 | Wandbezeichnung   | `IfcWall.Name`                   | Text vorhanden und nicht leer | Geplant   |
| A2 | Wanddicke         | `Width` aus den IFC-Mengendaten  | Wert > 0 m                    | Umgesetzt |
| A3 | Materialzuordnung | z. B. `IfcRelAssociatesMaterial` | Zuordnung vorhanden           | Geplant   |

Bei A1 soll später geprüft werden, ob der Name fehlt oder nur aus Leerzeichen besteht.

Bei A3 muss noch untersucht werden, ob die Materialinformationen direkt an der Wand, am Wandtyp oder an den Materialschichten hinterlegt sind.

Die Anforderungen sind zusätzlich in unserer AIA-BEP-Matrix dokumentiert.

## 3. Technische Umsetzung von A2

### 3.1 Auswahl der Wandobjekte

Unser Check sucht nach Objekten der IFC-Klasse:

`IfcWall` (inklusive Unterklassen)

Die Auswahl unterscheidet sich je nach Modell:

| Kriterium | IFC4-Lehrmodelle                  | IFC2X3-BIMcollab-Modell      |
| --------- | --------------------------------- | ---------------------------- |
| Objekte   | `IfcWall` inkl. Unterklassen      | `IfcWall` inkl. Unterklassen |
| Auswahl   | `CH_Lehre.Scope = Holzrahmen-MVP` | Alle Wandobjekte             |
| Datenpfad | `Qto_WallBaseQuantities.Width`    | `BaseQuantities.Width`       |

Der zusätzliche Filter aus den IFC4-Lehrmodellen ist im BIMcollab-Modell nicht vorhanden. Deshalb prüfen wir dort alle Wandobjekte.

### 3.2 Prüfregel

Für beide IFC-Versionen verwenden wir dieselbe Anforderung:

**Wanddicke > 0 m**

Die Wanddicke wird aus den IFC-Mengendaten ausgelesen und unter Berücksichtigung der Modelleinheit in Meter umgerechnet.

| Bedingung             | Ergebnis      |
| --------------------- | ------------- |
| Wanddicke > 0 m       | Bestanden     |
| Wanddicke ≤ 0 m       | Durchgefallen |
| Wanddicke fehlt       | Fehlend       |
| Ungültiger Zahlenwert | Durchgefallen |

Wenn keine passenden Wandobjekte gefunden werden, wird die Prüfung mit einer Fehlermeldung abgebrochen.

### 3.3 Verwendete Software

Wir haben den Check mit folgenden Werkzeugen umgesetzt:

| Werkzeug                 | Verwendung                           |
| ------------------------ | ------------------------------------ |
| Google Colab             | Notebooks ausführen                  |
| Python                   | Prüflogik                            |
| IfcOpenShell 0.9.0.post1 | IFC-Daten auslesen                   |
| pandas                   | Ergebnisse auswerten und exportieren |
| GitHub                   | Dateien und Ergebnisse dokumentieren |

Wir verwenden zwei Notebooks:

**IFC4-Lehrmodelle:**

`checks/Team_5_Wanddickencheck.ipynb`

**IFC2X3-BIMcollab-Modell:**

`checks/Team_5_IFC2X3_Wanddickencheck.ipynb`

Die Prüfung wurde als Python-Code umgesetzt. Eine separate IDS-Datei wurde bisher nicht erstellt.

Die Anleitung zur Ausführung befindet sich in der zentralen `README.md`.

## 4. Verwendete IFC-Modelle

Wir haben drei IFC-Dateien verwendet:

| Modell                             | IFC-Version | Verwendung                       |
| ---------------------------------- | ----------- | -------------------------------- |
| `Demo_Holzrahmen_fehlerhaft.ifc`   | IFC4        | Negativer Test                   |
| `Demo_Holzrahmen_korrigiert.ifc`   | IFC4        | Positiver Test                   |
| `Team_5_BIMcollab_Example_STR.ifc` | IFC2X3      | Zusätzlicher Test mit 114 Wänden |

Die beiden IFC4-Lehrmodelle stammen aus den Unterrichtsunterlagen und werden automatisch heruntergeladen.

Das BIMcollab-Modell befindet sich in unserem GitHub-Repository unter `models/`.

Mit dem IFC2X3-Modell konnten wir untersuchen, ob unsere Prüfregel auch bei einer anderen IFC-Version und einem anderen Datenpfad funktioniert.

Das BIMcollab-Beispielmodell ist allerdings kein nachgewiesenes Holzrahmenbaumodell.

Weitere Angaben zu den Modellquellen stehen in `models/README.md`.

## 5. Validierung und Ergebnisse

Wir haben die Wanddickenprüfung mit beiden IFC-Versionen in Google Colab getestet.

### 5.1 Test 1 – IFC4-Lehrmodelle

Den ersten Test haben wir am **27.10.2026** durchgeführt.

Bei beiden Modellen wurde dieselbe Wand mit derselben Prüfregel untersucht.

**GlobalId:**

`2ucZRLBGP4uxZW$9i1VAZ8`

| Kriterium      | Negativer Test | Positiver Test |
| -------------- | -------------- | -------------- |
| Geprüfte Wände | 1              | 1              |
| Wanddicke      | 0 m            | 0.1143 m       |
| Prüfregel      | Width > 0 m    | Width > 0 m    |
| Ergebnis       | Durchgefallen  | Bestanden      |

Der negative Test wurde korrekt als fehlerhaft erkannt. Der positive Test hat die Prüfung bestanden.

Damit konnten wir zeigen, dass unser Check zwischen einer ungültigen und einer positiven Wanddicke unterscheidet.

### 5.2 Test 2 – IFC2X3-BIMcollab-Modell

Am **28.10.2026** haben wir den Check zusätzlich auf unser ursprüngliches IFC2X3-Modell übertragen.

Dabei wurde die Wanddicke über `BaseQuantities.Width` ausgelesen.

**Ergebnisse:**

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

Alle 114 Wände erfüllen die Anforderung `Width > 0 m`.

Dieser Test zeigt, dass wir unsere Prüfregel auch auf ein IFC2X3-Modell übertragen konnten.

Da alle Wanddicken positiv sind, enthält dieses Modell keinen negativen Testfall. Die Überprüfung einer fehlerhaften Wanddicke haben wir bereits mit den IFC4-Lehrmodellen durchgeführt.

### 5.3 Gespeicherte Ergebnisse

Die Ergebnisse wurden als CSV-Dateien gespeichert:

* `results/Team_5_A2_Wanddicke_negativ.csv`
* `results/Team_5_A2_Wanddicke_positiv.csv`
* `results/Team_5_A2_IFC2X3_original.csv`

Die Dateien enthalten unter anderem die GlobalId, den geprüften Wert, die Prüfregel und das Ergebnis.

Zusätzlich werden Informationen zum verwendeten Modell und zur Softwareversion dokumentiert.

## 6. Grenzen und nächste Schritte

Mit unserem MVP haben wir die Wanddickenprüfung an IFC4- und IFC2X3-Modellen erfolgreich getestet.

Dabei gibt es noch folgende Einschränkungen:

* Bisher wird nur die Wanddicke (A2) automatisiert geprüft.
* Die Anforderungen A1 und A3 sind noch nicht umgesetzt.
* Das BIMcollab-Modell ist kein nachgewiesenes Holzrahmenbaumodell.
* Der Check prüft nur, ob die Wanddicke positiv ist. Ob die Dicke konstruktiv sinnvoll ist, wird nicht bewertet.

In einem nächsten Schritt möchten wir die Wandbezeichnung und Materialzuordnung ebenfalls automatisch prüfen.

Ausserdem möchten wir untersuchen, wie sich die Prüflogik auf weitere IFC-Modelle übertragen lässt.

---

**Team 5 – Data Management & BIM**
**GitHub:** https://github.com/beckr5/bxmid03-hs26-team-5
