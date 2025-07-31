# powershell remoting — remote shell the windows way

interactive or scripted management of remote windows systems via WinRM protocol. requires admin rights.

## commands overview

| command | purpose |
|---------|---------|
| `Enable-PSRemoting` | enable PS remoting on local machine |
| `New-PSSession` | create persistent session to remote computer |
| `Enter-PSSession` | start interactive live session |
| `Invoke-Command` | run commands/scripts on remote systems |
| `Remove-PSSession` | terminate existing remoting sessions |

## enable remoting

```powershell
Enable-PSRemoting -Force -SkipNetworkProfileCheck
```

this will:
* start and configure WinRM service
* create WSMan listeners  
* open TCP port 5985 (HTTP) + firewall exceptions

## create non-interactive sessions

```powershell

```powershell
New-PSSession -ComputerName <Ziel> [-Credential <Benutzer>] [-UseSSL] [-Port <Nummer>] [-Authentication <Typ>]
```

#### Beispiel:

```powershell
$session = New-PSSession -ComputerName "Server01" -Credential (Get-Credential)
```

***

### 3. Interaktive Sitzung starten

```powershell
Enter-PSSession -Session <Sitzung>  
# oder direkt:
Enter-PSSession -ComputerName <Ziel> -Credential <Benutzer>
```

#### Beispiel:

```powershell
Enter-PSSession -ComputerName "Server01" -Credential (Get-Credential)
```

> Startet eine interaktive Remote-Konsole (Prompt ändert sich)

***

### 4. Befehle remote ausführen

```powershell
Invoke-Command -Session <Sitzung> -ScriptBlock { <Befehl> }

# oder mit mehreren Computern:
Invoke-Command -ComputerName <Ziel1,Ziel2,...> -ScriptBlock { <Befehl> }
```

#### Beispiel:

```powershell
Invoke-Command -Session $session -ScriptBlock { Get-Service }
```

***

### 5. Sitzung beenden

```powershell
Remove-PSSession -Session <Sitzung>
```

#### Beispiel:

```powershell
Remove-PSSession -Session $session
```

***

### Erweiterte Parameter

| Parameter            | Bedeutung                                                                              |
| -------------------- | -------------------------------------------------------------------------------------- |
| `-UseSSL`            | Aktiviert HTTPS (Port 5986, Listener muss vorhanden sein)                              |
| `-Credential`        | Authentifizierung mit Benutzer/Passwort                                                |
| `-Authentication`    | Authentifizierungsart (`Default`, `Kerberos`, `Credssp`, `Basic`, `Negotiate`)         |
| `-ConfigurationName` | Sitzungskonfiguration (z. B. `Microsoft.PowerShell`, benutzerdefinierte Konfiguration) |
| `-Port`              | Portnummer (Standard: 5985 für HTTP, 5986 für HTTPS)                                   |

***

### Sicherheitshinweise

| Maßnahme / Bedrohung             | Bedeutung                                                                                |
| -------------------------------- | ---------------------------------------------------------------------------------------- |
| Zugriffskontrolle via ACL / SDDL | Nur autorisierte Benutzer dürfen Endpunkte verwenden                                     |
| Persistenz über StartupScripts   | Endpunkte können mit PowerShell-Backdoors versehen werden (`Set-PSSessionConfiguration`) |
| Logging                          | Mit Transkription (`TranscriptDirectory`) oder Eventlogs erweiterbar                     |
| HTTPS verwenden                  | Empfohlen für produktive Umgebungen (z. B. `winrm create` mit Zertifikat)                |
| Firewall prüfen                  | WinRM muss über Firewall erreichbar sein (Port 5985/5986)                                |

***

### Weitere Werkzeuge

* **`Set-PSSessionConfiguration` :** [set-pssessionconfiguration.md](set-pssessionconfiguration.md "mention")
* **`Get-PSSession`**: Auflisten aktiver Remoting-Sitzungen
* **`Disconnect-PSSession` / `Connect-PSSession`**: Temporäre Trennung und Wiederaufnahme
* **`Register-PSSessionConfiguration`**: Neue Sitzungskonfigurationen registrieren
* **`Test-WsMan`**: Erreichbarkeit und Konfiguration eines Zielsystems prüfen

***

### Beispielablauf (komplett)

```powershell
Enable-PSRemoting -Force

$cred = Get-Credential
$session = New-PSSession -ComputerName "srv01" -Credential $cred

Invoke-Command -Session $session -ScriptBlock { hostname; whoami }

Enter-PSSession -Session $session

# Nach Abschluss:
Remove-PSSession -Session $session
```
