# scan networks with nmap because knowing what's there matters

<details>

<summary>Links:</summary>

[https://nmap.org/nsedoc/scripts/](https://nmap.org/nsedoc/scripts/)

[https://nmap.org/book/man-bypass-firewalls-ids.html](https://nmap.org/book/man-bypass-firewalls-ids.html)

</details>

## scan types

<pre><code># ICMP echo packets to every IP in subnet
nmap -PE -sn &#x3C;ip>/24
nmap -PP  "" ""   # ICMP timestamp request ( ICMP type 13 ) 
nmap -PM  "" ""   # ICMP address mask query ( ICMP type 17 ) -> response ( ICMP type 18 )
nmap -PR "" ""    # ARP scan

nmap -PS&#x3C;port>-&#x3C;port> "" "" # TCP SYN ping
nmap -PA&#x3C;p>-&#x3C;p> "" "" # TCP ACK ping
nmap -PU "" "" # UDP ping
<strong>[!] UDP ping on closed UDP port returns ICMP port unreachable
</strong>

nmap -sT &#x3C;ip>    # TCP scan: completes 3-way handshake
nmap -sS &#x3C;ip>    # SYN scan: sends RST after handshake
nmap -sU &#x3C;ip>    # UDP scan

nmap -R &#x3C;scan type> &#x3C;ip>/&#x3C;subnet>
reverse DNS lookup for all possible hosts in subnet,
hoping to gain insights from hostnames

thorough scan
nmap -T4 -sT -sV -sC -A -p- $IP
</code></pre>

| parameter                                                                                                                                                                                                                         | meaning                                                  |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| `-n`                                                                                                                                                                                                                              | no DNS lookup                                            |
| `-R`                                                                                                                                                                                                                              | reverse DNS lookup for all hosts                        |
| `-sn`                                                                                                                                                                                                                             | host discovery only                                      |
| `-d (-dd)`                                                                                                                                                                                                                        | debugging                                                |
| `-v (-vv)`                                                                                                                                                                                                                        | verbose output                                           |
| `--reason`                                                                                                                                                                                                                        | extended info for scan results                           |
| `--top-ports 10`                                                                                                                                                                                                                  | 10 most common ports                                     |
| `-p-`                                                                                                                                                                                                                             | all ports                                                |
| `-T<0-5>`                                                                                                                                                                                                                         | scan speed                                               |
| `-T0`                                                                                                                                                                                                                             | slowest (paranoid)                                       |
| `-T5`                                                                                                                                                                                                                             | am schnellsten                                           |
| <p><code>paranoid (0)     #  Vermeidung</code></p><p><code>sneaky (1)       #    von Alarm</code></p><p><code>polite (2)</code></p><p><code>normal (3)</code></p><p><code>aggressive (4)</code></p><p><code>insane (5)</code></p> | Scanstufen                                               |
| `--min-rate <nummer>`                                                                                                                                                                                                             | Paketrate minimal                                        |
| `--max-rate <nummer>`                                                                                                                                                                                                             | Paketrate maximal                                        |
| `--min-parallelism <numprobes>`                                                                                                                                                                                                   | Parallele Anfragen minimal                               |
| `--max-parallelism <numprobes>`.                                                                                                                                                                                                  | Parallele Anfragen maximal                               |
| `-e <interface>`                                                                                                                                                                                                                  | Netzwerk Interface auswählen                             |
| `-Pn`                                                                                                                                                                                                                             | ping-Scan deaktivieren                                   |
| `--source-port <num>`                                                                                                                                                                                                             | definierter Quell-Port                                   |
| `--data-length <num>`                                                                                                                                                                                                             | random Daten hinzufügen um definierte Länge zu erreichen |

## Service und OS-Scans

| Parameter                         | Beschreibung                                                                                                        |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| `-sV`                             | Service + Version Informationen (kein -sS möglich, da -sV eine aktive TCP Verbindung mit 3-Wege Handshake benötigt) |
| `-sV --version-intensity <level>` | Level (0-9) schwach bis stärkstes                                                                                   |
| `-sV --version-light`             | hat eine Intensität von 2                                                                                           |
| `-sV --version-all`               | mit allen verfügbaren Intensitäten testen (9)                                                                       |
| `-O`                              | OS Erkennung                                                                                                        |
| `--traceroute`                    | traceroute zum Scan hinzufügen                                                                                      |
| `-A`                              | equivalent zu `-sV -O -sC --traceroute`                                                                             |

## Erweiterte Scan-Arten

| Parameter                        | Bedeutung                                                 |
| -------------------------------- | --------------------------------------------------------- |
| `-sN`                            | NULL Scan                                                 |
| `-sF`                            | FIN Scan                                                  |
| `-sX`                            | Xmas Scan                                                 |
| `-sM`                            | Maimon Scan                                               |
| `-sA`                            | ACK Scan                                                  |
| `-sW`                            | Windows Scan                                              |
| `--scanflags RSTSYNFIN`          | Custom Scan mit angegebenen Flags, hier ( RST, SYN, FIN ) |
| `--scanflags URGACKPSHRSTSYNFIN` | Alle möglichen Flags                                      |

### NULL Scan

Bei der Null-Abfrage wird kein Flag gesetzt; alle sechs Flag-Bits werden auf Null gesetzt.

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/04b178a9cf7048c21256988b8b2343e3.png" alt=""><figcaption></figcaption></figure>

Ein TCP-Paket, bei dem keine Flags gesetzt sind, löst keine Antwort aus, wenn es einen offenen Port erreicht. Ein geschlossener Port liefert ( RST, ACK ) zurück

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/224e01a913a1ce7b0fb2b9290ff5e1c8.png" alt=""><figcaption></figcaption></figure>

### FIN Scan

Der FIN-Scan sendet ein TCP-Paket mit gesetztem FIN-Flag

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/78eb3d6ba158542f2b3223184b032e64.png" alt=""><figcaption></figcaption></figure>

Wenn der TCP-Port offen ist, wird keine Antwort gesendet.

Nmap kann nicht sicher sein, ob der Port offen ist oder ob eine Firewall den Verkehr in Bezug auf diesen TCP-Port blockiert.

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/74dc07da7351a5a7f258948ec59efccc.png" alt=""><figcaption></figcaption></figure>

### Xmas Scan

Der Xmas-Scan hat seinen Namen von der Weihnachtsbaumbeleuchtung. Bei einem Xmas-Scan werden die Flaggen FIN, PSH und URG gleichzeitig gesetzt.

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/7d28b756aed3b6eb72faf98d6974776c.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/4304eacbc3db1af21657f285bc16ebce.png" alt=""><figcaption></figcaption></figure>

### Maimon Scan

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/8ca5e5e0f6e0a1843cebe11b5b0785b3.png" alt=""><figcaption></figcaption></figure>

Die Bits FIN und ACK sind gesetzt.

Das Ziel sollte ein RST-Paket als Antwort senden.

Bestimmte von BSD abgeleitete Systeme verwerfen das Paket, wenn es sich um einen offenen Port handelt, der die offenen Ports preisgibt.

Dieser Scan wird bei den meisten Zielen, die in modernen Netzwerken anzutreffen sind, nicht funktionieren.

### TCP ACK Scan

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/a991831cedbb2761dde1fe66012a7311.png" alt=""><figcaption></figcaption></figure>

Sendet ACK-Flag. Da ACK nur auf Anfrage (REquest) gesendet wird, sollte hier ein RST vom Ziel kommen.

### Window Scan

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5118dcb424d429376f09bf2f85db5bce.png" alt=""><figcaption></figcaption></figure>

Paket Window-Größe wird untersucht

### Custom Scan

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/d76c5020f14ac0d66e7ff3812bb0bec3.png" alt=""><figcaption></figcaption></figure>

## Status der Ports

1. **Open**: indicates that a service is listening on the specified port.
2. **Closed**: indicates that no service is listening on the specified port, although the port is accessible. By accessible, we mean that it is reachable and is not blocked by a firewall or other security appliances/programs.
3. **Filtered**: means that Nmap cannot determine if the port is open or closed because the port is not accessible. This state is usually due to a firewall preventing Nmap from reaching that port. Nmap’s packets may be blocked from reaching the port; alternatively, the responses are blocked from reaching Nmap’s host.
4. **Unfiltered**: means that Nmap cannot determine if the port is open or closed, although the port is accessible. This state is encountered when using an ACK scan `-sA`.
5. **Open|Filtered**: This means that Nmap cannot determine whether the port is open or filtered.
6. **Closed|Filtered**: This means that Nmap cannot decide whether a port is closed or filtered.

## Weitere Anwendungen

<table><thead><tr><th width="327">Parameter</th><th>Beschreibung</th></tr></thead><tbody><tr><td><code>-S &#x3C;spoofed ip></code></td><td>IP Spoofing</td></tr><tr><td><code>--spoof-mac &#x3C;spoofed mac></code></td><td>MAC Spoofing ( nur wenn im gleichen Subnetz )</td></tr><tr><td><code>-D Decoy1,me,Decoy2 &#x3C;ip></code></td><td>Decoy Scan</td></tr><tr><td><code>-f (8 byte) -ff (16 byte)</code></td><td>Zerlegen der Pakete in kleinere Pakete (fragmente)</td></tr><tr><td><code>nmap -sI &#x3C;zombie ip> &#x3C;ip></code></td><td>Zombie/Idle Scan</td></tr></tbody></table>

### IP/ MAC Spoofing

Zum Herausfinden des Pfads zu einer bestimmten ZielIP?

Verstecktes Scannen eines gewählten Ziels.\
**Diese Methode kann nur angewandt werden, wenn es möglich ist den Netzverkehr mitzulesen!**

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/45b982d501fd26deb2b381059b16f80c.png" alt=""><figcaption></figcaption></figure>

1. Attacker sends a packet with a spoofed source IP address to the target machine.
2. Target machine replies to the spoofed IP address as the destination.
3. Attacker captures the replies to figure out open ports.

```
nmap -e NET_INTERFACE -Pn -S SPOOFED_IP <ip>
```

Funktioniert nur in minimaler Anzahl der Fälle, deshalb lieber Decoy Scan

### Decoy Scan

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/754fc455556a424ca83f512665beaf7d.png" alt=""><figcaption></figcaption></figure>

Die "Decoys" 1 und 2 sind hier spoofed-IPS

### Fragmentierte Pakete

Fragmentierung der Pakete in kleinere Fragmente zur Verringerung der Entdeckbarkeit durch IDS oder Firewall.

Dabei kann in 8Byte oder 16Byte Fragmente gesplittet werden.\
Ein Aufruf mit -ff und einem TCP-Segment der Größe 64 erzeugt somit 4 IP-Fragmente.

### Idle/ Zombie Scan

Dieser Scan benötigt ein Gerät im Netz, das sich im idle befindet, da sonst der zu untersuchende Wert IP-ID verfälscht würde.

```
nmap -sI <zombie ip> <ip>
```

Der Idle-Scan (Zombie-Scan) erfordert die folgenden drei Schritte, um festzustellen, ob ein Port offen ist:

* Bringen Sie den Idle-Host dazu, zu antworten, damit Sie die aktuelle IP-ID des Idle-Hosts aufzeichnen können.
* Senden Sie ein SYN-Paket an einen TCP-Port des Ziels. Das Paket sollte so gefälscht werden, dass es aussieht, als käme es von der IP-Adresse des Idle-Hosts (Zombie).
* Fordern Sie den inaktiven Rechner erneut auf, zu antworten, damit Sie die neue IP-ID mit der zuvor empfangenen vergleichen können.

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/a93e181f0effe000554a8b307448bbb2.png" alt=""><figcaption></figcaption></figure>



<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/8e28bf940936ddbc2367b193ea3550b8.png" alt=""><figcaption></figcaption></figure>



<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/2b0de492e2154a30760852e07cebae0e.png" alt=""><figcaption></figcaption></figure>

## Skripte ( NSE )

Durch die nmap Scripting Engine können weitere Skripte eingebunden werden.

Diese liegen unter `/usr/share/nmap/scripts`.

```
--script=default oder -sC

--script <script_name>
z.b. --script "http-date"
```

### Einsatzzweck

<table><thead><tr><th width="133">Kategorie</th><th>Erklärung</th></tr></thead><tbody><tr><td>auth</td><td>Authentifizierungsbezogene Skripte</td></tr><tr><td>broadcast</td><td>Entdeckt Hosts durch Senden von Broadcast-Nachrichten</td></tr><tr><td>brute</td><td>Führt eine Brute-Force-Passwortüberprüfung gegen Logins durch</td></tr><tr><td>default</td><td>Standard-Skripte, wie -sC</td></tr><tr><td>discovery</td><td>Abrufen von zugänglichen Informationen, wie z. B. Datenbanktabellen und DNS</td></tr><tr><td>dos</td><td>Ermittelt Server, die für Denial of Service (DoS) anfällig sind</td></tr><tr><td>exploit</td><td>Versucht, verschiedene anfällige Dienste auszunutzen</td></tr><tr><td>external</td><td>Überprüft mit Hilfe eines Drittanbieterdienstes, z. B. Geoplugin und Virustotal</td></tr><tr><td>fuzzer</td><td>Starten von Fuzzing-Angriffen</td></tr><tr><td>intrusive</td><td>Intrusive Skripte wie Brute-Force-Angriffe und Exploitation</td></tr><tr><td>malware</td><td>Scannt nach Hintertüren</td></tr><tr><td>safe</td><td>Sichere Skripte, die das Ziel nicht zum Absturz bringen</td></tr><tr><td>version</td><td>Abrufen von Dienstversionen</td></tr><tr><td>vuln</td><td>Prüft auf Schwachstellen oder nutzt verwundbare Dienste aus</td></tr></tbody></table>

## Output speichern

<table><thead><tr><th width="262">Parameter</th><th>Bedeutung</th></tr></thead><tbody><tr><td>-oN &#x3C;dateiname></td><td>Speichern der Ausgabe in normalem Format ( übersichtlich )</td></tr><tr><td>-oG &#x3C;dateiname></td><td>.. im <strong>grep</strong>-able Format ( einfach/ effizient filterbar )</td></tr><tr><td>-oX &#x3C;dateiname></td><td>XML-Format</td></tr><tr><td>-oS &#x3C;dateiname></td><td>Script Kiddie ( keine Funktion, sieht nur 31337 aus )</td></tr><tr><td>-oA &#x3C;dateiname></td><td>Speichern in allen verfügbaren Formaten</td></tr></tbody></table>
