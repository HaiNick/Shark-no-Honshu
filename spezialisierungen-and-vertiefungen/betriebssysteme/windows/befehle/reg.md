---
description: Windows Registry
---

# reg

{% hint style="info" %}
Alle Befehle sollten mit **administrativen Rechten** in der Eingabeaufforderung (`cmd.exe`) oder in Skripten (z. B. `.bat`) ausgeführt werden.
{% endhint %}

## Allgemeine Syntax

```cmd
reg [add|delete|query|import|export|save|restore|load|unload|compare|copy] [Pfad] [Optionen]
```

***

### Schlüssel hinzufügen / ändern

```cmd
reg add [Pfad] /v [Wertname] /t [Typ] /d [Daten] [/f]
```

**Beispiel:**

```cmd
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1 /f
```

| Option | Bedeutung                                                               |
| ------ | ----------------------------------------------------------------------- |
| `/v`   | Name des Werts                                                          |
| `/t`   | Typ (REG\_SZ, REG\_DWORD, REG\_BINARY, REG\_MULTI\_SZ, REG\_EXPAND\_SZ) |
| `/d`   | Datenwert                                                               |
| `/f`   | Erzwingen ohne Rückfrage                                                |

***

### Schlüssel oder Wert löschen

```cmd
reg delete [Pfad] [/v Wertname | /ve | /va] [/f]
```

**Beispiele:**

```cmd
reg delete HKCU\Software\TestKey /v Wert1 /f
reg delete HKCU\Software\TestKey /va /f
reg delete HKCU\Software\TestKey /f
```

| Option | Bedeutung                |
| ------ | ------------------------ |
| `/v`   | Einzelnen Wert löschen   |
| `/ve`  | Standardwert löschen     |
| `/va`  | Alle Werte löschen       |
| `/f`   | Ohne Bestätigung löschen |

***

### Schlüssel oder Werte anzeigen (query)

```cmd
reg query [Pfad] [/v Wertname | /ve | /s]
```

**Beispiele:**

```cmd
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion
reg query HKCU\Software\TestKey /v Wert1
reg query HKLM\SOFTWARE /s
```

| Option | Bedeutung                             |
| ------ | ------------------------------------- |
| `/v`   | Zeigt nur bestimmten Wert             |
| `/ve`  | Zeigt Standardwert                    |
| `/s`   | Rekursiv alle Unterschlüssel anzeigen |

***

### Exportieren und Importieren

#### Exportieren (als `.reg` Datei):

```cmd
reg export [Pfad] [Datei] [/y]
```

**Beispiel:**

```cmd
reg export HKCU\Software\TestKey backup.reg
```

#### Importieren:

```cmd
reg import [Datei]
```

**Beispiel:**

```cmd
reg import backup.reg
```

***

### Registrierung sichern und wiederherstellen (Backup / Restore)

#### Sichern (binäres Format):

```cmd
reg save [Pfad] [Datei] [/y]
```

**Beispiel:**

```cmd
reg save HKLM\System system_backup.hiv
```

#### Wiederherstellen:

```cmd
reg restore [Pfad] [Datei]
```

**Beispiel:**

```cmd
reg restore HKLM\System system_backup.hiv
```

> Warnung: Diese Befehle überschreiben Daten. Nur bei Systemstart im abgesicherten Modus möglich für aktive Hives.

***

### Registrierungshives laden und entladen (z. B. für Offline-Bearbeitung)

#### Laden:

```cmd
reg load [Pfad] [Datei]
```

**Beispiel:**

```cmd
reg load HKLM\TempHive C:\Users\test\NTUSER.DAT
```

#### Entladen:

```cmd
reg unload [Pfad]
```

**Beispiel:**

```cmd
reg unload HKLM\TempHive
```

***

### Schlüssel vergleichen

```cmd
reg compare [Pfad1] [Pfad2] [/v Wertname | /ve | /s]
```

**Beispiel:**

```cmd
reg compare HKLM\Software\Test1 HKLM\Software\Test2 /s
```

***

### Schlüssel kopieren

```cmd
reg copy [Quelle] [Ziel] [/s | /se] [/f]
```

**Beispiel:**

```cmd
reg copy HKCU\Software\Alt HKCU\Software\Neu /s /f
```

| Option | Bedeutung                       |
| ------ | ------------------------------- |
| `/s`   | Alle Unterschlüssel mitkopieren |
| `/se`  | Nur Struktur (ohne Werte)       |
| `/f`   | Ohne Rückfrage überschreiben    |

***

{% hint style="info" %}
### Weitere Hinweise

* Registry-Pfade müssen **ohne führendes `HKEY_`** angegeben werden (z. B. `HKLM` statt `HKEY_LOCAL_MACHINE`).
* Unter `HKLM\SYSTEM` und `HKU\.DEFAULT` gelten **besondere Berechtigungen**.
* Automatisierung mit `.bat` oder PowerShell möglich.
{% endhint %}
