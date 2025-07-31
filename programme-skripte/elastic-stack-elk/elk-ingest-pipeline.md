# ELK Ingest Pipeline

### Objekt :: ctx

**ctx** (Kontext) repräsentiert das aktuelle Dokument, das in der Pipeline verarbeitet wird.

\-> Alle Felder des Dokuments sind Attribute des **ctx**-Objekts

Beispiel:

| **ctx**.bytes\_send     | Greift auf das Feld "bytes\_send" des Dokuments zu |
| ----------------------- | -------------------------------------------------- |
| **ctx**\['field\_name'] | Alternative Syntax bei Sonderzeichen im Feldnamen  |

### Objekt :: params

**params** wird verwendet um Parameter an das Skript zu übertragen.

\-> Diese können innerhalb des Skripts verwendet werden, um dynamische Werte zu verarbeiten

```json
"script": {
  "lang": "painless",
  "source": """
    if (ctx.bytes_sent < params.threshold) {
      ctx.remove('bytes_sent');
    }
  """,
  "params": {
    "threshold": 1000
  }
}
```

### Objekt :: doc

**doc** stellt das aktuelle indizierte Dokument dar.

\-> Wird im Kontext von Ingest Pipelines normalerweise nicht verwendet, da ctx bereits das Dokument repräsentiert

### Objekt :: \_\_ingest

**\_\_ingest** bietet Zugriff auf Informationen über den aktuellen Ingest-Vorgang (bspw. aktuelle Pipeline und das Dokument). Wird selten verwendet

```json
{
  "script": {
    "lang": "painless",
    "source": """
      def pipeline = __ingest.getPipeline();
      // Weitere Logik basierend auf der Pipeline-Information
    """
  }
}
```

### Objekt :: log

**log** ermöglicht Protokollierung von Informationen während der Skriptausführung

\-> Nützlich für Debugging

```
{
  "script": {
    "lang": "painless",
    "source": """
      log.info('Processing document with ID: ' + ctx._id);
    """
  }
}
```

## Syntax & nützliche Funktionen

### Felder hinzufügen/ ändern

```
{
  "script": {
    "lang": "painless",
    "source": """
      ctx.new_field = 'new_value';
      ctx.existing_field = 'updated_value';
    """
  }
}
```

### Felder entfernen

```
{
  "script": {
    "lang": "painless",
    "source": """
      ctx.remove('field_to_remove');
    """
  }
}
```

### Arithmetische Operationen und Bedingungen

```
{
  "script": {
    "lang": "painless",
    "source": """
      if (ctx.field > 10) {
        ctx.field = ctx.field * 2;
      }
    """
  }
}
```

## Prozessoren

```
1. Convert Processor:
   - Konvertiert den Typ eines Feldes.

2. Date Processor:
   - Konvertiert ein Datumsfeld.

3. Dot Expander Processor:
   - Konvertiert Felder mit Punkt-Notation zu nested Objekten.

4. GeoIP Processor:
   - Fügt geografische Informationen basierend auf IP-Adressen hinzu.

5. Grok Processor:
   - Extrahiert strukturierte Felder aus ungeordneten Textfeldern.

6. Remove Processor:
   - Entfernt ein Feld aus dem Dokument.

7. Rename Processor:
   - Benennt ein Feld um.

8. Script Processor:
   - Führt eine benutzerdefinierte Transformation durch.

9. Set Processor:
   - Setzt den Wert eines Feldes auf einen bestimmten Wert.

10. Split Processor:
    - Teilt ein Feld mit einem Trennzeichen auf.

11. Lowercase Processor:
    - Konvertiert den Inhalt eines Feldes in Kleinbuchstaben.

12. Uppercase Processor:
    - Konvertiert den Inhalt eines Feldes in Großbuchstaben.

13. Trim Processor:
    - Entfernt Leerzeichen am Anfang und Ende eines Feldes.

14. Drop Processor:
    - Verwirft das gesamte Dokument oder Felder.

15. Tag Processor:
    - Fügt Tags zu einem Dokument hinzu.

16. Union Processor:
    - Vereint Felder zu einem einzelnen Feld.

17. Pipeline Processor:
    - Verkettet Ingest Pipelines für komplexe Transformationen.

```

## Patterns

[https://grokdebugger.com/](https://grokdebugger.com/)

### Datentypen

* `NUMBER`: Extrahiert numerische Werte, z.B. `123`, `3.14`.
* `INT`: Extrahiert ganze Zahlen (Integer), z.B. `123`.
* `BASE10NUM`: Extrahiert Zahlen im Dezimalformat, ähnlich zu `NUMBER`.
* `FLOAT`: Extrahiert Fließkommazahlen, z.B. `3.14`.
* `WORD`: Extrahiert Wörter ohne Leerzeichen, z.B. `openvpn`.
* `GREEDYDATA`: Extrahiert eine beliebige Zeichenfolge bis zum Ende der Zeile, z.B. `mki/13.16.19.18:2911 TCPv4_SERVER WRITE [97] to [AF_INET]13.16.19.18:2911 (via [AF_INET]23.14.1.17:444): P_DATA_V1 kid=0 DATA len=96`.
* `DATA`: Extrahiert eine beliebige Zeichenfolge bis zum nächsten Muster oder Trennzeichen.
* `PATH`: Extrahiert Dateipfade und Verzeichnisse, z.B. `/var/log/messages`.
* `IP`: Extrahiert IP-Adressen, z.B. `192.168.1.1`.
* `URI`: Extrahiert Uniform Resource Identifiers (URIs), z.B. `https://example.com/path/to/resource`.
* `HOSTNAME`: Extrahiert Hostnamen, z.B. `example.com`.
* `USERNAME`: Extrahiert Benutzernamen, z.B. `john_doe`.
* `UUID`: Extrahiert UUIDs (Universally Unique Identifiers), z.B. `123e4567-e89b-12d3-a456-426614174000`.
* `EMAILADDRESS`: Extrahiert E-Mail-Adressen, z.B. `user@example.com`.
* `MAC`: Extrahiert MAC-Adressen, z.B. `00:1A:2B:3C:4D:5E`.
* `SYSLOGTIMESTAMP`: Extrahiert Syslog-Zeitstempel, z.B. `Jun 19 08:44:00`.
* `ISO8601_TIMESTAMP`: Extrahiert ISO8601-konforme Zeitstempel, z.B. `2024-06-19T08:44:00Z`.
* `DATE`: Extrahiert Datumsangaben im Format `YYYY-MM-DD`.
* `TIME`: Extrahiert Uhrzeiten im Format `HH:MM:SS`.
* `MONTH`: Extrahiert Monatsnamen, z.B. `January`, `Feb`.

### Beispiel

{% code overflow="wrap" %}
```json
"^%{TIMESTAMP_ISO8601:timestamp} %{IP:client_ip} - - \[%{HTTPDATE:httpdate}\] \"%{WORD:method} %{DATA:request} HTTP/%{NUMBER:http_version}\" %{NUMBER:statuscode:int} %{NUMBER:bytes:int}$"
```
{% endcode %}

* `^`: Beginn des Musters.
* `%{TIMESTAMP_ISO8601:timestamp}`: ISO8601-Datumsfeld, benannt als `timestamp`.
* `%{IP:client_ip}`: IP-Adresse, benannt als `client_ip`.
* `\[`: Öffnende Klammer (literal).
* `%{HTTPDATE:httpdate}`: HTTP-Datumsformat, benannt als `httpdate`.
* `\"`: Öffnendes Anführungszeichen (literal).
* `%{WORD:method}`: Einzelnes Wort, benannt als `method`.
* `%{DATA:request}`: Beliebige Daten, benannt als `request`.
* `HTTP/`: Literal (HTTP/).
* `%{NUMBER:http_version}`: Zahl, interpretiert als Ganzzahl (`int`).
* `\"`: Schließendes Anführungszeichen (literal).
* `%{NUMBER:statuscode:int}`: Zahl, interpretiert als Ganzzahl (`int`).
* `%{NUMBER:bytes:int}`: Zahl, interpretiert als Ganzzahl (`int`).
* `$`: Ende des Musters.
