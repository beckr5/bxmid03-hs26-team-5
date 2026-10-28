# KI-Nutzung – Team 5

**Modul:** Data Management & BIM – BFH
**Projekt:** Automatisierte Qualitätsprüfung von Holzrahmenbauwänden
**Stand:** 28.10.2026

## Einsatz von ChatGPT

Während unserer Projektarbeit haben wir ChatGPT (OpenAI) als Unterstützung verwendet.

| Bereich               | Wofür haben wir ChatGPT verwendet?                                                        | Wie haben wir es überprüft?                                       |
| --------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Projektdokumentation  | Hilfe beim Formulieren, Kürzen und Strukturieren unserer Markdown-Dateien                 | Vergleich mit unseren Projektunterlagen und Anforderungen         |
| AIA-BEP-Matrix        | Unterstützung bei der Übertragung und Überarbeitung der Matrix                            | Abgleich mit unserer ursprünglichen Projektmatrix                 |
| GitHub                | Unterstützung beim Aufbau und bei der Organisation der Dateien                            | Kontrolle der Ordner und Dateien auf GitHub                       |
| Python / Google Colab | Unterstützung bei der Erstellung der beiden Notebooks und der Prüfregel für die Wanddicke | Ausführung des Codes in Google Colab und Kontrolle der Ergebnisse |

Für die Programmierung haben wir die Unterrichtsübungen als Grundlage verwendet. Die Prüfregel und die Notebooks wurden für unser Projekt angepasst und erweitert.

## Kontrolle der Ergebnisse

Den ersten automatisierten Wanddickencheck haben wir am **27.10.2026** in Google Colab ausgeführt.

Dabei haben wir zwei IFC4-Lehrmodelle mit derselben Prüfregel getestet:

* **Negativer Test:** Wanddicke 0 m → durchgefallen
* **Positiver Test:** Wanddicke 0.1143 m → bestanden

Am **28.10.2026** haben wir den Check zusätzlich auf das ursprüngliche IFC2X3-BIMcollab-Modell übertragen.

Dabei wurden insgesamt **114 Wände geprüft**. Alle 114 Wände haben die Anforderung `Width > 0 m` erfüllt.

Die Ergebnisse haben wir kontrolliert und als CSV-Dateien im Ordner `results/` gespeichert.

## Verantwortung

ChatGPT hat uns bei der Dokumentation und technischen Umsetzung unterstützt.

Die Projektwahl und die drei Informationsanforderungen stammen aus unserer eigenen Projektarbeit.

Wir sind als Team dafür verantwortlich, die Inhalte zu überprüfen, den Code zu testen und die Ergebnisse richtig zu dokumentieren.

---

**Team 5 – Data Management & BIM**
