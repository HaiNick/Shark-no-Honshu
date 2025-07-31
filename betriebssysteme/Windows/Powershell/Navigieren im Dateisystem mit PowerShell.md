PowerShell bietet eine Reihe von Cmdlets zum Navigieren im Dateisystem und zur Verwaltung von Dateien, von denen viele Entsprechungen im traditionellen Windows-Befehlszeilen-Interface haben.

### Get-ChildItem

Ähnlich dem Befehl **dir** in der Eingabeaufforderung (oder **ls** in Unix-ähnlichen Systemen) listet **Get-ChildItem** die Dateien und Verzeichnisse an einem angegebenen Ort auf, der mit dem Parameter **-Path** spezifiziert wird. Dieses Cmdlet kann verwendet werden, um Verzeichnisse zu erkunden und deren Inhalte anzuzeigen.

Wenn kein **-Path** angegeben wird, zeigt das Cmdlet den Inhalt des aktuellen Arbeitsverzeichnisses an.

### Beispiele

Um alle Dateien und Ordner im aktuellen Verzeichnis anzuzeigen, verwenden Sie:
```powershell
Get-ChildItem
```
Um die Dateien und Ordner in einem spezifischen Verzeichnis anzuzeigen, geben Sie den Pfad an:
```powershell
Get-ChildItem -Path "C:\Users\Benutzername\Documents"
```
### Zusätzliche Optionen

**Get-ChildItem** bietet eine Vielzahl von Optionen und Parametern, um die Ausgabe zu filtern oder anzupassen, wie z. B.:

- **-Recurse**: Listet alle Unterverzeichnisse und deren Inhalte auf.
- **-Filter**: Ermöglicht das Filtern von Ergebnissen basierend auf bestimmten Kriterien, z. B. Dateitypen.

### Beispiel für rekursive Anzeige

Um alle Dateien und Ordner in einem Verzeichnis und dessen Unterverzeichnissen anzuzeigen, verwenden Sie:
```powershell
Get-ChildItem -Path "C:\Users\Benutzername\Documents" -Recurse
```
## Erstellen von Dateien und Verzeichnissen mit PowerShell

Im Gegensatz zur traditionellen Windows-Eingabeaufforderung, die separate Befehle zum Erstellen und Verwalten verschiedener Elemente wie Verzeichnisse und Dateien verwendet, vereinfacht PowerShell diesen Prozess, indem es eine einheitliche Sammlung von Cmdlets zur Handhabung der Erstellung und Verwaltung sowohl von Dateien als auch von Verzeichnissen bereitstellt.

### New-Item

Um ein Element in PowerShell zu erstellen, verwenden wir das Cmdlet **New-Item**. Wir müssen den Pfad des Elements und dessen Typ angeben (ob es sich um eine Datei oder ein Verzeichnis handelt).

### Verwendung

Hier sind einige grundlegende Beispiele zur Verwendung von **New-Item**:
#### Erstellen eines neuen Verzeichnisses

Um ein neues Verzeichnis zu erstellen, verwenden Sie den Parameter **-ItemType** und setzen diesen auf **Directory**:
```powershell
New-Item -Path "C:\Users\Benutzername\Documents\NeuesVerzeichnis" -ItemType Directory
```
#### Erstellen einer neuen Datei

Um eine neue Datei zu erstellen, setzen Sie den **-ItemType** Parameter auf **File**:
```powershell
New-Item -Path "C:\Users\Benutzername\Documents\NeueDatei.txt" -ItemType File
```
### Zusätzliche Optionen

**New-Item** unterstützt auch die Erstellung anderer Elementtypen, wie z. B.:

- **SymbolicLink**: Erstellt einen symbolischen Link zu einer Datei oder einem Verzeichnis.
- **HardLink**: Erstellt einen Hardlink zu einer Datei.

### Beispiel für einen symbolischen Link

Um einen symbolischen Link zu erstellen, verwenden Sie den folgenden Befehl:
```powershell
New-Item -Path "C:\Benutzer\Benutzername\Documents\LinkZuNeuerDatei.txt" -ItemType SymbolicLink -Target "C:\Users\Benutzername\Documents\NeueDatei.txt"
```
## Navigieren zwischen Verzeichnissen mit PowerShell

Um in PowerShell zu einem anderen Verzeichnis zu navigieren, verwenden wir das Cmdlet **Set-Location**. Dieses Cmdlet ändert das aktuelle Verzeichnis und bringt uns zum angegebenen Pfad, ähnlich dem **cd** Befehl in der Eingabeaufforderung.

### Verwendung von Set-Location

#### Syntax

Die grundlegende Syntax des **Set-Location** Cmdlets ist wie folgt:
```powershell
Set-Location -Path "Pfad\zum\Verzeichnis"
```
### Beispiele

#### Wechseln zu einem Verzeichnis

Um zu einem bestimmten Verzeichnis zu wechseln, geben Sie einfach den Pfad des gewünschten Verzeichnisses an:
```powershell
Set-Location -Path "C:\Users\Benutzername\Documents"
```
#### Wechseln zum Stammverzeichnis

Um zum Stammverzeichnis eines Laufwerks zu wechseln, verwenden Sie den entsprechenden Laufwerksbuchstaben:
```powershell
Set-Location -Path "C:\"
```
#### Verwenden von Alias

PowerShell unterstützt auch Aliase, um häufige Befehle abzukürzen. Für **Set-Location** ist der Alias **sl** verfügbar:
```powershell
sl "C:\Users\Benutzername\Documents"
```
### Überprüfen des aktuellen Verzeichnisses

Um das aktuelle Verzeichnis zu überprüfen, können Sie das Cmdlet **Get-Location** verwenden:
```powershell
Get-Location
```