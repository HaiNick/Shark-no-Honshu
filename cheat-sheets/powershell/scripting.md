# Scripting

PowerShell ist eine objektorientierte Shell und Scriptsprache, die sich besonders für Systemadministration, Automatisierung und Konfigurationsaufgaben eignet. Dieser Artikel gibt einen kompakten Überblick über wichtige Konzepte im PowerShell-Scripting.

***

## Setzen von Variablen

```powershell
# String
$text = "Hallo Welt"

# Zahl (Integer)
$zahl = 42

# Boolean
$istWahr = $true

# Array
$array = @(1, 2, 3, 4)

# HashTable (assoziatives Array)
$hash = @{ Name = "Max"; Alter = 30 }
```

***

### Variable abrufen

```powershell
Write-Output $text
```

Oder einfach:

```powershell
$text
```

***

### Variablen in Strings und URLs verwenden

Beim Zusammensetzen von Strings mit Variablen – insbesondere bei URLs mit Ports oder Pfaden – muss PowerShell-spezifische Syntax beachtet werden, um Fehler zu vermeiden.

**❌ Problematisches Beispiel:**

```powershell
curl $IP:6666/modules  # Falsch: PowerShell interpretiert das als arithmetischen Ausdruck
```

**✅ Korrekte Varianten:**

```powershell
# Variante 1: Direkte Interpolation mit doppelten Anführungszeichen
curl "$IP:6666/modules"

# Variante 2: Geschweifte Klammern (empfohlen für komplexe Fälle)
curl "${IP}:6666/modules"

# Variante 3: Stringverkettung
$url = "$IP" + ":6666/modules"
curl $url
```

**Alternative: Native HTTP-Funktion**

Für API-Aufrufe ist `Invoke-RestMethod` in PowerShell oft die bessere Wahl:

```powershell
Invoke-RestMethod -Uri "http://${IP}:6666/modules"
```

Diese Methode erlaubt auch direkt die Verarbeitung von JSON- oder XML-Antworten.

***

## Bedingungen und Schleifen

```powershell
if ($zahl -gt 10) {
    Write-Output "Größer als 10"
} elseif ($zahl -eq 10) {
    Write-Output "Gleich 10"
} else {
    Write-Output "Kleiner als 10"
}
```

```powershell
foreach ($item in $array) {
    Write-Output $item
}
```

***

## Funktionen

```powershell
function SagHallo($name) {
    return "Hallo, $name!"
}
```

***

## Pipeline-Nutzung

PowerShell arbeitet objektorientiert – in Pipelines werden keine reinen Textströme, sondern **Objekte** weitergegeben:

```powershell
Get-Process | Where-Object { $_.CPU -gt 100 }
```

***

## Nützliche Cmdlets

| Cmdlet                | Beschreibung                                   |
| --------------------- | ---------------------------------------------- |
| `Get-Help`            | Zeigt Hilfe zu Cmdlets an                      |
| `Get-Command`         | Listet verfügbare Befehle auf                  |
| `Get-Member`          | Zeigt Eigenschaften eines Objekts              |
| `Set-ExecutionPolicy` | Legt fest, ob Skripte ausgeführt werden dürfen |
| `Import-Module`       | Lädt Module zur Erweiterung                    |

***

## Arbeiten mit Dateien

```powershell
# Datei lesen
$inhalt = Get-Content "C:\datei.txt"

# Datei schreiben
Set-Content -Path "C:\neu.txt" -Value "Textinhalt"

# Inhalt anhängen
Add-Content -Path "C:\neu.txt" -Value "Weitere Zeile"
```

***

## Fehlerbehandlung

```powershell
try {
    Get-Item "C:\nichtvorhanden.txt"
}
catch {
    Write-Output "Datei nicht gefunden."
}
finally {
    Write-Output "Fertig."
}
```

***

## Remote und Automatisierung

```powershell
# Befehl auf entfernten Rechner ausführen
Invoke-Command -ComputerName SERVER1 -ScriptBlock { Get-Service }

# Remotesitzung starten
Enter-PSSession -ComputerName SERVER1
```

### Parameter (Invoke-Command)

Invoke-Command \<parameter>

**-FilePath** c:\scripts\test.ps1          Datei zum Ausführen

**-ComputerName** Server01            Remote-System Name

**-Credential** Domain01\User01

**-ScriptBlock {** _Get-ChildItem_ **}**     Befehle auf Remote-System

***

## Skriptausführung und Sicherheit

Standardmäßig verhindert PowerShell das Ausführen unsignierter Skripte. Gängige Einstellung für eigene Skripte:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

***

## Modulnutzung und Wiederverwendbarkeit

* Funktionen können in `.psm1`-Dateien ausgelagert werden.
* Solche Module lassen sich mit `Import-Module` laden.
* Mit Hilfe-Kommentaren (`<##>`) lassen sich Skripte dokumentieren.

Beispiel:

```powershell
<#
.SYNOPSIS
  Gibt eine personalisierte Begrüßung aus.
#>
function SagHallo($name) {
    "Hallo, $name!"
}
```
