# Automatisierte Wanddickenprüfung – Team 5

In diesem Ordner befinden sich unsere beiden Google-Colab-Notebooks.

| Notebook                              | Verwendung                                         |
| ------------------------------------- | -------------------------------------------------- |
| `Team_5_Wanddickencheck.ipynb`        | Positiver und negativer Test mit IFC4-Lehrmodellen |
| `Team_5_IFC2X3_Wanddickencheck.ipynb` | Prüfung von 114 Wänden im BIMcollab-Modell         |

Beide Notebooks wurden mit Python und IfcOpenShell erstellt.

**Unsere Prüfregel A2:** Wanddicke > 0 m.

Bei den IFC4-Modellen lesen wir die Wanddicke über `Qto_WallBaseQuantities.Width` aus, beim IFC2X3-Modell über `BaseQuantities.Width`.

Beide Tests wurden erfolgreich in Google Colab durchgeführt.

Die Ausführungsanleitung befindet sich in der zentralen `README.md`.
