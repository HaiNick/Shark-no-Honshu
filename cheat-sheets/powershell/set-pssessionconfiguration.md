# Set-PSSessionConfiguration

{% hint style="info" %}
Dieser Befehl dient zur Verwaltung von PowerShell-Remoting-Endpunkten. Administrative Rechte sind erforderlich.
{% endhint %}

### Zweck

`Set-PSSessionConfiguration` konfiguriert Eigenschaften einer vorhandenen PowerShell-Sitzungskonfiguration (Session Endpoint), wie z. B. **Zugriffsrechte**, **Ausführungsumgebung**, **Timeouts** oder **Profilskripte**. Dies beeinflusst, wie und unter welchen Bedingungen Remote-Sitzungen über PowerShell-Remoting ausgeführt werden.

***

### Allgemeine Syntax

```powershell
Set-PSSessionConfiguration -Name <Name> [Optionen]
```

***

### Wichtige Parameter

| Parameter                   | Beschreibung                                                  |
| --------------------------- | ------------------------------------------------------------- |
| `-Name`                     | Name der Sitzungskonfiguration (z. B. `Microsoft.PowerShell`) |
| `-SecurityDescriptorSddl`   | Legt den Zugriff per SDDL-Zeichenfolge fest                   |
| `-ShowSecurityDescriptorUI` | Öffnet grafische Oberfläche zur Berechtigungskonfiguration    |
| `-StartupScript`            | Pfad zu einem Skript, das beim Start der Sitzung geladen wird |
| `-SessionType`              | Art der Sitzung (`Default`, `RestrictedRemoteServer`)         |
| `-RunAsCredential`          | Legt einen Benutzer fest, unter dem die Sitzung läuft         |
| `-TranscriptDirectory`      | Verzeichnis zur Protokollierung von Sitzungsaktivitäten       |
| `-EnvironmentVariables`     | Setzt Umgebungsvariablen in der Sitzung                       |
| `-ModulesToImport`          | Module, die automatisch in die Sitzung geladen werden         |
| `-NoServiceRestart`         | Verhindert das automatische Neustarten des WinRM-Dienstes     |
| `-Force`                    | Überschreibt ohne Rückfrage                                   |
| `-Confirm`                  | Fordert Bestätigung zur Ausführung an                         |
| `-WhatIf`                   | Zeigt an, was passieren würde, ohne es auszuführen            |

***

### Beispiele

#### 1. Standard-Endpunkt für bestimmte Benutzer zugänglich machen (per SDDL)

```powershell
Set-PSSessionConfiguration -Name "Microsoft.PowerShell" -SecurityDescriptorSddl "O:NSG:BAD:P(A;;GA;;;S-1-5-21-...)" -Force
```

> Gewährt Vollzugriff (GA) an eine bestimmte SID (z. B. einer Benutzergruppe)

***

#### 2. Sicherheitsberechtigungen grafisch setzen

```powershell
Set-PSSessionConfiguration -Name "Microsoft.PowerShell" -ShowSecurityDescriptorUI
```

> Öffnet die grafische Oberfläche zur Berechtigungskonfiguration des Endpunkts

***

#### 3. Sitzung mit Startskript und Protokollierung konfigurieren

```powershell
Set-PSSessionConfiguration -Name "CustomSession" -StartupScript "C:\ps\init.ps1" -TranscriptDirectory "C:\Logs" -Force
```

> Das Skript wird beim Verbindungsaufbau geladen, Transkripte werden gespeichert

***

#### 4. Benutzerdefinierte Umgebung für Remote-Sitzungen einrichten

```powershell
Set-PSSessionConfiguration -Name "CustomSession" -EnvironmentVariables @{ "Mode" = "Test" } -ModulesToImport "ActiveDirectory" -Force
```

> Richtet die Umgebung beim Start der Sitzung automatisch ein

***

### Hinweise

* Änderungen gelten **systemweit** für alle Remote-Sitzungen, die über den konfigurierten Endpunkt hergestellt werden.
* **Remote-Zugriffe werden über WinRM** abgewickelt – dieser Dienst muss aktiv sein.
* Die Änderung von Berechtigungen kann **ausgenutzt oder eingeschränkt** werden – z. B. für **Remote-Persistenz**, **Privilegieneskalation** oder Hardening.
* Überprüfen der Konfiguration:

```powershell
Get-PSSessionConfiguration
```

* Entfernen einer benutzerdefinierten Konfiguration:

```powershell
Unregister-PSSessionConfiguration -Name "CustomSession"
```

***

### Sicherheitsrelevanz

| Szenario             | Bedeutung                                                                    |
| -------------------- | ---------------------------------------------------------------------------- |
| Admin-Backdoor       | Ein Startskript kann als Persistenzmechanismus missbraucht werden            |
| Hardening            | Reduzierte Sessiontypen (`RestrictedRemoteServer`) verringern Angriffsfläche |
| Compliance / Logging | Transkriptverzeichnis unterstützt Nachvollziehbarkeit und Überwachung        |
