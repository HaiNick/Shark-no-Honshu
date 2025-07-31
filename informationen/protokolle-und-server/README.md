# (Netzwerk-)Protokolle und Server

## IP-Addressen (IPv4)

**CIDR-Notation (Classless Inter-Domain Routing)**: Heutzutage werden IP-Adressen eher durch CIDR-Notation wie z.B. 192.168.1.0/24 angegeben, die die Netzmaske (Subnetzmaske) direkt spezifiziert, anstatt sich ausschließlich auf die traditionellen Klassen A, B, C usw. zu verlassen.

### Bereiche öffentlicher IP-Adressen

<table><thead><tr><th width="110">Klasse</th><th width="183">Bereich</th><th>Verwendung</th></tr></thead><tbody><tr><td>A</td><td>0.0.0.0 - 127.255.255.255</td><td>Große Netzwerke (früher Internet Service Provider, Universitäten, große Unternehmen)</td></tr><tr><td>B</td><td>128.0.0.0 - 191.255.255.255</td><td>Mittlere Netzwerke (früher mittlere Unternehmen, Universitäten)</td></tr><tr><td>C</td><td>192.0.0.0 - 223.255.255.255</td><td>Kleine Netzwerke (früher kleine Unternehmen, Privatnetzwerke)</td></tr><tr><td>D</td><td>224.0.0.0 - 239.255.255.255</td><td>Multicast-Adressen (nicht zur Verwendung im IP-Routing)</td></tr><tr><td>E</td><td>240.0.0.0 - 255.255.255.255</td><td>Reserviert für zukünftige Zwecke (Experimente, Forschung)</td></tr></tbody></table>

### Bereiche privater IP-Adressen

**Private IP-Bereiche**: Es gibt auch spezielle private IP-Adressbereiche, die für den Einsatz in privaten Netzwerken reserviert sind und nicht im öffentlichen Internet geroutet werden. Diese sind in RFC 1918 definiert und umfassen:

* **Klasse A**: 10.0.0.0 - 10.255.255.255
* **Klasse B**: 172.16.0.0 - 172.31.255.255
* **Klasse C**: 192.168.0.0 - 192.168.255.255

## Protokollliste

<table><thead><tr><th width="180">Protokoll</th><th width="149">TCP Port</th><th>Anwendung(en)</th><th>Datensicherheit</th></tr></thead><tbody><tr><td>FTP</td><td>21</td><td>File Transfer</td><td>Cleartext</td></tr><tr><td>FTPS</td><td>990</td><td>File Transfer</td><td>Encrypted</td></tr><tr><td>HTTP</td><td>80</td><td>Worldwide Web</td><td>Cleartext</td></tr><tr><td>HTTPS</td><td>443</td><td>Worldwide Web</td><td>Encrypted</td></tr><tr><td>IMAP</td><td>143</td><td>Email (MDA)</td><td>Cleartext</td></tr><tr><td>IMAPS</td><td>993</td><td>Email (MDA)</td><td>Encrypted</td></tr><tr><td>POP3</td><td>110</td><td>Email (MDA)</td><td>Cleartext</td></tr><tr><td>POP3S</td><td>995</td><td>Email (MDA)</td><td>Encrypted</td></tr><tr><td>SFTP</td><td>22</td><td>File Transfer</td><td>Encrypted</td></tr><tr><td>SSH</td><td>22</td><td>Remote Access and File Transfer</td><td>Encrypted</td></tr><tr><td>SMTP</td><td>25</td><td>Email (MTA)</td><td>Cleartext</td></tr><tr><td>SMTPS</td><td>465</td><td>Email (MTA)</td><td>Encrypted</td></tr><tr><td>Telnet</td><td>23</td><td>Remote Access</td><td>Cleartext</td></tr><tr><td>QUIC</td><td></td><td>Netzwerkprotokoll</td><td>Encrypted</td></tr></tbody></table>

PORT STATE SERVICE VERSION 21/tcp open ftp vsftpd 3.0.5 | ftp-syst: | STAT: | FTP server status: | Connected to ::ffff:10.9.8.148 | Logged in as ftp | TYPE: ASCII | No session bandwidth limit | Session timeout in seconds is 300 | Control connection is plain text | Data connections will be plain text | At session startup, client count was 4 | vsFTPd 3.0.5 - secure, fast, stable |\_End of status | ftp-anon: Anonymous FTP login allowed (FTP code 230) |\_Can't get directory listing: TIMEOUT Service Info: OS: Unix

## HTTP:80 ( HyperText Transfer Protocol )

```
GET /pfad HTTP/1.1   # Get an Pfad + Angabe HTTP-Version zur Kommunikation
GET / HTTP/1.1       # Get Index

+
host: telnet (etc.)
```

## FTP:21 ( File Transfer Protocol )

[https://book.hacktricks.xyz/network-services-pentesting/pentesting-ftp](https://book.hacktricks.xyz/network-services-pentesting/pentesting-ftp)

Das File Transfer Protocol (FTP) ist ein Protokoll, das die Fernübertragung von Dateien über ein Netzwerk ermöglicht. Es verwendet dazu ein Client-Server-Modell und leitet Befehle und Daten auf sehr effiziente Weise weiter. Der Client initiiert eine Verbindung mit dem Server, der Server überprüft die angegebenen Anmeldedaten und eröffnet dann die Sitzung.

Eine typische FTP-Sitzung arbeitet mit zwei Kanälen:

* einem **Befehlskanal** (manchmal auch Kontrollkanal genannt)
* einem **Datenkanal**.

Der FTP-Server kann **aktive** und **passive** Verbindungen unterstützen:

* aktiv: Client öffnet port und lauscht. Server muss sich aktiv verbinden
* passiv: Server öffnet port und lauscht (passiv). Client muss sich verbinden

```
ftp <ip> <port>

close   # verbindung schliessen
bye     # ftp verlassen

#Anonymous login mit
anonymous:anonymous
```

Einige verwundbare Versionen von **in.ftpd** und einige andere FTP-Server-Varianten geben unterschiedliche Antworten auf den "**cwd**"-Befehl für existierende und nicht existierende Home-Verzeichnisse zurück. Dies kann ausgenutzt werden, da man cwd-Befehle vor der Authentifizierung ausgeben kann, und wenn es ein Home-Verzeichnis gibt, gibt es höchstwahrscheinlich auch ein dazugehöriges Benutzerkonto. Obwohl dieser Fehler hauptsächlich in älteren Systemen auftritt, sollte man ihn kennen, da er eine Möglichkeit darstellt, FTP auszunutzen. [https://www.exploit-db.com/exploits/20745](https://www.exploit-db.com/exploits/20745)

<pre data-overflow="wrap"><code># Bruteforcing FTP-Passwort
<strong>hydra -t 4 -l &#x3C;user> -P /usr/share/wordlists/rockyou.txt -vV &#x3C;ip> ftp
</strong>      -t paralelle Verbindungen
           -l Username (Useraccount)
                     -P Passwort-Wordlist
                               -vV verbose mit anzeige login+pass pro Versuch
                                         ftp : Protokoll
                               
# KOmpletter FTP Scan Metasploit
msfconsole -q -x 'use auxiliary/scanner/ftp/anonymous; set RHOSTS 10.9.9.109; set RPORT 21; run; exit' &#x26;&#x26; msfconsole -q -x 'use auxiliary/scanner/ftp/ftp_version; set RHOSTS 10.9.9.109; set RPORT 21; run; exit' &#x26;&#x26; msfconsole -q -x 'use auxiliary/scanner/ftp/bison_ftp_traversal; set RHOSTS 10.9.9.109; set RPORT 21; run; exit' &#x26;&#x26; msfconsole -q -x 'use auxiliary/scanner/ftp/colorado_ftp_traversal; set RHOSTS 10.9.9.109; set RPORT 21; run; exit' &#x26;&#x26;  msfconsole -q -x 'use auxiliary/scanner/ftp/titanftp_xcrc_traversal; set RHOSTS 10.9.9.109; set RPORT 21; run; exit'
</code></pre>

## SMTP:25 ( Simple Mail Transfer Protocol )

[https://www.afternerd.com/blog/smtp/](https://www.afternerd.com/blog/smtp/)

Sprache zur Email-Kommunikation. **Klartext**.\
Hierbei werden folgende Akteure benötigt:

* MSA ( Mail Submission Agent )
* MTA ( Mail Transfer Agent )
* MDA ( Mail Delivery Agent )
* MUA ( Mail User Agent )

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/ed910ad418376edc846846fc2a0dd3f6.png" alt=""><figcaption></figcaption></figure>

1. Der Mail-User-Agent, also entweder Ihr E-Mail-Client oder ein externes Programm, verbindet sich mit dem SMTP-Server Ihrer Domäne, z. B. smtp.google.com. Dadurch wird der SMTP-Handshake eingeleitet. Diese Verbindung funktioniert über den SMTP-Port, der normalerweise 25 ist. Sobald diese Verbindungen hergestellt und validiert sind, beginnt die SMTP-Sitzung. `Ein Mail User Agent (MUA), oder einfach ein E-Mail-Client, hat eine E-Mail-Nachricht zu versenden. Der MUA stellt eine Verbindung zu einem Mail Submission Agent (MSA) her, um seine Nachricht zu senden.`&#x20;
2. Der Versand von E-Mails kann nun beginnen. Der Client übermittelt zunächst die E-Mail-Adresse des Absenders und des Empfängers, den Text der E-Mail und eventuelle Anhänge an den Server. `Der MSA empfängt die Nachricht, prüft sie auf Fehler und leitet sie dann an den Mail Transfer Agent (MTA)-Server weiter, der sich in der Regel auf demselben Server befindet.`&#x20;
3. Der SMTP-Server überprüft dann, ob der Domänenname des Empfängers und des Absenders identisch sind. `Der MTA sendet die E-Mail-Nachricht an den MTA des Empfängers. Der MTA kann auch als Mail Submission Agent (MSA) fungieren.`
4. Der SMTP-Server des Absenders stellt eine Verbindung zum SMTP-Server des Empfängers her, bevor er die E-Mail weiterleitet. Wenn auf den Server des Empfängers nicht zugegriffen werden kann oder dieser nicht verfügbar ist, wird die E-Mail in eine SMTP-Warteschlange gestellt. `Bei einer typischen Einrichtung fungiert der MTA-Server auch als Mail Delivery Agent (MDA).` Dann überprüft der SMTP-Server des Empfängers die eingehende E-Mail. Dazu prüft er, ob die Domäne und der Benutzername erkannt wurden. Der Server leitet die E-Mail dann an den POP- oder IMAP-Server weiter, wie in der Abbildung oben dargestellt.
5. Die E-Mail wird dann im Posteingang des Empfängers angezeigt. `Der Empfänger holt seine E-Mail mithilfe seines E-Mail-Clients vom MDA ab.`

<table><thead><tr><th width="233">Befehl</th><th>Bedeutung</th></tr></thead><tbody><tr><td><code>helo hostname</code></td><td>Bei MSA anmelden. Hostname kann telnet, etc. sein.</td></tr><tr><td><code>mail from: xxx</code></td><td>Sender und</td></tr><tr><td><code>rcpt to: xxx</code></td><td>Empfänger angeben</td></tr><tr><td><code>data</code></td><td>Dateneingabe starten</td></tr><tr><td><code>.</code></td><td>Eingabe mit Punkt <code>"."</code> beenden</td></tr><tr><td><code>quit</code></td><td>Beenden</td></tr></tbody></table>

## POP3:110 ( Post Office Protocol Version 3 )

Protokoll zum Abrufen der Nachrichten

<table><thead><tr><th width="233">Befehl</th><th>Bedeutung</th></tr></thead><tbody><tr><td><code>USER xxx</code></td><td>Usernamen angeben</td></tr><tr><td><code>PASS xxx</code></td><td>Passwort angeben</td></tr><tr><td><code>STAT</code></td><td>Ergebnis in Format ( +OK nn mm ) mit nn=Anzahl Emails in Mailbox und mm=Größe der Mailbox in byte</td></tr><tr><td><code>LIST</code></td><td>Liste neuer Emails</td></tr><tr><td><code>RETR 1</code></td><td>Ersten Eintrag in Liste aufrufen</td></tr><tr><td><code>QUIT</code></td><td>Beenden</td></tr></tbody></table>

## IMAP:143 ( Internet Message Access Protocol )

Auch ein Protokoll zum Abrufen der Nachrichten.\
IMAP behält Read-Status von Mails und kann deshalb zur Synchronisation über mehrere Geräte und Mail-Clients synchronisiert werden.

Protokoll zum Abrufen der Nachrichten

Vor jedem Befehl muss ein Kürzel stehen, das zur Identifikation der Antwort verwendet wird

<table><thead><tr><th width="348">Befehl</th><th>Bedeutung</th></tr></thead><tbody><tr><td><code>cX LOGIN &#x3C;username> &#x3C;password></code></td><td>Login (+ kürzel davor)</td></tr><tr><td><code>LIST "" "*"</code></td><td>Mailordner auflisten</td></tr><tr><td>EXAMINE &#x3C;mailboxname></td><td>Inhalt der Mailbox anzeigen</td></tr><tr><td>LOGOUT</td><td>Beenden</td></tr></tbody></table>

## TLS ( Transfer Layer Security )

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/9d265987c58331f6fd2f664837f85380.png" alt=""><figcaption></figcaption></figure>

TLS ist sicherer als SSL. DNS kann auch mit TLS verschlüsselt werden. Dann lautet es DoT für DNS on TLS.

<table><thead><tr><th width="145">Protocol</th><th width="181">Default Port</th><th>Secured Protocol</th><th>Default Port with TLS</th></tr></thead><tbody><tr><td>HTTP</td><td>80</td><td>HTTPS</td><td>443</td></tr><tr><td>FTP</td><td>21</td><td>FTPS</td><td>990</td></tr><tr><td>SMTP</td><td>25</td><td>SMTPS</td><td>465</td></tr><tr><td>POP3</td><td>110</td><td>POP3S</td><td>995</td></tr><tr><td>IMAP</td><td>143</td><td>IMAPS</td><td>993</td></tr></tbody></table>

Verbindung mit einem Webserver mit HTTPS ist wie folgt

* TCP Verbindung herstellen
* SSL/TLS Verbindung herstellen
* HTTP Request an Webserver senden

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/ea654470ae699d10e9c07bd11a8320ac.png" alt=""><figcaption></figcaption></figure>

1. Der Client sendet ein **ClientHello** an den Server, um seine Fähigkeiten, z. B. die unterstützten Algorithmen, anzugeben.
2. Der Server antwortet mit einem **ServerHello**, das die ausgewählten Verbindungsparameter angibt. Der Server stellt sein Zertifikat zur Verfügung, wenn eine Server-Authentifizierung erforderlich ist. Das Zertifikat ist eine digitale Datei zur Identifizierung des Servers; es wird in der Regel von einem Dritten digital signiert. Darüber hinaus kann er in seiner **ServerKeyExchange**-Nachricht zusätzliche Informationen zur Erzeugung des Hauptschlüssels übermitteln, bevor er die **ServerHelloDone**-Nachricht sendet, um anzuzeigen, dass er mit der Aushandlung fertig ist.
3. Der Client antwortet mit einer **ClientKeyExchange**-Nachricht, die zusätzliche Informationen enthält, die zur Generierung des Hauptschlüssels erforderlich sind. Außerdem schaltet er auf Verschlüsselung um und teilt dies dem Server mit der Nachricht **ChangeCipherSpec** mit.
4. Der Server schaltet ebenfalls auf Verschlüsselung um und teilt dies dem Client in der Nachricht **ChangeCipherSpec** mit.

## SSH:22 ( Secure Shell )

{% code overflow="wrap" %}
```
# Public/Private-key generieren
ssh-keygen -t rsa -b 4096

# ssh-Verbindung + mit id_rsa identitätslogin
ssh <username>@<ip>:<port>
ssh -i .ssh/id_rsa <username>@<ip>:<port>

# ssh Transfer
scp <username>@<ip>:/<pfad> # von lokal -> zielpfad
scp <username>@<ip>:/<pfad> <lokal> # von von zielpfad -> lokal

# nano /etc/ssh/sshd_config
Hier wird die Einstellung PasswordAuthentication yes zu PasswordAuthentication no geändert.

nun muss der SSH Dienst auf dem Server noch neu gestartet werden

# sudo service ssh restart
Ein erneuter Test zeigt, dass nun der Login ohne Schlüssel nicht mehr möglich ist. 
```
{% endcode %}

## RDP:3389 ( Remote Desktop Protokoll )

```
xfreerdp /u:<username> /p:<password> /v:<ip>
```

## SMB:139/445 ( Server Message Block Protokoll )

SMB ist ein **Client**-**Server**-**Kommunikationsprotokoll** für den **gemeinsamen** **Zugriff** auf Dateien, Drucker, serielle Schnittstellen und andere Ressourcen in einem Netzwerk.

Server stellen den Clients im Netzwerk Dateisysteme und andere Ressourcen (Drucker, Named Pipes, APIs) zur Verfügung. Client-Computer haben zwar ihre eigenen Festplatten, möchten aber auch auf die gemeinsamen Dateisysteme und Drucker auf den Servern zugreifen.

Das SMB-Protokoll ist als **Antwort**-**Anfrage**-**Protokoll** bekannt, was bedeutet, dass es mehrere Nachrichten zwischen Client und Server überträgt, um eine Verbindung herzustellen. Clients verbinden sich mit Servern über TCP/IP (eigentlich NetBIOS über TCP/IP, wie in RFC1001 und RFC1002 beschrieben), NetBEUI oder IPX/SPX.

{% code title="SMB in nmap" %}
```
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
```
{% endcode %}

Enumeration mit: [enum4linux.md](../../programme-skripte/enum4linux.md "mention")

{% code title="SMB-Client" %}
```
smbclient //[IP]/[SHARE] -U <name> -p <port>p [port] : to specify the port


Sharename       Type      Comment
---------       ----      -------
ADMIN$          Disk      Remote Admin		 /
C$              Disk      Default share	      <-|  Default Share ( $ )
IPC$            IPC       Remote IPC		 \
WorkShares      Disk      		<- Custom Share ( kein $ )
```
{% endcode %}

#### Anonymer / Passwortloser Zugriff

```bash
smbclient //SERVER/SHARE -U "" -N
```

**Parameter erklärt:**

* `-U ""` → leerer Benutzername
* `-N` → keine Passwortabfrage (keine Eingabe nötig)

#### Suche nach Freigaben

```
smbclient -L //192.168.1.100 -U "" -N
```

## Telnet:beliebig

Telnet ist ein Anwendungsprotokoll, das es ermöglicht, mit Hilfe eines Telnet-Clients eine Verbindung zu einem entfernten Rechner herzustellen, auf dem sich ein Telnet-Server befindet, und dort Befehle auszuführen.

Der Telnet-Client stellt eine Verbindung mit dem Server her. Der Client wird dann zu einem virtuellen Terminal, über das Sie mit dem entfernten Host interagieren können.

Telnet sendet alle Nachrichten im Klartext und verfügt über keine speziellen Sicherheitsmechanismen. Daher wurde Telnet in vielen Anwendungen und Diensten in den meisten Fällen durch SSH ersetzt.

{% code title="Verbindung" %}
```
telnet <ip> <port>
```
{% endcode %}



## NFS ( Network File System )

[https://nfs.sourceforge.net/](https://nfs.sourceforge.net/)

Angriffsszenario :&#x20;

{% content-ref url="../../killchain/recon-and-initial-access/angriffsszenarien/netzwerkbasierte-schwachstellen/nfs.md" %}
[nfs.md](../../killchain/recon-and-initial-access/angriffsszenarien/netzwerkbasierte-schwachstellen/nfs.md)
{% endcontent-ref %}

1. Zuerst wird der Client das Einhängen eines Verzeichnisses von einem entfernten Host in ein lokales Verzeichnis anfordern, genauso wie er ein physisches Gerät einhängen kann. Der Einhängungsdienst stellt dann mittels RPC eine Verbindung zum entsprechenden Einhängungsdaemon her.
2. Der Server prüft, ob der Benutzer die Berechtigung hat, das angeforderte Verzeichnis einzuhängen. Er gibt dann ein Dateihandle zurück, das jede Datei und jedes Verzeichnis auf dem Server eindeutig identifiziert.
3. Wenn jemand über NFS auf eine Datei zugreifen möchte, wird ein **RPC**-Aufruf an NFSD (den NFS-Daemon) auf dem Server gesendet. Dieser Aufruf enthält Parameter wie:

* Das Dateihandle&#x20;
* den Namen der Datei, auf die zugegriffen werden soll&#x20;
* die Benutzer-ID des Benutzers&#x20;
* die Gruppen-ID des Benutzers

Diese Parameter werden verwendet, um die Zugriffsrechte auf die angegebene Datei zu bestimmen. Damit werden die Benutzerrechte gesteuert, d.h. das Lesen und Schreiben von Dateien.

### Enumeration

```
# Shares herausfinden
/usr/sbin/showmount -e <ip>

# NFS Share mounten
sudo mount -t nfs IP:share /tmp/mount/ -nolock
```

\[ ! ] Bei NFS-Freigaben ist standardmäßig das Root Squashing aktiviert, um zu verhindern, dass sich jemand mit der NFS-Freigabe verbindet und Root-Zugriff auf das NFS-Volume erhält. Remote-Root-Benutzer werden als Benutzer „nfsnobody” zugewiesen, der die geringsten lokalen Berechtigungen hat.&#x20;

Die Deaktivierung dieser Funktion birgt jedoch das Risiko, dass SUID-Bit-Dateien erstellt werden, wodurch ein Remote-Benutzer Root-Zugriff auf das verbundene System erhalten könnte.



## Redis:6379

Redis ist ein fortschrittlicher Open-Source-Schlüsselwertspeicher und eine geeignete Lösung für die Erstellung leistungsstarker, skalierbarer Webanwendungen.

[https://www.tutorialspoint.com/redis/redis\_overview.htm](https://www.tutorialspoint.com/redis/redis_overview.htm)

```
# Verbindung herstellen
redis-cli -h <ip>

# Komplette Config anzeigen, statt * kann auch der config_name eingegeben werden
config get *

# Config setzen
config set <config_name> <new_value>

# ping absetzen
ping

# Informationen und Statikstik
info

# Datenbank auswählen
select <index>

# Alle Keys ausgeben
keys *

# Key auslesen
get <key>

# aktive Verbindungen anzeigen
client list

# Verbindung schließen
client kill id <id>
```

### Server-Befehle

[https://www.tutorialspoint.com/redis/redis\_server.htm](https://www.tutorialspoint.com/redis/redis_server.htm)



