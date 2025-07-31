---
description: Windows Sicherheitsrichtlinien
---

# secedit

{% hint style="info" %}
Alle Befehle sollten mit **administrativen Rechten** in der Eingabeaufforderung (`cmd.exe`) oder in Skripten (z. B. `.bat`) ausgeführt werden.
{% endhint %}

### Allgemeine Syntax

```
secedit [operation] /cfg [Konfigurationsdatei] [weitere Optionen]
```

***

### Sicherheitsrichtlinie analysieren

```
secedit /analyze /db [Pfad zur Datenbank] /cfg [Inf-Datei] [/log [Logdatei]]
```

**Beispiel:**

```
secedit /analyze /db C:\temp\analyze.sdb /cfg C:\temp\secpol.inf /log C:\temp\analyze.log
```

| **Option** | **Bedeutung**                                |
| ---------- | -------------------------------------------- |
| `/analyze` | Analyse der Konfiguration                    |
| `/db`      | Pfad zur lokalen Sicherheitsdatenbank (.sdb) |
| `/cfg`     | Pfad zur Sicherheitsvorlagendatei (.inf)     |
| `/log`     | Protokollausgabe                             |

***

### Sicherheitsrichtlinie anwenden (konfigurieren)

```
secedit /configure /db [Pfad zur Datenbank] /cfg [Inf-Datei] [/overwrite] [/log [Logdatei]]
```

**Beispiel:**

```
secedit /configure /db C:\temp\secure.sdb /cfg C:\temp\secpol.inf /overwrite /log C:\temp\secure.log
```

| **Option**   | **Bedeutung**                                          |
| ------------ | ------------------------------------------------------ |
| `/configure` | Wendet die Richtlinie auf das System an                |
| `/overwrite` | Überschreibt bestehende Konfiguration in der Datenbank |

***

### Sicherheitsrichtlinie exportieren

```
secedit /export /cfg [Ziel-INF-Datei] /log [Logdatei]
```

**Beispiel:**

```
secedit /export /cfg C:\temp\export.inf /log C:\temp\export.log
```

| **Option** | **Bedeutung**                                |
| ---------- | -------------------------------------------- |
| `/export`  | Exportiert aktuelle Richtlinieneinstellungen |
| `/cfg`     | Ziel-Dateipfad für Export (.inf)             |

***

### Sicherheitsdatenbank zurücksetzen (Rollback)

```
secedit /refreshpolicy machine_policy [/enforce]
secedit /refreshpolicy user_policy [/enforce]
```

**Beispiel:**

```
secedit /refreshpolicy machine_policy /enforce
```

| **Option**       | **Bedeutung**                      |
| ---------------- | ---------------------------------- |
| `/refreshpolicy` | Erneuert lokale Gruppenrichtlinien |
| `/enforce`       | Erzwingt sofortige Anwendung       |

***

### Hilfe anzeigen

```
secedit /?
```

***

### Sicherheitsvorlage importieren

```
secedit /import /cfg [Inf-Datei] /db [Datenbank] [/mergedpolicy] [/areas [Bereiche]] [/log [Logdatei]]
```

**Beispiel:**

```
secedit /import /cfg C:\temp\security.inf /db C:\temp\security.sdb /log C:\temp\import.log
```

| **Option**      | **Bedeutung**                                                                                    |
| --------------- | ------------------------------------------------------------------------------------------------ |
| `/import`       | Importiert eine Sicherheitsvorlage (.inf) in die angegebene Sicherheitsdatenbank (.sdb)          |
| `/cfg`          | Pfad zur Konfigurationsdatei im INF-Format                                                       |
| `/db`           | Pfad zur Sicherheitsdatenbank, in die importiert werden soll (neu oder vorhanden)                |
| `/mergedpolicy` | Fügt die importierten Einstellungen in bestehende Richtlinien ein (anstatt sie zu überschreiben) |
| `/areas [x]`    | Importiert nur bestimmte Bereiche (siehe unten)                                                  |
| `/log`          | Gibt eine Datei zur Protokollierung der Aktion an                                                |

#### Gültige Werte für `/areas`

Mehrere Bereiche können durch Kommata getrennt angegeben werden:

* `SECURITYPOLICY` – Lokale Sicherheitsrichtlinien (z. B. Überwachungsrichtlinie, Benutzerrechte)
* `ACCOUNTPOLICY` – Kennwortrichtlinien und Kontosperrung
* `USER_RIGHTS` – Benutzerrechte-Zuweisungen
* `REGKEYS` – Registrierungseinstellungen
* `FILESTORE` – Dateisystemberechtigungen (NTFS)
* `SERVICES` – Konfiguration von Diensten
* `GROUP_MGMT` – Verwaltung lokaler Gruppenmitgliedschaften

**Beispiel mit Bereichsauswahl:**

```
secedit /import /cfg C:\temp\security.inf /db C:\temp\security.sdb /areas SECURITYPOLICY,REGKEYS /log C:\temp\import.log
```

***

{% hint style="info" %}
Der Import überträgt die Einstellungen lediglich in die angegebene Datenbank. Um diese Richtlinien tatsächlich auf das System anzuwenden, ist anschließend der Befehl `/configure` erforderlich:

```
secedit /configure /db [Datenbank]
```
{% endhint %}

***

{% hint style="info" %}
Weitere Hinweise

* `.inf`-Dateien sind im typischen INI-Format aufgebaut.
* `.sdb` ist die interne Datenbank, in der Richtlinienzustände gespeichert werden.
* Bei Problemen mit der Richtlinienanwendung hilft ein Blick in die Logdateien.
* Kompatibel mit Sicherheitsvorlagen aus der Gruppenrichtlinienverwaltung.
{% endhint %}
