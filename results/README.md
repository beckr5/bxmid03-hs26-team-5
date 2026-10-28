# Prüfergebnisse – Team 5

In diesem Ordner befinden sich die CSV-Ergebnisse unserer automatisierten Wanddickenprüfung (A2).

| Datei                             | Geprüfte Wände | Ergebnis       |
| --------------------------------- | -------------: | -------------- |
| `Team_5_A2_Wanddicke_negativ.csv` |              1 | Durchgefallen  |
| `Team_5_A2_Wanddicke_positiv.csv` |              1 | Bestanden      |
| `Team_5_A2_IFC2X3_original.csv`   |            114 | Alle bestanden |

## Unsere Tests

**IFC4-Lehrmodelle (27.10.2026)**

Wir haben dieselbe Wand mit einer fehlerhaften Wanddicke von 0 m und einer korrigierten Wanddicke von 0.1143 m geprüft. Beide Tests lieferten das erwartete Ergebnis.

**IFC2X3-BIMcollab-Modell (28.10.2026)**

Zusätzlich haben wir 114 Wände unseres ursprünglichen Arbeitsmodells geprüft.

* 102 Wände mit 10 cm Dicke
* 12 Wände mit 37 cm Dicke

Alle 114 Wände erfüllen unsere Prüfregel `Width > 0 m`.

Die CSV-Dateien enthalten die GlobalIds, geprüften Wanddicken und Prüfergebnisse.

Die zugehörigen Notebooks befinden sich im Ordner `checks/`.
