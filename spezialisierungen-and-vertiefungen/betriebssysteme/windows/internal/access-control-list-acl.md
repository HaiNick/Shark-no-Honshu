# Access Control List (ACL)

{% hint style="info" %}
Alle Befehle sollten mit **administrativen Rechten** in der Eingabeaufforderung (cmd.exe), PowerShell oder in Skripten (z. B. .bat) ausgeführt werden.
{% endhint %}

### Grundlagen

Eine ACL (Access Control List) steuert den Zugriff auf Dateien, Verzeichnisse, Registry-Schlüssel, Dienste und andere Objekte im Windows-Dateisystem.\
Sie besteht aus Einträgen (ACEs), die jeweils definieren, **welcher Benutzer oder welche Gruppe** welche Berechtigungen besitzt.

Die gängigsten Werkzeuge zur Verwaltung von ACLs sind:

* `icacls.exe` (ab Windows Vista, empfohlen)
* `cacls.exe` (veraltet)
* `takeown.exe` (Eigentümerübernahme)
* `whoami /priv` (Anzeige von Berechtigungen)
* `Get-Acl` / `Set-Acl` (PowerShell)

***

### Anzeigen von Berechtigungen

```
icacls [Pfad]
```

**Beispiel:**

```
icacls "C:\Daten"
```

Zeigt alle Zugriffsberechtigungen (ACEs) des angegebenen Objekts.

***

### Setzen oder Ersetzen von Berechtigungen

```
icacls [Pfad] /grant[:r] Benutzer:(Rechte)
```

**Beispiel:**

```
icacls "C:\Daten" /grant Benutzer1:(R)
icacls "C:\Daten" /grant:r Benutzer1:(F)
```

**Optionen und Bedeutungen:**

| Option     | Bedeutung                                     |
| ---------- | --------------------------------------------- |
| `/grant`   | Fügt Rechte hinzu                             |
| `/grant:r` | Setzt bestehende Rechte des Benutzers zurück  |
| Rechte     | F (Full), M (Modify), RX (Read+Execute), R, W |

**Rechte im Detail:**

| Kürzel | Bedeutung         |
| ------ | ----------------- |
| F      | Vollzugriff       |
| M      | Ändern            |
| RX     | Lesen + Ausführen |
| R      | Lesen             |
| W      | Schreiben         |

***

### Entfernen von Berechtigungen

```
icacls [Pfad] /remove Benutzer
```

**Beispiel:**

```
icacls "C:\Daten" /remove Benutzer1
```

Entfernt alle expliziten Rechte für den angegebenen Benutzer.

***

### Vererbung deaktivieren

```
icacls [Pfad] /inheritance:r
```

| Option           | Bedeutung                                               |
| ---------------- | ------------------------------------------------------- |
| `/inheritance:e` | Vererbung aktivieren                                    |
| `/inheritance:d` | Vererbte ACEs entfernen, Vererbung beibehalten          |
| `/inheritance:r` | Vererbung deaktivieren (vererbte ACEs bleiben erhalten) |

***

### Eigentümer übernehmen

```
takeown /f [Pfad] [/r] [/d Y]
```

**Beispiel:**

```
takeown /f "C:\Daten" /r /d Y
```

| Option | Bedeutung                         |
| ------ | --------------------------------- |
| `/f`   | Datei oder Ordnerpfad             |
| `/r`   | Rekursiv auf Unterverzeichnisse   |
| `/d Y` | Bestätigung für „Ja“ bei Abfragen |

***

### Setzen des Eigentümers (mit `icacls`)

```
icacls [Pfad] /setowner Benutzer
```

**Beispiel:**

```
icacls "C:\Daten" /setowner "Administratoren"
```

Hinweis: Erfordert oft vorher `takeown`, wenn kein Zugriff besteht.

***

### Rekursive Anwendung

```
icacls [Pfad] /grant Benutzer:(Rechte) /t
```

**Beispiel:**

```
icacls "C:\Daten" /grant "Benutzer1":(M) /t
```

Die Option `/t` wendet die Aktion auf alle Unterordner und Dateien an.

***

### Sichern und Wiederherstellen von Berechtigungen

**Sichern (ACL exportieren):**

```
icacls [Pfad] /save Datei /t
```

**Beispiel:**

```
icacls "C:\Daten" /save C:\backup\acl.txt /t
```

**Wiederherstellen (ACL importieren):**

```
icacls [.] /restore Datei
```

**Beispiel:**

```
icacls . /restore C:\backup\acl.txt
```

***

### Weitere nützliche Befehle

| Befehl                                  | Funktion                                    |
| --------------------------------------- | ------------------------------------------- |
| `icacls [Pfad] /reset`                  | Setzt Berechtigungen auf Standard zurück    |
| `icacls [Pfad] /deny Benutzer:(Rechte)` | Verweigert explizit bestimmte Rechte        |
| `icacls [Pfad] /remove:d Benutzer`      | Entfernt verweigerte (deny) Einträge        |
| `icacls [Pfad] /replace`                | Ersetzt bestimmte Einträge (selten genutzt) |

***

### PowerShell-Äquivalente

**Anzeigen:**

```powershell
Get-Acl "C:\Daten"
```

**Setzen:**

```powershell
$acl = Get-Acl "C:\Daten"
$rule = New-Object System.Security.AccessControl.FileSystemAccessRule("Benutzer1","FullControl","Allow")
$acl.AddAccessRule($rule)
Set-Acl "C:\Daten" $acl
```

***

{% hint style="info" %}
### Hinweise

* ACLs unterscheiden zwischen expliziten und vererbten Einträgen.
* Die explizite Verweigerung (`/deny`) überschreibt eine erlaubte (`/grant`) Berechtigung.
* Bei Verzeichnissen kann mit `/t` rekursiv gearbeitet werden.
* NTFS ist erforderlich – FAT32 unterstützt keine ACLs.
* Eine falsch gesetzte ACL kann zu **Zugriffsverlust** führen. Sichern vor Änderungen empfohlen.
{% endhint %}
