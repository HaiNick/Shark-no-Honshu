# Windows-Registry

Die Windows-Registry ist eine zentrale, hierarchisch aufgebaute Datenbank, die von Windows zur Speicherung von Konfigurationsdaten verwendet wird. Hier werden Einstellungen für das Betriebssystem selbst, für Benutzerkonten, Geräte, Treiber, installierte Software und viele Systemdienste verwaltet.

Sie ist unverzichtbar für die Funktion des Systems und wird von fast allen Windows-Komponenten genutzt.

***

### Struktur

Die Registry ist vergleichbar mit einem Dateisystem:

* **Hives** sind die Hauptabschnitte (sozusagen die Laufwerke)
* **Keys (Schlüssel)** sind wie Ordner
* **Values (Werte)** sind die eigentlichen Einträge
* **Data (Daten)** ist der Inhalt der Werte

Ein Registry-Pfad sieht aus wie:

```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
```

***

### Hauptbereiche (Hives)

| Kurzform | Langform              | Zweck                                                      |
| -------- | --------------------- | ---------------------------------------------------------- |
| `HKLM`   | HKEY\_LOCAL\_MACHINE  | Einstellungen, die für alle Benutzer gelten                |
| `HKCU`   | HKEY\_CURRENT\_USER   | Einstellungen des aktuell angemeldeten Benutzers           |
| `HKCR`   | HKEY\_CLASSES\_ROOT   | Dateitypen, Shell-Verhalten, COM-Objekte                   |
| `HKU`    | HKEY\_USERS           | Alle Benutzerprofile (auch SYSTEM, .DEFAULT usw.)          |
| `HKCC`   | HKEY\_CURRENT\_CONFIG | Aktuelle Hardwarekonfiguration (nur temporär zur Laufzeit) |

> HKCU verweist intern auf einen Unterschlüssel von HKU (nämlich den des angemeldeten Benutzers). Gleiches gilt für HKCR, das aus HKLM\Software\Classes und HKCU\Software\Classes zusammengeführt wird.

***

### Datentypen

| Typ             | Beschreibung                                       |
| --------------- | -------------------------------------------------- |
| `REG_SZ`        | Zeichenfolge                                       |
| `REG_DWORD`     | 32-Bit-Ganzzahl                                    |
| `REG_QWORD`     | 64-Bit-Ganzzahl                                    |
| `REG_BINARY`    | Rohdaten in Binärform                              |
| `REG_MULTI_SZ`  | Mehrzeilige Zeichenfolge (z. B. Liste von Strings) |
| `REG_EXPAND_SZ` | Zeichenfolge mit Variablen wie `%SystemRoot%`      |

***

### Beispiel: Registry-Wert

```plaintext
Pfad:  HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
Name:  LocalAccountTokenFilterPolicy
Typ:   REG_DWORD
Daten: 1
```

Dieser Eintrag aktiviert oder deaktiviert den Token-Filter für lokale Konten bei Remotezugriffen (wird z. B. für WMI-Verbindungen oft benötigt).

***

### Speicherorte der Registry-Dateien

Die Registry besteht intern aus mehreren Dateien, die auf der Festplatte gespeichert sind – sogenannte "Hives".

| Hive                  | Datei      | Speicherort                  |
| --------------------- | ---------- | ---------------------------- |
| HKLM\SAM              | SAM        | C:\Windows\System32\config\\ |
| HKLM\SYSTEM           | SYSTEM     | C:\Windows\System32\config\\ |
| HKLM\SECURITY         | SECURITY   | C:\Windows\System32\config\\ |
| HKLM\SOFTWARE         | SOFTWARE   | C:\Windows\System32\config\\ |
| HKCU (Benutzerprofil) | NTUSER.DAT | C:\Users\[Benutzername]\\    |
| HKU.DEFAULT           | DEFAULT    | C:\Windows\System32\config\\ |

Diese Dateien können nicht direkt mit einem Texteditor geöffnet werden. Sie werden exklusiv vom System verwendet.

***

### Zugriffsmöglichkeiten

* **GUI:** `regedit.exe`
* **Kommandozeile:** `reg.exe` (siehe [reg.md](../befehle/reg.md "mention"))
* **PowerShell:** Zugriff über Cmdlets wie `Get-ItemProperty`, `Set-ItemProperty` usw.
* **Skripte:** `.bat`, `.ps1` oder GPO-Skripte

***

### Sicherheit

Der Zugriff auf viele Bereiche der Registry ist geschützt. Schreibzugriff auf z. B. `HKLM\SYSTEM` erfordert Administratorrechte. Es gibt außerdem ACLs (Zugriffslisten) für jeden Schlüssel. Auch Dienste wie der Gruppenrichtlinien-Client, WMI oder Antivirenprogramme greifen regelmäßig auf die Registry zu.

***

### Beispiele für häufige Einträge

#### Autostart:

```plaintext
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
"MeineApp" = "C:\Tools\app.exe"
```

#### UAC deaktivieren:

```cmd
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v EnableLUA /t REG_DWORD /d 0 /f
```

#### Netzlaufwerk beim Start einbinden:

```plaintext
HKCU\Network\Z
"RemotePath" = "\\server\freigabe"
"UserName"   = "DOMAIN\User"
```

***

{% hint style="info" %}
### Zusammenfassung

* Die Registry ist das zentrale Konfigurationssystem unter Windows.
* Sie besteht aus mehreren Teilstrukturen ("Hives"), die physisch als Dateien existieren.
* Der Zugriff erfolgt meist per `regedit`, `reg.exe`, PowerShell oder über GPOs.
* Fehlerhafte Änderungen können das System beschädigen, daher nur gezielt und mit Backup eingreifen.
{% endhint %}



### Zum Herausfinden und Tracken welcher Key zu was gehört, kann man folgendes machen:

1. **Offizielle Microsoft-Dokumentation prüfen**\
   Teilweise dokumentierte Registry-Keys – vor allem zu Gruppenrichtlinien und Systemkonfiguration:
   * **Policy CSP (für MDM/GPO-Registry-Keys):**\
     [https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-configuration-service-provider](https://learn.microsoft.com/en-us/windows/client-management/mdm/policy-configuration-service-provider)
   * **Gruppenrichtlinien-Referenz (Excel-Liste):**\
     [https://www.microsoft.com/en-us/download/details.aspx?id=104003](https://www.microsoft.com/en-us/download/details.aspx?id=104003)
   * **Registry Reference für Entwickler:**\
     [https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry](https://learn.microsoft.com/en-us/windows/win32/sysinfo/registry)

***

2. **Registry-Zugriffe live überwachen (Monitoring)**\
   Überwachen, welche Schlüssel beim Starten von Programmen oder Ändern von Einstellungen angefasst werden:
   * **Process Monitor (Procmon)** – Microsoft Sysinternals:\
     [https://learn.microsoft.com/en-us/sysinternals/downloads/procmon](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon)
   * **Filter setzen:** `Operation is RegSetValue` o. ä.

***

3. **Gruppenrichtlinien testen und Registry abgleichen**
   * Gruppenrichtlinien-Editor öffnen:\
     `gpedit.msc`
   * Dann Registry prüfen:\
     z. B. mit `regedit` oder `reg query`
   * Erweiterte Auswertung mit:\
     **LGPO.exe (Microsoft Local Group Policy Tool)**\
     [https://www.microsoft.com/en-us/download/details.aspx?id=55319](https://www.microsoft.com/en-us/download/details.aspx?id=55319)

***

4. **Vorher-/Nachher-Vergleich (Snapshot-Tools)**\
   Änderungen durch Programme oder Systemaktionen direkt vergleichen:
   * **Regshot (Open Source):**\
     [https://sourceforge.net/projects/regshot/](https://sourceforge.net/projects/regshot/)
   * **RegFromApp (NirSoft):**\
     [https://www.nirsoft.net/utils/reg\_from\_app.html](https://www.nirsoft.net/utils/reg_from_app.html)
   * **Total Uninstall (kommerziell):**\
     [https://www.martau.com/](https://www.martau.com/)

***

5. **Community-Wissen und Registry-Sammlungen nutzen**\
   Viele undocumented Keys oder praktische Tweaks wurden von der Community dokumentiert:
   * **Winaero Tweaker Blog:**\
     [https://winaero.com/blog/](https://winaero.com/blog/)
   * **SuperUser (Registry-Tag):**\
     [https://superuser.com/questions/tagged/registry](https://superuser.com/questions/tagged/registry)
   * **TenForums (Tutorial-Sektion):**\
     [https://www.tenforums.com/tutorials/](https://www.tenforums.com/tutorials/)
   * **r/Windows10TechSupport (Reddit):**\
     [https://www.reddit.com/r/Windows10TechSupport/](https://www.reddit.com/r/Windows10TechSupport/)
   * **GitHub: Windows Tweaks (Beispielrepo):**\
     [https://github.com/ChrisTitusTech/win10script](https://github.com/ChrisTitusTech/win10script)
