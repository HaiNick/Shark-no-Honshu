# dig

## Cheat Sheet – DNS-Analyse und Fehlerdiagnose

### Allgemeine Befehle

| Befehl                       | Beschreibung                                       |
| ---------------------------- | -------------------------------------------------- |
| `dig domain.com`             | Standardabfrage (A-Record)                         |
| `dig @dns-server domain.com` | Anfrage über einen bestimmten DNS-Server           |
| `dig domain.com ANY`         | Abfrage aller verfügbaren DNS-Ressourceneinträge   |
| `dig domain.com MX`          | Mailserver-Einträge (MX) abfragen                  |
| `dig -x <IP>`                | Reverse Lookup (PTR)                               |
| `dig +short domain.com`      | Kompakte Ausgabe (nur IP-Adresse)                  |
| `dig +trace domain.com`      | Auflösungspfad von Root-Server bis zur Ziel-Domain |
| `dig domain.com NS`          | Nameserver-Einträge anzeigen                       |
| `dig domain.com SOA`         | SOA (Start of Authority) anzeigen                  |
| `dig domain.com TXT`         | Text-Einträge wie SPF, DKIM oder DMARC anzeigen    |

***

### Struktur der Ausgabe

| Sektion             | Beschreibung                                                    |
| ------------------- | --------------------------------------------------------------- |
| `HEADER`            | Enthält Status, Transaktions-ID, Flags (z. B. `ra`, `qr`, `aa`) |
| `QUESTION SECTION`  | Die angeforderte Abfrage                                        |
| `ANSWER SECTION`    | Ergebnis der Anfrage                                            |
| `AUTHORITY SECTION` | Authoritative Nameserver (bei nicht vorhandener Antwort)        |
| `ADDITIONAL`        | Zusätzliche Informationen, z. B. IP-Adressen von NS             |
| `OPT PSEUDOSECTION` | EDNS-bezogene Angaben (z. B. UDP-Paketgröße, Protokollversion)  |

***

### Wichtige Statuscodes

| Code       | Bedeutung                                    |
| ---------- | -------------------------------------------- |
| `NOERROR`  | Anfrage erfolgreich bearbeitet               |
| `NXDOMAIN` | Domain existiert nicht                       |
| `SERVFAIL` | Serverfehler, z. B. Server nicht erreichbar  |
| `REFUSED`  | Anfrage vom Server abgelehnt                 |
| `FORMERR`  | Anfrage fehlerhaft formatiert                |
| `NOTIMP`   | Funktion nicht implementiert                 |
| `TIMEOUT`  | DNS-Server hat nicht rechtzeitig geantwortet |

***

### Häufige Flags im Header

| Flag | Beschreibung                                                            |
| ---- | ----------------------------------------------------------------------- |
| `qr` | Antwort (nicht Anfrage)                                                 |
| `rd` | Rekursion erwünscht (vom Client angefordert)                            |
| `ra` | Rekursion verfügbar (Server erlaubt rekursive Auflösung)                |
| `aa` | Authoritative Answer (Antwort von zuständigem Nameserver)               |
| `tc` | Truncated – Antwort wurde aufgrund von Größenbeschränkung abgeschnitten |

***

### Wichtige Zusatzoptionen

| Option          | Beschreibung                               |
| --------------- | ------------------------------------------ |
| `+nocmd`        | Kommandozeile in der Ausgabe unterdrücken  |
| `+noquestion`   | Blendet Question Section aus               |
| `+noauthority`  | Authority-Bereich unterdrücken             |
| `+noadditional` | Additional Section ausblenden              |
| `+dnssec`       | DNSSEC-Daten einblenden                    |
| `+short`        | Nur relevante Daten als kompakte Ausgabe   |
| `+multiline`    | Mehrzeilige Ausgabe für bessere Lesbarkeit |

***

### Beispielausgabe

```bash
dig @10.10.10.10 internal.local

;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12345
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; QUESTION SECTION:
;internal.local.         IN      A

;; ANSWER SECTION:
internal.local.      3600   IN   A   10.10.10.20
```

**Interpretation**:

* Anfrage erfolgreich verarbeitet (`status: NOERROR`)
* Eine gültige Antwort vorhanden
* Der verwendete DNS-Server erlaubt rekursive Anfragen (`ra`)
* Kein Fehler im Protokoll erkennbar

***

### Fehleranalyse mit `dig`

| Symptom                            | Mögliche Ursache                                       |
| ---------------------------------- | ------------------------------------------------------ |
| `NXDOMAIN`                         | Hostname falsch oder Domain existiert nicht            |
| `SERVFAIL`                         | DNS-Server intern fehlerhaft oder blockiert Anfrage    |
| `REFUSED`                          | DNS-Server verweigert die Anfrage (ACL, Konfiguration) |
| Keine Antwort / Zeitüberschreitung | Server nicht erreichbar, Netzwerkproblem               |
| Unterschiedliche Antworten         | DNS-Rotation, Caching, CDN, TTL-Differenzen            |

***

#### Erweiterte Anwendung

*   **Anfrage an spezifischen DNS-Server für interne Namensauflösung**:

    ```bash
    dig @192.168.100.10 host.internal.net
    ```
*   **Zonenübertragungsversuch (AXFR, falls erlaubt)**:

    ```bash
    dig @192.168.100.10 internal.net AXFR
    ```
