# Spezifikation – Team 5

**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
**Modul:** Data Management & BIM – BFH
**Team:** 5
**Version:** MVP 0.1
**Stand:** 28.10.2026

## 1. Ziel

Wir möchten ausgewählte Informationen von Holzrahmenbauwänden in IFC-Modellen automatisch prüfen.

Damit sollen fehlende oder ungültige Angaben frühzeitig erkannt werden. Die Prüfung ist vor der Werkplanung vorgesehen.

Für das erste MVP haben wir die **Wanddicke (A2)** als automatisierten Check umgesetzt. Die Wandbezeichnung (A1) und die Materialzuordnung (A3) sind für die weitere Entwicklung vorgesehen.

## 2. Informationsanforderungen

Wir haben drei Anforderungen definiert:

| ID | Anforderung       | IFC-Information                  | Prüfregel                     | Status    |
| -- | ----------------- | -------------------------------- | ----------------------------- | --------- |
| A1 | Wandbezeichnung   | `IfcWall.Name`                   | Text vorhanden und nicht leer | Geplant   |
| A2 | Wanddicke         | `Qto_WallBaseQuantities.Width`   | Wert > 0 m                    | Umgesetzt |
| A3 | Materialzuordnung | z. B. `IfcRelAssociatesMaterial` | Zuordnung vorhanden           | Geplant   |

Bei A1 soll später geprüft werden, ob der Name fehlt oder nur aus Leerzeichen besteht.

Bei A3 muss noch untersucht werden, ob die Materialinformationen direkt an der Wand, am Wandtyp oder an den Materialschichten hinterlegt sind.

Die Anforderungen sind zusätzlich in unserer AIA-BEP-Matrix dokumentiert.

## 3. Technische Umsetzung von A2

### 3.1 Auswahl der Wandobjekte

Unser Check sucht im IFC-Modell nach Objekten der Klasse:

`IfcWall` (inklusive Unterklassen)

Damit nur die vorgesehenen Wände aus den Lehrmodellen geprüft werden, verwenden wir zusätzlich den Filter:

`CH_Lehre.Scope = Holzrahmen-MVP`

Dieser Filter wurde für die verwendeten Lehrmodelle festgelegt.

### 3.2 Prüfregel

Die Wanddicke wird aus folgender IFC-Information ausgelesen:

`Qto_WallBaseQuantities.Width`

Anschliessend wird der Wert mit unserer Prüfregel verglichen:

**Width > 0 m**

Die Längeneinheit wird aus dem IFC-Modell gelesen und für die Prüfung in Meter umgerechnet.

| Bedingung             | Ergebnis      |
| --------------------- | ------------- |
| Wanddicke > 0 m       | Bestanden     |
| Wanddicke ≤ 0 m       | Durchgefallen |
| Wanddicke fehlt       | Fehlend       |
| Ungültiger Zahlenwert | Durchgefallen |

Wenn keine Wand zum definierten Filter passt, wird die Prüfung mit einer Fehlermeldung abgebrochen.

### 3.3 Verwendete Software

Wir haben den Check mit folgenden Werkzeugen umgesetzt:

| Werkzeug                 | Verwendung                           |
| ------------------------ | ------------------------------------ |
| Google Colab             | Notebook ausführen                   |
| Python                   | Prüflogik                            |
| IfcOpenShell 0.9.0.post1 | IFC-Daten auslesen                   |
| pandas                   | Ergebnisse auswerten und exportieren |
| GitHub                   | Dateien und Ergebnisse dokumentieren |

Die Prüfung wurde als Python-Code umgesetzt. Eine separate IDS-Datei wurde bisher nicht erstellt.

**Notebook:**

`checks/Team_5_Wanddickencheck.ipynb`

Die Anleitung zur Ausführung befindet sich in der zentralen `README.md`.

## 4. Verwendete IFC-Modelle

Unser ursprüngliches Arbeitsmodell ist:

`Team_5_BIMcollab_Example_STR.ifc` (IFC2X3)

Dieses Modell befindet sich im Ordner `models/`.

Für die technische Prüfung haben wir zwei IFC4-Lehrmodelle aus den Unterrichtsunterlagen verwendet:

| Modell                           | Verwendung     |
| -------------------------------- | -------------- |
| `Demo_Holzrahmen_fehlerhaft.ifc` | Negativer Test |
| `Demo_Holzrahmen_korrigiert.ifc` | Positiver Test |

Die beiden Lehrmodelle werden vom Notebook automatisch heruntergeladen.

**Wichtig:** Unser ursprüngliches IFC2X3-Modell wurde bisher noch nicht mit diesem Check geprüft.

Bei IFC2X3 kann die Wanddicke beispielsweise unter `BaseQuantities.Width` gespeichert sein. Dies muss vor einer Anwendung auf unser Arbeitsmodell untersucht werden.

Weitere Angaben zu den Modellquellen stehen in `models/README.md`.

## 5. Validierung und Ergebnisse

Wir haben unseren Check am **27.10.2026** in Google Colab getestet.

Für beide Modelle wurde dieselbe Wand mit derselben Prüfregel untersucht.

**GlobalId der geprüften Wand:**

`2ucZRLBGP4uxZW$9i1VAZ8`

### Testergebnisse

| Kriterium      | Negativer Test | Positiver Test |
| -------------- | -------------- | -------------- |
| Geprüfte Wände | 1              | 1              |
| Wanddicke      | 0 m            | 0.1143 m       |
| Prüfregel      | Width > 0 m    | Width > 0 m    |
| Ergebnis       | Durchgefallen  | Bestanden      |

Der negative Test wurde korrekt als fehlerhaft erkannt. Der positive Test hat die Prüfung bestanden.

Damit konnten wir zeigen, dass unser Check bei den beiden Lehrmodellen zwischen einer ungültigen und einer positiven Wanddicke unterscheidet.

### Ausgabe

Die Ergebnisse werden als CSV-Dateien gespeichert:

* `results/Team_5_A2_Wanddicke_negativ.csv`
* `results/Team_5_A2_Wanddicke_positiv.csv`

Die Dateien enthalten unter anderem die GlobalId, den geprüften Wert, die Prüfregel, das Ergebnis und die Begründung.

Zusätzlich werden das verwendete Modell, die Softwareversion und der Prüfzeitpunkt dokumentiert.

## 6. Grenzen und nächste Schritte

Unser MVP funktioniert mit den beiden verwendeten IFC4-Lehrmodellen.

Dabei gibt es noch folgende Einschränkungen:

* Bisher wird nur die Wanddicke (A2) automatisiert geprüft.
* Die Anforderungen A1 und A3 sind noch nicht umgesetzt.
* Die Prüfung wurde noch nicht auf unser ursprüngliches IFC2X3-Modell übertragen.
* Der Check prüft nur, ob die Wanddicke positiv ist. Ob die Dicke konstruktiv sinnvoll ist, wird nicht bewertet.

In einem nächsten Schritt möchten wir die Wandbezeichnung und Materialzuordnung ebenfalls automatisch prüfen.

Ausserdem soll der Check auf weitere IFC-Modelle angewendet werden können.

---

**Team 5 – Data Management & BIM**
**GitHub:** https://github.com/beckr5/bxmid03-hs26-team-5
