# AIA-BEP-Anforderungsmatrix – Team 5

**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
**Modul:** Data Management & BIM – BFH
**Team:** 5
**Version:** 0.2
**Stand:** 28.10.2026

## 1. Unsere Informationsanforderungen

Für unser Projekt haben wir drei Anforderungen festgelegt. Diese sollen bei der IFC-Modellabgabe vor der Werkplanung geprüft werden.

| Anforderung         | A1 – Wandbezeichnung                | A2 – Wanddicke                                   | A3 – Materialzuordnung             |
| ------------------- | ----------------------------------- | ------------------------------------------------ | ---------------------------------- |
| **Zweck**           | Fehlende Wandbezeichnungen erkennen | Gültige Wanddicke sicherstellen                  | Fehlende Materialangaben erkennen  |
| **Meilenstein**     | IFC-Abgabe vor der Werkplanung      | IFC-Abgabe vor der Werkplanung                   | IFC-Abgabe vor der Werkplanung     |
| **Objekte**         | Relevante `IfcWall`-Objekte         | Relevante `IfcWall`-Objekte                      | Relevante `IfcWall`-Objekte        |
| **IFC-Information** | `IfcWall.Name`                      | `Qto_WallBaseQuantities.Width` (IFC4)            | z. B. `IfcRelAssociatesMaterial`   |
| **Anforderung**     | Text vorhanden und nicht leer       | Wanddicke > 0 m                                  | Materialzuordnung vorhanden        |
| **Prüfung**         | Name auf vorhandenen Text prüfen    | Wanddicke auslesen und mit Grenzwert vergleichen | Materialzuordnung kontrollieren    |
| **Wer liefert?**    | Fachplanung / Modellautor           | Fachplanung / Modellautor                        | Fachplanung / Modellautor          |
| **Wer prüft?**      | BIM-Koordination / prüfende Stelle  | BIM-Koordination / prüfende Stelle               | BIM-Koordination / prüfende Stelle |
| **Status**          | Geplant                             | Umgesetzt                                        | Geplant                            |

Die Anforderungen stammen aus unserer Projektwahl. Die genaue Rollenverteilung muss im jeweiligen BIM-Projekt festgelegt werden.

## 2. Umsetzung im MVP 0.1

Für unser erstes MVP haben wir die **Anforderung A2 – Wanddicke** automatisiert umgesetzt.

Dafür verwenden wir Google Colab mit Python und IfcOpenShell.

Geprüft werden `IfcWall`-Objekte mit dem Filter:

`CH_Lehre.Scope = Holzrahmen-MVP`

Die Prüfregel lautet:

`Qto_WallBaseQuantities.Width > 0 m`

Die Längeneinheit wird aus dem IFC-Modell gelesen und in Meter umgerechnet.

Wir haben den Check mit zwei IFC4-Lehrmodellen getestet:

| Testfall       | Wanddicke | Ergebnis      |
| -------------- | --------- | ------------- |
| Negativer Test | 0 m       | Durchgefallen |
| Positiver Test | 0.1143 m  | Bestanden     |

**GlobalId der geprüften Wand:**

`2ucZRLBGP4uxZW$9i1VAZ8`

Bei beiden Tests wurden dieselbe Wand und dieselbe Prüfregel verwendet.

Die Prüfung hat bei beiden Modellen das erwartete Ergebnis geliefert.

Die CSV-Dateien befinden sich im Ordner `results/`.

## 3. Nächste Schritte

Die Anforderungen A1 und A3 sind bisher nur definiert und noch nicht automatisiert umgesetzt.

Ausserdem wurde unser ursprüngliches IFC2X3-Arbeitsmodell noch nicht mit dem aktuellen Check geprüft.

Diese Punkte möchten wir in der weiteren Projektbearbeitung angehen.

Die technischen Details stehen in `Team_5_Spezifikation.md`. Die Anleitung zur Ausführung befindet sich in der zentralen `README.md`.

---

**Team 5 – Data Management & BIM**
