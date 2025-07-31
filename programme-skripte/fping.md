# fping

`fping` ist ein Kommandozeilenwerkzeug zum schnellen Anpingen mehrerer Hosts. Im Gegensatz zu `ping` kann `fping` mehrere Ziele parallel prüfen und ist besonders für Skripte und Netzwerkscans geeignet.

**Befehl:**

```
fping [Optionen] Ziel(e)
```

**Parameter:**

* `-a` – Zeigt nur erreichbare Hosts
* `-u` – Zeigt nur nicht erreichbare Hosts
* `-g` – Generiert eine IP-Range (z. B. `fping -g 192.168.1.0/24`)
* `-r <n>` – Anzahl der Wiederholungsversuche (Standard: 3)
* `-t <ms>` – Timeout in Millisekunden pro Antwort
* `-i <ms>` – Wartezeit zwischen einzelnen Pings (Standard: 25 ms)
* `-p <ms>` – Zeitabstand zwischen Pings an denselben Host
* `-b <n>` – Paketgröße in Bytes
* `-c <n>` – Anzahl der Pings pro Host
* `-e` – Zeigt Antwortzeit
* `-q` – Quiet-Modus (nur Zusammenfassung)
* `-s` – Ausgabe einer Statistik am Ende
* `-f` – Liest Ziele aus Datei (eine IP/Hostname pro Zeile)
* `-h` – Hilfe anzeigen

**Beispiel:**

```
fping -a -g 192.168.0.0/24
```
