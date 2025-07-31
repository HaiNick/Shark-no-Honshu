---
description: Windows Management Instrumentation Command-line
---

# wmic & PS-wmi

{% hint style="info" %}
`wmic` ist ein klassisches Kommandozeilenwerkzeug zur Abfrage und Verwaltung von Systeminformationen über Windows Management Instrumentation (WMI). Es ist offiziell veraltet, jedoch weiterhin auf vielen Systemen verfügbar. Die PowerShell-Befehle `Get-WmiObject` und `Get-CimInstance` stellen moderne Alternativen dar. Alle Befehle erfordern administrative Rechte.
{% endhint %}

***

### Verwendung von `wmic` (Kommandozeile)

#### Allgemeine Syntax

```cmd
wmic [Klasse] [WHERE Bedingung] [GET | CALL | LIST] [Eigenschaften oder Methoden]
```

***

#### Häufig verwendete `wmic`-Befehle

| Zweck                       | Befehl                                                                                  |
| --------------------------- | --------------------------------------------------------------------------------------- |
| Betriebssysteminformationen | `wmic os get Caption, Version, BuildNumber`                                             |
| Computername und Modell     | `wmic computersystem get Name, Model, Manufacturer`                                     |
| BIOS-Seriennummer           | `wmic bios get SerialNumber`                                                            |
| Lokale Benutzerkonten       | `wmic useraccount get Name, SID, Disabled`                                              |
| Prozesse auflisten          | `wmic process list brief`                                                               |
| Prozess beenden             | `wmic process where "name='notepad.exe'" call terminate`                                |
| Software deinstallieren     | `wmic product where "name='Programmname'" call uninstall`                               |
| IP-Konfiguration anzeigen   | `wmic nicconfig where "IPEnabled=TRUE" get IPAddress`                                   |
| Autostartprogramme anzeigen | `wmic startup get Caption, Command`                                                     |
| Laufwerke anzeigen          | `wmic logicaldisk get DeviceID, FreeSpace, Size, VolumeName`                            |
| Remote-Befehl ausführen     | `wmic /node:"<IP>" /user:"<Benutzer>" /password:"<Passwort>" process call create "..."` |

> Hinweis: Der Befehl `wmic product` funktioniert ausschließlich mit MSI-basierten Installationen und kann unvollständige Ergebnisse liefern.

***

### PowerShell-WMI-Äquivalente

Die PowerShell bietet mit `Get-WmiObject` (älter) und `Get-CimInstance` (empfohlen) leistungsfähige Alternativen zu `wmic`. Die folgenden Tabellen zeigen häufige Anwendungsfälle:

#### Vergleich von `wmic` mit PowerShell-Alternativen

| Zweck                       | Get-WmiObject                                     | Get-CimInstance                                     |
| --------------------------- | ------------------------------------------------- | --------------------------------------------------- |
| Betriebssysteminformationen | `Get-WmiObject -Class Win32_OperatingSystem`      | `Get-CimInstance -ClassName Win32_OperatingSystem`  |
| Computername und Modell     | `Get-WmiObject Win32_ComputerSystem`              | `Get-CimInstance Win32_ComputerSystem`              |
| BIOS-Informationen          | `Get-WmiObject Win32_BIOS`                        | `Get-CimInstance Win32_BIOS`                        |
| Benutzerkonten anzeigen     | `Get-WmiObject Win32_UserAccount`                 | `Get-CimInstance Win32_UserAccount`                 |
| Prozesse anzeigen           | `Get-WmiObject Win32_Process`                     | `Get-CimInstance Win32_Process`                     |
| Prozess beenden             | `$p.Terminate()`                                  | `Invoke-CimMethod -MethodName Terminate`            |
| Software anzeigen           | `Get-WmiObject Win32_Product`                     | `Get-CimInstance Win32_Product`                     |
| IP-Konfiguration            | `Get-WmiObject Win32_NetworkAdapterConfiguration` | `Get-CimInstance Win32_NetworkAdapterConfiguration` |
| Autostartprogramme          | `Get-WmiObject Win32_StartupCommand`              | `Get-CimInstance Win32_StartupCommand`              |
| Laufwerke anzeigen          | `Get-WmiObject Win32_LogicalDisk`                 | `Get-CimInstance Win32_LogicalDisk`                 |

***

### Übersicht wichtiger WMI-Klassen

| WMI-Klasse                          | Beschreibung                                       |
| ----------------------------------- | -------------------------------------------------- |
| `Win32_OperatingSystem`             | Informationen über das installierte Betriebssystem |
| `Win32_ComputerSystem`              | Modell, Name, Hersteller und Benutzer              |
| `Win32_BIOS`                        | BIOS-Version, Seriennummer                         |
| `Win32_UserAccount`                 | Lokale Benutzerkonten                              |
| `Win32_Process`                     | Alle laufenden Prozesse                            |
| `Win32_Product`                     | Installierte MSI-basierte Software                 |
| `Win32_NetworkAdapterConfiguration` | Netzwerkadapter, IP-Konfigurationen                |
| `Win32_StartupCommand`              | Programme, die beim Start ausgeführt werden        |
| `Win32_LogicalDisk`                 | Informationen zu logischen Laufwerken              |

***

### PowerShell-Beispiel zur Remotesteuerung mit CIM

```powershell
$Session = New-CimSession -ComputerName "192.168.1.10"
Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine = "cmd.exe /c calc.exe"}
```

> Voraussetzung: Der Zielcomputer muss über WSMan (PowerShell Remoting) oder DCOM erreichbar sein. Berechtigungen und Netzwerkfreigaben müssen korrekt konfiguriert sein.

***

{% hint style="info" %}
* `wmic` bietet eine einfache Text-basierte Oberfläche für viele administrative Aufgaben, ist jedoch veraltet.
* `Get-CimInstance` ist der bevorzugte Weg für moderne PowerShell-Skripte, insbesondere bei Remoteverwaltung.
* Viele WMI-Klassen ermöglichen tiefe Einblicke in Systemkonfiguration und Prozesse.
* Für automatisierte Aufgaben oder Remotezugriffe empfiehlt sich die Kombination aus `CIM`-Befehlen und `Invoke-CimMethod`.
{% endhint %}
