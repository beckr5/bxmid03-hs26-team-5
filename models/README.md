# IFC-Modelle – Team 5

## 1. Übersicht

Im Projekt «Automatisierte Qualitätsprüfung von Holzrahmenbauwänden in IFC-Modellen» werden verschiedene IFC-Modelle für die Untersuchung und Validierung der Informationsanforderungen verwendet.

Dabei wird zwischen dem ursprünglichen Arbeitsmodell von Team 5 und den beiden Testmodellen für den automatisierten Wanddickencheck unterschieden.

| Modell                             | IFC-Version | Verwendung                   |
| ---------------------------------- | ----------- | ---------------------------- |
| `Team_5_BIMcollab_Example_STR.ifc` | IFC2X3      | Ursprüngliches Arbeitsmodell |
| `Demo_Holzrahmen_fehlerhaft.ifc`   | IFC4        | Negativer Testfall           |
| `Demo_Holzrahmen_korrigiert.ifc`   | IFC4        | Positiver Testfall           |

## 2. Ursprüngliches Arbeitsmodell

**Dateiname:** `Team_5_BIMcollab_Example_STR.ifc`

**IFC-Version:** IFC2X3

**Speicherort:** `models/Team_5_BIMcollab_Example_STR.ifc`

Das Modell wurde von Team 5 für die Untersuchung von Wandbauteilen und deren IFC-Informationen bereitgestellt.

Untersucht werden insbesondere:

* Wandbezeichnungen (`Name`)
* Wanddicken (`Width`)
* Materialzuordnungen

Das Modell ist im GitHub-Repository von Team 5 gespeichert.

**Hinweis:** Der aktuelle automatisierte Wanddickencheck wurde noch nicht auf diesem Modell ausgeführt. Die Anpassung und Validierung der Prüflogik für das IFC2X3-Modell ist ein weiterer Entwicklungsschritt.

**Originalquelle und Veröffentlichungsrechte:** Noch durch das Team zu dokumentieren beziehungsweise zu bestätigen.

## 3. IFC-Lehrmodelle für das MVP 0.1

Für den automatisierten Qualitätscheck der Anforderung A2 wurden zwei IFC4-Lehrmodelle verwendet.

### Negativer Testfall

**Dateiname:** `Demo_Holzrahmen_fehlerhaft.ifc`

**IFC-Version:** IFC4

**Geprüfte Wanddicke:** 0 m

**Prüfregel:** `Qto_WallBaseQuantities.Width > 0 m`

**Ergebnis:** DURCHGEFALLEN

### Positiver Testfall

**Dateiname:** `Demo_Holzrahmen_korrigiert.ifc`

**IFC-Version:** IFC4

**Geprüfte Wanddicke:** 0.1143 m

**Prüfregel:** `Qto_WallBaseQuantities.Width > 0 m`

**Ergebnis:** BESTANDEN

Beide Testfälle verwenden dieselbe Prüfregel und dieselbe Objektauswahl.

**GlobalId der geprüften Wand:**

`2ucZRLBGP4uxZW$9i1VAZ8`

## 4. Herkunft der Lehrmodelle

Die beiden IFC-Lehrmodelle werden über die im Unterricht verwendeten Google-Colab-Unterlagen bereitgestellt.

Das Notebook bezieht die Dateien aus folgendem GitHub-Repository:

`LTplus-AG/bxmid03-hs26`

Der verwendete Quellenstand ist durch folgenden Commit festgelegt:

`b271ce09e369d73e248a3d11f7af1410d98f4eb7`

**Direkte Download-Adressen:**

Negativer Testfall:

https://raw.githubusercontent.com/LTplus-AG/bxmid03-hs26/b271ce09e369d73e248a3d11f7af1410d98f4eb7/modelle/Demo_Holzrahmen_fehlerhaft.ifc

Positiver Testfall:

https://raw.githubusercontent.com/LTplus-AG/bxmid03-hs26/b271ce09e369d73e248a3d11f7af1410d98f4eb7/modelle/Demo_Holzrahmen_korrigiert.ifc

Die Lehrmodelle werden nicht zusätzlich im Team-Repository gespeichert.

## 5. Reproduzierbarkeit

Der automatisierte Check befindet sich unter:

`checks/Team_5_Wanddickencheck.ipynb`

Beim Ausführen in Google Colab werden die beiden Lehrmodelle automatisch heruntergeladen.

Ein manueller Upload ist nicht erforderlich.

Zur Identifikation der verwendeten Dateien werden SHA-256-Prüfsummen dokumentiert.

| Modell     | SHA-256                                                            |
| ---------- | ------------------------------------------------------------------ |
| Fehlerhaft | `688c3ff8258fb025accf7e8b286c49fe0e202974b7af29940adccf82c0886c0a` |
| Korrigiert | `2b4fcea90e9dc25dbbb4491496205ac1d5112585a8eacd04f8177642b51d06d6` |

Die tatsächlich erzeugten Prüfergebnisse befinden sich im Ordner `results/`.

## 6. Grenzen

Der aktuelle MVP-Check wurde ausschliesslich anhand der beiden IFC4-Lehrmodelle validiert.

Die Übertragbarkeit auf weitere IFC-Modelle muss separat überprüft werden.

Insbesondere müssen beim ursprünglichen IFC2X3-Arbeitsmodell die relevanten Wandobjekte, Quantity-Bezeichnungen und Datenpfade untersucht werden.

Ein erfolgreich bestandener Wanddickencheck bestätigt, dass die definierte Informationsanforderung erfüllt ist. Er bestätigt nicht automatisch die konstruktive Richtigkeit einer Wand.

---

**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
**Team:** 5
**Modul:** Data Management & BIM
**Stand:** 28.10.2026
