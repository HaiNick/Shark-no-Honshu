---
description: >-
  Curl ist ein Tool zur Übertragung von Daten von bzw. zu einem Webserver. Dabei
  werden vielfältige Protokolle unterstützt
  (FILE,FTP,FTPS,HTTP,HTTPS,SCP,SMB,etc.)
---

# curl

Cheatsheet siehe: [curl.md](../cheat-sheets/curl.md "mention")

### Hilfsmittel zum kodieren/entkodieren von URL-enc

[https://www.urlencoder.org/de/](https://www.urlencoder.org/de/)

## Einfaches Anfordern GET

```
curl <ip>:port/file?param=x
```

## Senden mit POST-Daten

```
curl <ip>:port/file -d "param=x&param2=y"
```

## Mit Header POST+GET

{% code overflow="wrap" %}
```
curl <ip>:port/file -H 'Content-Type: application/x-www-form-urlencoded' -d "param=x&param2=y"
```
{% endcode %}

<table data-full-width="false"><thead><tr><th width="59">-H</th><th>Zusätzlicher Header (Übertragene Daten sind vom Typ x-www-form-urlencoded, was dem Server mitteilt, dass form-Daten übertragen werden)</th></tr></thead><tbody><tr><td>-d</td><td>gibt zu übertragende Datenpaare an</td></tr></tbody></table>

## Mit Cookie Header

<pre><code><strong>curl -H "Cookie: logged_in=true; admin=false" http://MACHINE_IP/cookie-test
</strong></code></pre>

## Erweiterte Datenübertragung mit PUT/POST

1.  Senden von Formulardaten:

    ```
    curl -X PUT -H "Content-Type: multipart/form-data;" -F "key1=val1" "YOUR_URI"
    ```
2.  Senden von Rohdaten als json:

    ```
    curl -X PUT -H "Content-Type: application/json" -d '{"key1":"value"}' "YOUR_URI"
    ```
3.  Senden einer Datei als POST-Request:

    ```
    curl -X POST "YOUR_URI" -F 'file=@/file-path.csv'
    ```
