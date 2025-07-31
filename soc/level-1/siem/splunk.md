---
description: https://www.splunk.com/de_de
---

# Splunk

**Splunk** ist eine Plattform zur **Erfassung, Indexierung und Analyse von maschinell generierten Daten** (z. B. Logs, Events, Metriken). Sie wird hauptsächlich für **Security Information and Event Management (SIEM)**, **Monitoring** und **IT Operations** eingesetzt.

#### Kernfunktionen

* **Datenaufnahme:** Strukturierte und unstrukturierte Daten aus verschiedenen Quellen (Syslog, APIs, Dateien, etc.)
* **Suche & Analyse:** Eigene Suchsprache (**SPL – Search Processing Language**)
* **Dashboards & Visualisierung:** Interaktive Reports, Alarme und Echtzeit-Dashboards
* **Machine Learning & Korrelationsregeln:** Für Anomalieerkennung und automatisierte Analysen

#### Anwendungsbereiche

* Security Monitoring (SIEM)
* Log-Analyse
* Incident Response
* Infrastruktur- und Applikationsüberwachung
* Business Analytics (auf Basis technischer Daten)

#### Beispiel: SPL-Suche

```spl
index=web_logs status=500 | stats count by source
```

{% hint style="info" %}
Gibt die Anzahl von HTTP 500-Fehlern pro Quelle zurück.
{% endhint %}
