---
description: Windows-Kommandozeilenwerkzeug zur Verwaltung von geplanten Aufgaben
---

# schtasks

{% hint style="info" %}
Mit `schtasks.exe` können geplante Aufgaben (Scheduled Tasks) unter Windows erstellt, bearbeitet, gelöscht, gestartet, beendet und angezeigt werden – sowohl lokal als auch auf entfernten Computern.\
Die Nutzung erfordert **administrative Berechtigungen**, insbesondere beim Bearbeiten systemweiter Aufgaben oder für Aktionen im Systemkontext.
{% endhint %}

***

### Allgemeine Syntax

```cmd
schtasks.exe /Befehl [Optionen]
```

***

### Häufige Befehle

| Befehl    | Beschreibung                          |
| --------- | ------------------------------------- |
| `/create` | Erstellt eine neue geplante Aufgabe   |
| `/delete` | Löscht eine vorhandene Aufgabe        |
| `/query`  | Zeigt geplante Aufgaben an            |
| `/run`    | Startet eine Aufgabe sofort           |
| `/end`    | Beendet eine aktuell laufende Aufgabe |
| `/change` | Ändert Eigenschaften einer Aufgabe    |

***

### Wichtige Optionen bei `/create`

```cmd
schtasks /create /tn "MeinTask" /tr "notepad.exe" /sc daily /st 12:00
```

| Option | Bedeutung                                                                                                                       |
| ------ | ------------------------------------------------------------------------------------------------------------------------------- |
| `/tn`  | Taskname (Pfadangabe möglich, z. B. `\Ordner\Task1`)                                                                            |
| `/tr`  | Befehl oder Pfad zum auszuführenden Programm                                                                                    |
| `/sc`  | Zeitplan: `once`, `minute`, `hourly`, `daily`, `weekly`, `monthly`, `onstart`, `onlogon`, `onidle`, `onconnect`, `ondisconnect` |
| `/st`  | Startzeit im Format `HH:MM` (24h)                                                                                               |
| `/sd`  | Startdatum im Format `TT/MM/JJJJ`                                                                                               |
| `/ru`  | Benutzername für Ausführung (`SYSTEM`, `Benutzername`)                                                                          |
| `/rp`  | Passwort für `/ru` (nicht erforderlich bei SYSTEM)                                                                              |
| `/rl`  | Ausführungsstufe: `LIMITED` oder `HIGHEST`                                                                                      |
| `/mo`  | Modifikator, z. B. alle 5 Minuten: `/mo 5`                                                                                      |
| `/d`   | Wochentag oder Tag (bei `weekly`, `monthly`)                                                                                    |
| `/it`  | Interaktive Ausführung zulassen (sichtbares Fenster)                                                                            |
| `/f`   | Erzwingt Überschreiben vorhandener Aufgaben                                                                                     |

***

### Beispiele

#### Tägliche Aufgabe um 8 Uhr als SYSTEM

```cmd
schtasks /create /tn "TäglicherTask" /tr "C:\script\job.bat" /sc daily /st 08:00 /ru SYSTEM
```

***

#### Einmalige Aufgabe mit Benutzer

```cmd
schtasks /create /tn "EinmalTask" /tr "notepad.exe" /sc once /st 14:00 /sd 17/05/2025 /ru Benutzer /rp Passwort
```

***

#### Aufgabe löschen

```cmd
schtasks /delete /tn "TäglicherTask" /f
```

***

#### Aufgabe anzeigen

```cmd
schtasks /query /tn "TäglicherTask" /v /fo LIST
```

| Option           | Beschreibung                             |
| ---------------- | ---------------------------------------- |
| `/v`             | Ausführliche Informationen anzeigen      |
| `/fo`            | Ausgabeformat: `TABLE`, `LIST`, `CSV`    |
| `/s`, `/u`, `/p` | Für Remote-Zugriff mit Authentifizierung |

***

#### Aufgabe manuell starten oder beenden

```cmd
schtasks /run /tn "TäglicherTask"
schtasks /end /tn "TäglicherTask"
```

***

#### Aufgabe ändern (z. B. Startzeit)

```cmd
schtasks /change /tn "TäglicherTask" /st 09:30
```

{% hint style="info" %}
Hinweis: Nicht alle Eigenschaften lassen sich mit `/change` modifizieren. In komplexen Fällen empfiehlt sich das Löschen und Neuerstellen der Aufgabe.
{% endhint %}

***

{% hint style="warning" %}
Sicherheitshinweise

* Für `/ru SYSTEM` wird kein Passwort benötigt.
* `/rp *` fordert zur Eingabe des Passworts auf.
* Die Passwortübertragung bei `/rp` erfolgt im Klartext (nicht sicher in Skripten).
* Standardmäßig werden Aufgaben im Kontext des Benutzers ausgeführt, der sie erstellt.
{% endhint %}

***

### PowerShell-Äquivalente

| Funktion          | PowerShell-Befehl          |
| ----------------- | -------------------------- |
| Aufgabe erstellen | `Register-ScheduledTask`   |
| Aufgabe anzeigen  | `Get-ScheduledTask`        |
| Aufgabe starten   | `Start-ScheduledTask`      |
| Aufgabe stoppen   | `Stop-ScheduledTask`       |
| Aufgabe löschen   | `Unregister-ScheduledTask` |

***

### Besonderheiten und Tipps

* Aufgaben werden standardmäßig unter dem Pfad `\` gespeichert (Root). Unterordner sind mit `\Ordner\Taskname` möglich.
* Zur Anzeige aller Aufgaben inklusive versteckter Tasks empfiehlt sich `/query /fo LIST /v`.
* Aufgaben lassen sich auch als `.xml` exportieren und importieren (`/xml` bei `schtasks /create`).

***

{% hint style="info" %}
`schtasks.exe` ist ein leistungsfähiges Werkzeug zur Automatisierung zeitgesteuerter Prozesse in Windows-Umgebungen. Für komplexere Szenarien empfiehlt sich ergänzend der Einsatz von PowerShell oder die grafische Aufgabenplanung (taskschd.msc).\
Besonders in Skriptumgebungen ist `schtasks.exe` wegen seiner Stabilität und weiten Kompatibilität unverzichtbar.
{% endhint %}
