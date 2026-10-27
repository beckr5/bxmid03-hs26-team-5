# Projektmatrix – Team 5

## Automatisierte Qualitätsprüfung von Holzrahmenbauwänden

### AIA-BEP-Anforderungsmatrix

| Anforderung             | 1. Wandbezeichnung                                                  | 2. Wanddicke                                                    | 3. Materialzuordnung                                                  |
| ----------------------- | ------------------------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------------------------- |
| **Zweck**               | Wände eindeutig ansprechen und fehlende Bezeichnungen erkennen      | Vorhandensein einer gültigen Wanddicke sicherstellen            | Sicherstellen, dass jeder relevanten Wand ein Material zugeordnet ist |
| **Meilensteine**        | IFC-Modellabgabe vor der Werkplanung                                | IFC-Modellabgabe vor der Werkplanung                            | IFC-Modellabgabe vor der Werkplanung                                  |
| **Objekte**             | Relevante IfcWall-Objekte                                           | Relevante IfcWall-Objekte                                       | Relevante IfcWall-Objekte                                             |
| **Property / Quantity** | `IfcWall.Name`                                                      | Z. B. `Qto_WallBaseQuantities.Width`                            | IFC-Materialzuordnung, z. B. über `IfcRelAssociatesMaterial`          |
| **Werte**               | Text vorhanden; nicht leer und nicht nur Leerzeichen                | Zahlenwert vorhanden und grösser als 0; Einheit berücksichtigen | Materialzuordnung vorhanden                                           |
| **Prüfung**             | Automatisch prüfen, ob die Bezeichnung vorhanden und nicht leer ist | Automatisch prüfen, ob die Wanddicke vorhanden und positiv ist  | Automatisch prüfen, ob eine Materialzuordnung vorhanden ist           |
| **Wer**                 | Fachplanung / Modellautor liefert; Prüfung kontrolliert             | Fachplanung / Modellautor liefert; Prüfung kontrolliert         | Fachplanung / Modellautor liefert; Prüfung kontrolliert               |
| **Quelle**              | Eigene Projektanforderung gemäss Projektwahl                        | Eigene Projektanforderung gemäss Projektwahl                    | Eigene Projektanforderung gemäss Projektwahl                          |
