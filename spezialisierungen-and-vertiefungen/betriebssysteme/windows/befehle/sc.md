---
description: Dienststeuerung über CLI
---

# sc

{% hint style="info" %}
`sc.exe` (Service Control) ist ein Befehlszeilentool zur Verwaltung von Windows-Diensten. Es kann Dienste erstellen, löschen, starten, stoppen, konfigurieren und abfragen – lokal oder remote.\
Alle Befehle sollten mit **administrativen Rechten** ausgeführt werden.
{% endhint %}

Siehe auch: [#service-fehlkonfigurationen](../../../../killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/windows-privesc.md#service-fehlkonfigurationen "mention")

***

### Allgemeine Syntax

```cmd
sc.exe [\\Computername] Befehl [Dienstname] [Optionen]
```

* `\\Computername` ist optional. Wird kein Zielsystem angegeben, wird der Befehl lokal ausgeführt.
* `[Dienstname]` ist der tatsächliche Name des Dienstes (nicht der Anzeigename).

***

### Häufige Befehle

| Befehl               | Zweck                                        |
| -------------------- | -------------------------------------------- |
| `query`              | Dienststatus anzeigen                        |
| `start`              | Dienst starten                               |
| `stop`               | Dienst stoppen                               |
| `pause` / `continue` | Dienst pausieren / fortsetzen                |
| `config`             | Dienstkonfiguration ändern                   |
| `create`             | Neuen Dienst erstellen                       |
| `delete`             | Dienst löschen                               |
| `qc`                 | Dienstkonfiguration anzeigen                 |
| `sdshow` / `sdset`   | Sicherheitsbeschreibung anzeigen oder ändern |
| `failure`            | Fehlerverhalten konfigurieren                |
| `description`        | Beschreibung eines Dienstes ändern           |

***

### Beispiele und Optionen

#### Dienst anzeigen

```cmd
sc query wuauserv
```

> Zeigt den Status des Windows Update-Dienstes.

***

#### Dienst starten / stoppen

```cmd
sc start Spooler
sc stop Spooler
```

***

#### Dienstkonfiguration anzeigen

```cmd
sc qc Spooler
```

***

#### Dienstbeschreibung anzeigen / setzen

```cmd
sc description Spooler "Verwaltet Druckwarteschlangen"
```

***

#### Dienst erstellen

```cmd
sc create MeinDienst binPath= "C:\Tools\meindienst.exe" start= auto DisplayName= "Mein Dienst"
```

**Wichtige Optionen bei `create`:**

| Option         | Bedeutung                                                           |
| -------------- | ------------------------------------------------------------------- |
| `binPath=`     | Pfad zur ausführbaren Datei (Pflicht, **= ohne Leerzeichen davor**) |
| `DisplayName=` | Anzeigename in der Diensteverwaltung                                |
| `start=`       | Starttyp: `auto`, `demand`, `disabled`                              |
| `type=`        | Typ: `own`, `share`, `interact`, `kernel`, `filesys`, `rec`         |
| `obj=`         | Benutzerkonto, unter dem der Dienst ausgeführt wird                 |
| `password=`    | Kennwort für oben angegebenes Konto (optional bei `LocalSystem`)    |

> Hinweis: Zwischen Schlüssel und Wert steht ein **Leerzeichen**, nicht zwischen `=`.

***

#### Dienst löschen

```cmd
sc delete MeinDienst
```

> Vorsicht: Der Dienst wird **dauerhaft entfernt**.

***

#### Fehlerverhalten konfigurieren

```cmd
sc failure MeinDienst reset= 60 actions= restart/5000/restart/5000/""/5000
```

**Bedeutung:**

* Dienst wird nach einem Fehler 2× neu gestartet, dann keine Aktion.
* `reset=` legt fest, nach wie vielen Sekunden die Fehleranzahl zurückgesetzt wird.

***

#### Sicherheit anzeigen oder setzen

```cmd
sc sdshow Dienstname
sc sdset Dienstname D:(A;;CCLCSWRPWPDTLOCRRC;;;SY)
```

> DACL in SDDL-Notation zum Festlegen von Berechtigungen für Dienste.

***

#### Remote-Verwendung

```cmd
sc \\192.168.1.100 query Spooler
```

> Setzt voraus, dass der Remote-PC erreichbar und der Benutzer berechtigt ist.

***

{% hint style="info" %}
Weitere Hinweise

* Dienste haben einen **kurzen internen Namen** (z. B. `wuauserv` für "Windows Update").
* `sc.exe` ist besonders nützlich in Skripten und Automatisierungen.
* Für komplexe Berechtigungsszenarien empfiehlt sich ergänzend `subinacl.exe` oder PowerShell mit `Set-Service`, `Get-Service`.
{% endhint %}

***

### PowerShell-Äquivalente

| Funktion                 | PowerShell                                                       |
| ------------------------ | ---------------------------------------------------------------- |
| Dienst anzeigen          | `Get-Service -Name "Spooler"`                                    |
| Dienst starten / stoppen | `Start-Service -Name "Spooler"` / `Stop-Service -Name "Spooler"` |
| Dienst erstellen         | Kein direktes Äquivalent, nur über Registry oder .NET            |
| Dienst löschen           | Nur über WMI oder `sc.exe`                                       |
| Dienst konfigurieren     | `Set-Service` für Starttyp; vollständige Konfig nur mit WMI      |

***

{% hint style="info" %}
`sc.exe` bietet eine robuste Kommandozeilenschnittstelle zur Verwaltung von Windows-Diensten. Es ermöglicht präzise Steuerung und Automatisierung in lokalen und entfernten Umgebungen. Bei modernen Skripten ist PowerShell vorzuziehen, insbesondere für Fehlerbehandlung und Objektorientierung.
{% endhint %}
