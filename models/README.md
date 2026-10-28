# IFC-Modelle – Team 5

In unserem Projekt verwenden wir drei IFC-Modelle, um die automatische Wanddickenprüfung zu testen.

## 1. Verwendete Modelle

| Modell                             | IFC-Version | Verwendung          |
| ---------------------------------- | ----------- | ------------------- |
| `Team_5_BIMcollab_Example_STR.ifc` | IFC2X3      | Test mit 114 Wänden |
| `Demo_Holzrahmen_fehlerhaft.ifc`   | IFC4        | Negativer Test      |
| `Demo_Holzrahmen_korrigiert.ifc`   | IFC4        | Positiver Test      |

## 2. Unser BIMcollab-Arbeitsmodell

Das Modell `Team_5_BIMcollab_Example_STR.ifc` befindet sich direkt im Ordner `models/`.

Wir haben darin **114 Wandobjekte** mit unserem IFC2X3-Notebook geprüft.

Die Wanddicken werden über `BaseQuantities.Width` ausgelesen.

| Wanddicke |  Anzahl |
| --------- | ------: |
| 10 cm     |     102 |
| 37 cm     |      12 |
| **Total** | **114** |

Alle Wände erfüllen unsere Prüfregel `Width > 0 m`.

Das Modell ist ein allgemeines Tragwerksbeispiel und kein nachgewiesenes Holzrahmenbaumodell.

**Offen:** Die ursprüngliche Modellquelle und die Veröffentlichungsrechte müssen wir noch bestätigen.

## 3. IFC4-Lehrmodelle

Die beiden IFC4-Modelle stammen aus den Unterrichtsunterlagen.

Wir verwenden sie, um einen positiven und einen negativen Testfall mit derselben Wand und derselben Prüfregel durchzuführen.

| Testfall   | Wanddicke | Ergebnis      |
| ---------- | --------- | ------------- |
| Fehlerhaft | 0 m       | Durchgefallen |
| Korrigiert | 0.1143 m  | Bestanden     |

**GlobalId der geprüften Wand:**

`2ucZRLBGP4uxZW$9i1VAZ8`

Die Modelle werden vom Notebook automatisch aus dem Kurs-Repository `LTplus-AG/bxmid03-hs26` heruntergeladen.

Verwendeter Quellenstand:

`b271ce09e369d73e248a3d11f7af1410d98f4eb7`

Die Lehrmodelle sind deshalb nicht nochmals in unserem Repository gespeichert.

## 4. Ausführung und Ergebnisse

Unsere beiden Notebooks befinden sich im Ordner `checks/`:

* `Team_5_Wanddickencheck.ipynb` – IFC4
* `Team_5_IFC2X3_Wanddickencheck.ipynb` – IFC2X3

Die zugehörigen CSV-Dateien sind im Ordner `results/` gespeichert.

Die Anleitung zur Ausführung steht in der zentralen `README.md`.

---

**Team 5 – Data Management & BIM**
