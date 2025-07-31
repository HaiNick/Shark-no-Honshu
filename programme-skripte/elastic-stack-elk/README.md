---
description: >-
  Elasticsearch ist eine verteilte, quelloffene Such- und Analyse-Engine für
  alle Arten von Daten, einschließlich textueller, numerischer, raumbezogener,
  strukturierter und unstrukturierter Daten.
cover: >-
  https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/f858c0d22d015b663438dae207981532.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Elastic stack (ELK)

* Elasticsearch ist eine Datenbank, die **dokumentenorientiert**e und **halbstrukturiert**e Daten speichert, abruft und verwaltet.
* Rohdaten fließen aus **verschiedene**n **Quellen** inkl. Logs, Sytem Metriken und Web-Applikationen, in Elasticsearch

**Data Ingestion:** Prozess welcher Rohdaten parst, normalisiert und anreichert bevor es in Elasticsearch indexiert wird.

Die mit **Logstash** indexierten und formatierten Daten können auf beliebige Weise mit **Elastic Search** durchsucht werden. Die Ergebnisse können nach eigenen Bedürfnissen aufgebaut werden. Die Ergebnisse können mit **Kibana** visualisiert, geteilt sowie gemanagt werden.

## Bestandteile

### **Elastic Search**

**Kurzinfo**: Modifizierbare, variable Suche, Rest-API.

Elasticsearch ist eine Volltextsuch- und Analysemaschine, die zum Speichern von JSON-formatierten Dokumenten verwendet wird. Elasticsearch ist eine wichtige Komponente zum Speichern, Analysieren, Korrelieren von Daten usw. Elasticsearch unterstützt die RESTFul-API für die Interaktion mit den Daten.

**Download :** [https://www.elastic.co/de/downloads/elasticsearch](https://www.elastic.co/de/downloads/elasticsearch)

#### Grundkonzepte

<figure><img src="../../.gitbook/assets/grafik (4) (1) (1) (1).png" alt="" width="563"><figcaption><p>Grundlegender Aufbau einer Anfrage</p></figcaption></figure>

<table><thead><tr><th width="165">Bezeichnung</th><th>Erklärung</th></tr></thead><tbody><tr><td><strong>Cluster</strong></td><td>Sammlung von einem oder mehreren Servern, die zusammen alle Daten enthalten und über alle Server hinweg föderierte Indizierungs- und Suchfunktionen bieten. Bei relationalen Datenbanken ist der Knoten eine DB-Instanz. Es kann N Knoten mit demselben Clusternamen geben</td></tr><tr><td><strong>Near-Real-Time</strong> (NRT)</td><td></td></tr><tr><td><strong>Index</strong></td><td>Sammlung von Dokumenten, die ähnliche Merkmale aufweisen</td></tr><tr><td><strong>Node</strong> (single Elasticsearch Instanz)</td><td>Einzelner Server innerhalb eines Clusters der Daten hält und bei <strong>indexing</strong> und <strong>querying</strong> teilnimmt. Ein Node kann so konfiguriert werden, dass er einem bestimmten Cluster durch dessen Clustername beitritt. Ein einzelnes Cluster kann aus einer unbegrenzten Anzahl an Nodes bestehen.</td></tr><tr><td><strong>Shards</strong></td><td>Subset von Dokumenten eines Index. Ein Index kann in viele Shards unterteilt werden.</td></tr></tbody></table>



### **Logstash**

**Kurzinfo**: Logging-Tool zur Extraktion und Filterung von Event- und Logdaten jedweder Programme

Logstash ist eine Datenverarbeitungs-Engine, die dazu dient, Daten aus verschiedenen Quellen zu übernehmen, den Filter darauf anzuwenden oder sie zunormalisieren und sie dann an das Ziel zu senden, bei dem es sich um Kibana oder einen Überwachungsport handeln kann

<figure><img src="../../.gitbook/assets/grafik (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Pipeline Datenfluss</p></figcaption></figure>

**Download/Installation**: [https://www.elastic.co/guide/en/logstash/6.2/installing-logstash.html](https://www.elastic.co/guide/en/logstash/6.2/installing-logstash.html)

**Dokumentation**: [https://www.elastic.co/de/blog/a-practical-introduction-to-logstash](https://www.elastic.co/de/blog/a-practical-introduction-to-logstash)

#### Grundkonzepte

<table><thead><tr><th width="190">Bezeichnung</th><th>Erklärung</th></tr></thead><tbody><tr><td>Event Object</td><td>Hauptobjekt welches den Datenfluss innerhalb der pipeline kapselt.</td></tr><tr><td>Pipeline</td><td>Besteht aus den Stages des Datenfluss von Input zu Output.</td></tr><tr><td><strong>Input</strong></td><td>Erste Stage, erfassen der Daten für weitere Bearbeitung. Es gibt viele Plugins um Daten von unterschiedlichen Plattformen zu erfassen. File, Syslog, Redis und Beats gehören zu den am häufigsten verwendeten.<br><a href="https://www.elastic.co/guide/en/logstash/8.1/input-plugins.html">https://www.elastic.co/guide/en/logstash/8.1/input-plugins.html</a></td></tr><tr><td><strong>Filter</strong></td><td>Mittlere Stage zur Verarbeitung der Events. Vordefinierte Regex-Muster können verwendet werden um Events zu untersuchen und Daten nach Bedingungen zu Extrahieren.<br><a href="https://www.elastic.co/guide/en/logstash/8.1/filter-plugins.html">https://www.elastic.co/guide/en/logstash/8.1/filter-plugins.html</a></td></tr><tr><td><strong>Output</strong></td><td>Letzte Stage mit Formatierung in benötigte Struktur. Nach Verarbeitung werden die Daten unter Nutzung eines Plugins an das Ziel gesendet. Häufig verwendet werden Elasticsearch, File, Graphite, Statsd, etc.<br><a href="https://www.elastic.co/guide/en/logstash/8.1/output-plugins.html">https://www.elastic.co/guide/en/logstash/8.1/output-plugins.html</a></td></tr></tbody></table>



### Beats

**Kurzinfo**: Single-purpose Agent zur Übertragung von Daten ( Endpunkte -> Elasticsearch )

Beats ist ein hostbasierter Agent, bekannt als Data-Shipper, der zum Versenden/Übertragen von Daten von den Endpunkten an Elasticsearch verwendet wird. Jeder Beat ist ein Einzweckagent, der spezifische Daten an die Elasticsearch sendet.

**Dokumentation**: [https://www.elastic.co/guide/en/beats/libbeat/current/beats-reference.html](https://www.elastic.co/guide/en/beats/libbeat/current/beats-reference.html)

**Community-Beats**: [https://www.elastic.co/guide/en/beats/libbeat/current/community-beats.html](https://www.elastic.co/guide/en/beats/libbeat/current/community-beats.html)

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/0f2969b20c466e7a371a49bc809a6d5b.png" alt=""><figcaption><p>Beat Single-purpose agents</p></figcaption></figure>



### **Kibana**

**Download :** [https://www.elastic.co/de/downloads/kibana](https://www.elastic.co/de/downloads/kibana)

**Kurzinfo**: _Tool zur Visualisierung von Daten_

Kibana ist eine webbasierte Datenvisualisierung, die mit Elasticsearch zusammenarbeitet, um den Datenstrom in Echtzeit zu analysieren, zu untersuchen und zu visualisieren. Es ermöglicht den Benutzern, mehrere Visualisierungen und Dashboards für eine bessere Sichtbarkeit zu erstellen



## Zusammenarbeit aller Bestandteile

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5e8dd9a4a45e18443162feab/room-content/ec4f681a412aa825b284523dcd5b8650.png" alt=""><figcaption></figcaption></figure>

* **Beats** ist eine Reihe verschiedener Datenversandagenten, die zum Sammeln von Daten von mehreren Agenten verwendet werden. So wie **Winlogbeat** zum Sammeln von Windows-Ereignisprotokollen verwendet wird, erfasst **Packetbeat** Netzwerkverkehrsströme.
* **Logstash** sammelt Daten von **Beats**, Ports oder Dateien usw., analysiert/normalisiert sie in Feldwertpaare und speichert sie in **Elasticsearch**.
* **Elasticsearch** fungiert als Datenbank zur Suche und Analyse der Daten.
* **Kibana** ist für die Anzeige und Visualisierung der in **Elasticsearch** gespeicherten Daten verantwortlich. Die in **Elasticseach** gespeicherten Daten können mit **Kibana** problemlos in verschiedene Visualisierungen, Zeitdiagramme, Infografiken usw. umgewandelt werden.

<figure><img src="https://www.elastic.co/guide/en/fleet/8.13/images/fleet-server-on-prem-deployment.png" alt=""><figcaption><p>Deloyment an Agents über den Fleet Server</p></figcaption></figure>

## Search API/ Query DSL

Eine Suche besteht aus einer oder mehreren Abfragen, die kombiniert und an Elasticsearch gesendet werden. Dokumente, die mit den Abfragen einer Suche übereinstimmen, werden in den Treffern oder Suchergebnissen der Antwort zurückgegeben.

Eine Suche kann auch zusätzliche Informationen enthalten, die zur besseren Verarbeitung ihrer Abfragen verwendet werden. Beispielsweise kann eine Suche auf einen bestimmten Index beschränkt sein oder nur eine bestimmte Anzahl von Ergebnissen zurückgeben.

Sie können die Such-API verwenden, um Daten zu suchen und zu aggregieren, die in Elasticsearch-Datenströmen oder -Indizes gespeichert sind. Der Abfrageanforderungstextparameter der API akzeptiert Abfragen, die in Query DSL geschrieben wurden.

```bash
# Suche in my-index-000001 nach einem Dokument mit dem Wert
# user.id = kimchy
GET /my-index-000001/_search
{
  "query": {
    "match": {
      "user.id": "kimchy"
    }
  }
}
# Der Return besteht aus den Top10-Dokumenten, die auf die Query passen
```

CHEATSHEET: [elk-search-api.md](elk-search-api.md "mention")

### Anzeige der möglichen Spalten

```bash
# Anfrage nach möglicher Spalten für nodes, formatiert mit "Compact and aligned text"
# Verwendung von _cat nur für human-readable. Für Verarbeitung durch Anwendungen soll
# JSON API verwendet werden
GET /_cat/nodes?help

# Liefert id |     alias | beschreibung
          id | id,nodeId | unique node id
         pid |         p | process id
          ip |         i | ip address
          
GET /_cat/nodes?h=pid
# Liefert Prozess-id 1345

GET /_cat/indices?v
# Liefert alle Indizes mit ausführlicher (verbose) Ausgabe

GET /_cat/indices?v&s=index
# Liefert eine ausführliche Ausgabe sortiert nach index-name

GET /_cat/indices?v&s=store.size:desc
# Liefert eine ausführliche Ausgabe nach index-größe absteigend sortiert

GET /_cat/indices?health=green
# Zeigt nur Indizes mit bestimmtem Status an
```



#### Mögliche Parameter ( GET /\_cat/\<parameter> )

<table data-full-width="true"><thead><tr><th width="236">Parameter</th><th>Bedeutung</th></tr></thead><tbody><tr><td>aliases</td><td>Zeigt eine Liste der Index-Aliases und die dazugehörigen Indizes.</td></tr><tr><td>allocation</td><td>Gibt Informationen zur Zuweisung von Shards an die Knoten.</td></tr><tr><td>component_templates</td><td>Listet die vorhandenen Komponentenvorlagen auf und zeigt deren Details.</td></tr><tr><td>count</td><td>Zeigt die Anzahl der Dokumente in den Indizes.</td></tr><tr><td>fielddata</td><td>Gibt Informationen zur Speichernutzung von Felddaten.</td></tr><tr><td>health</td><td>Zeigt den Gesundheitszustand des Clusters (grün, gelb, rot).</td></tr><tr><td>indices</td><td>Listet die Indizes auf und zeigt deren Status, Anzahl der Dokumente, Speicherverbrauch, etc.</td></tr><tr><td>master</td><td>Gibt Informationen zum aktuellen Master-Knoten im Cluster.</td></tr><tr><td>ml/anomaly_detectors</td><td>Zeigt die Machine Learning Anomaly Detection Jobs und deren Status an.</td></tr><tr><td>ml/data_frame/analytics</td><td>Listet die Data Frame Analytics Jobs und deren Status auf.</td></tr><tr><td>ml/datafeeds</td><td>Zeigt die Datafeeds für die Machine Learning Jobs und deren Status an.</td></tr><tr><td>ml/trained_models</td><td>Listet die trainierten Machine Learning Modelle und deren Details auf.</td></tr><tr><td>nodeattrs</td><td>Gibt Informationen zu den Attributen der Knoten.</td></tr><tr><td>nodes</td><td>Zeigt eine Liste der Knoten im Cluster und deren grundlegende Informationen wie Name, IP-Adresse, Rollen, etc.</td></tr><tr><td>pending_tasks</td><td>Zeigt die Aufgaben an, die im Cluster noch anstehen.</td></tr><tr><td>plugins</td><td>Listet die installierten Plugins und deren Versionen auf.</td></tr><tr><td>recovery</td><td>Gibt Informationen zum Wiederherstellungsstatus von Shards.</td></tr><tr><td>repositories</td><td>Zeigt die registrierten Snapshot-Repositories und deren Details an.</td></tr><tr><td>segments</td><td>Listet die Segmente der Indizes auf und zeigt deren Speicherverbrauch und andere Details.</td></tr><tr><td>shards</td><td>Zeigt eine detaillierte Ansicht der Shards im Cluster, einschließlich der Verteilung und des Status.</td></tr><tr><td>snapshots</td><td>Gibt Informationen zu den Snapshots in den registrierten Repositories.</td></tr><tr><td>tasks</td><td>Listet die aktuell laufenden Aufgaben im Cluster auf.</td></tr><tr><td>templates</td><td>Zeigt die Indexvorlagen und deren Details an.</td></tr><tr><td>thread_pool</td><td>Gibt Informationen zu den Thread Pools der Knoten, einschließlich der Anzahl der aktiven Threads, Warteschlangenlängen und Ablehnungen.</td></tr><tr><td>transforms</td><td>Listet die laufenden und abgeschlossenen Transformationsjobs auf.</td></tr></tbody></table>

#### APIs

{% code fullWidth="true" %}
```
+---------------------------------------+----------------------------------------------------------------------+
| Befehl                                | Beschreibung                                                         |
+---------------------------------------+----------------------------------------------------------------------+
| GET /_cat/indices?v                   | Listet alle Indizes auf                                              |
| GET /_cat/indices?v&s=index           | Sortiert die Indizes nach dem Namen                                  |
| GET /_cat/indices?v&s=store.size:desc | Sortiert die Indizes nach der Größe                                  |
| GET /_cluster/health                  | Gibt den Gesundheitszustand des Clusters zurück                      |
| GET /_cluster/state                   | Gibt den aktuellen Zustand des Clusters zurück                       |
| GET /_cluster/stats                   | Gibt statistische Informationen über den Cluster zurück              |
| GET /index_name/_settings             | Gibt die Einstellungen eines spezifischen Index zurück               |
| GET /index_name/_mapping              | Gibt das Mapping eines spezifischen Index zurück                     |
| GET /index_name/_stats                | Gibt statistische Informationen über einen spezifischen Index zurück |
| GET /_recovery                        | Gibt den Wiederherstellungsstatus aller Indizes zurück               |
| GET /_nodes                           | Gibt detaillierte Informationen über alle Knoten im Cluster zurück   |
| GET /_nodes/stats                     | Gibt statistische Informationen über die Knoten im Cluster zurück    |
| GET /_nodes/hot_threads               | Gibt Informationen über die heißesten Threads im Cluster zurück      |
| GET /_snapshot/_all                   | Listet alle Snapshot-Repositories auf                                |
| GET /_snapshot/repository_name/_all   | Listet alle Snapshots in einem bestimmten Repository auf             |
| GET /_tasks                           | Listet alle laufenden Tasks im Cluster auf                           |
| GET /_tasks/task_id                   | Gibt den Status einer spezifischen Task zurück                       |
| GET /_alias                           | Listet alle Aliases im Cluster auf                                   |
| GET /_template                        | Listet alle Index-Templates auf                                      |
| PUT /index_name                       | Erstellt einen neuen Index                                           |
| POST /index_name/_doc/1               | Fügt ein Dokument zu einem Index hinzu                               |
| POST /index_name/_update/1            | Aktualisiert ein Dokument in einem Index                             |
| DELETE /index_name/_doc/1             | Löscht ein Dokument aus einem Index                                  |
| DELETE /index_name                    | Löscht einen Index                                                   |
+---------------------------------------+----------------------------------------------------------------------+

```
{% endcode %}

