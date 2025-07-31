---
description: Systemverwaltungswerkzeug
---

# PsExec64.exe

{% hint style="info" %}
`PsExec64.exe` ist ein Systemverwaltungswerkzeug aus der Sysinternals Suite, das es ermöglicht, Prozesse **lokal oder remote** auszuführen – inklusive mit erhöhten Rechten. Es ist besonders nützlich für administrative Aufgaben in großen Netzwerken oder auf Servern ohne interaktive Anmeldung.\
Für 32-Bit-Systeme steht die Variante `PsExec.exe` zur Verfügung.\
**Hinweis**: Die Ausführung erfordert administrative Berechtigungen sowohl lokal als auch ggf. auf dem Zielsystem.
{% endhint %}

### Allgemeine Syntax

```cmd
PsExec64.exe [Optionen] \\Zielcomputer [Pfad\]Befehl [Argumente]
```

Für lokale Ausführung genügt:

```cmd
PsExec64.exe [Optionen] Befehl [Argumente]
```

***

### Wichtige Optionen

| Option          | Bedeutung                                                                 |
| --------------- | ------------------------------------------------------------------------- |
| `\\Computer`    | Zielrechner (optional, Standard: lokal)                                   |
| `-s`            | Führe den Befehl im Kontext des **Systemkontos** aus                      |
| `-i`            | Interaktive Sitzung (z. B. für GUI-Programme; Standard: Sitzung 1)        |
| `-i <ID>`       | Bestimmte Sitzung angeben                                                 |
| `-d`            | Prozess asynchron starten (nicht auf Rückgabe warten)                     |
| `-h`            | Mit erhöhten Rechten starten (nur bei UAC erforderlich)                   |
| `-u Benutzer`   | Anderer Benutzername für Authentifizierung                                |
| `-p Passwort`   | Passwort für obigen Benutzer                                              |
| `-accepteula`   | Akzeptiert die EULA automatisch (notwendig beim ersten Start)             |
| `-c`            | Kopiert die angegebene ausführbare Datei auf den Zielcomputer             |
| `-f`            | Erzwingt Kopieren der Datei auch bei vorhandener Kopie                    |
| `-v`            | Überprüft die Dateiversion vor dem Kopieren                               |
| `-n <Sekunden>` | Timeout für Verbindung in Sekunden                                        |
| `-x`            | Startet die Anwendung im getrennten Fenster (nur interaktiv sinnvoll)     |
| `-low`, `-high` | Prozess mit niedriger oder hoher Priorität starten                        |
| `-e`            | Umgebung des aktuellen Benutzers verwenden (standardmäßig Systemumgebung) |

***

### Anwendungsbeispiele

#### Lokale Ausführung mit SYSTEM-Rechten

```cmd
PsExec64.exe -s cmd.exe
```

#### Lokale interaktive GUI-Anwendung als SYSTEM

```cmd
PsExec64.exe -s -i notepad.exe
```

#### Remote-Ausführung eines Befehls

```cmd
PsExec64.exe \\192.168.1.10 ipconfig /all
```

#### Remote-Ausführung mit Benutzeranmeldung

```cmd
PsExec64.exe \\server01 -u Domäne\Admin -p Passwort cmd.exe
```

#### Remote-Kopieren und Starten einer Datei

```cmd
PsExec64.exe \\192.168.1.5 -c C:\Tools\flag.exe
```

> Hinweis: Der Dienst "PsExecSvc" wird temporär auf dem Zielsystem installiert. Er bleibt ggf. zurück, wenn der Prozess abstürzt oder gewaltsam beendet wird.

***

### Hinweise zur Sicherheit und Firewall

* Die Kommunikation erfolgt standardmäßig über **Port 445 (SMB)**.
* PsExec verwendet für Authentifizierung **NTLM** oder Kerberos (bei Domänenbetrieb).
* Die Zielsysteme müssen **Remote-Dienstzugriff (Admin-Share, z. B. C$)** und **RPC** zulassen.
* **Windows Defender SmartScreen oder AV-Software** blockieren PsExec häufig – dies sollte bei Tests berücksichtigt werden.

***

### Alternativen in PowerShell

| Funktion                         | PowerShell-Alternative                                 |
| -------------------------------- | ------------------------------------------------------ |
| Remote-Prozess starten           | `Invoke-Command`, `Start-Process` über `New-PSSession` |
| Interaktive lokale SYSTEM-Shell  | `psexec -s -i` → Kein direktes PowerShell-Äquivalent   |
| Datei remote ausführen mit Kopie | Kombination aus `Copy-Item` + `Invoke-Command`         |

{% hint style="info" %}
`PsExec64.exe` ermöglicht flexible und tiefe Kontrolle über lokale und entfernte Systeme. Für moderne Skripting- und Remote-Verwaltung empfiehlt sich jedoch zunehmend PowerShell mit Remoting-Funktionalität (`WinRM`, `CIM`). In sicherheitskritischen Umgebungen sollte PsExec nur in kontrollierten Szenarien verwendet werden.
{% endhint %}
