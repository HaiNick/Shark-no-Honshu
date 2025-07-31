# Windows

<details>

<summary>Links:</summary>

[https://www.howtogeek.com/405806/windows-task-manager-the-complete-guide/](https://www.howtogeek.com/405806/windows-task-manager-the-complete-guide/)

</details>

***

### Windows-Editionen

Microsoft Windows ist in verschiedenen Editionen erhältlich, die sich in Funktionalität und Zielgruppe unterscheiden:

* **Home**: Für Privatanwender mit grundlegenden Funktionen.
* **Pro**: Erweitert um Funktionen für kleine Unternehmen, wie BitLocker und Gruppenrichtlinien.
* **Enterprise**: Für große Unternehmen mit erweiterten Sicherheits- und Verwaltungsfunktionen.
* **Education**: Für Bildungseinrichtungen mit speziellen Lizenzierungsoptionen.

Die Auswahl der Edition hängt von den spezifischen Anforderungen und dem Einsatzgebiet ab. ([Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/get-started/editions-comparison?utm_source=chatgpt.com))

***

### Die Desktop-Benutzeroberfläche (GUI)

Die grafische Benutzeroberfläche (GUI) von Windows bietet eine visuelle Interaktion mit dem Betriebssystem:

* **Desktop**: Arbeitsfläche für Dateien, Ordner und Verknüpfungen.
* **Taskleiste**: Zeigt geöffnete Anwendungen und ermöglicht den schnellen Zugriff auf Funktionen.
* **Startmenü**: Zentraler Zugriffspunkt für Programme, Einstellungen und Suchfunktionen.

Die GUI kann über **Einstellungen > Personalisierung** angepasst werden.

***

### Das Dateisystem

Windows verwendet verschiedene Dateisysteme zur Organisation und Speicherung von Daten:([Lifewire](https://www.lifewire.com/what-is-a-file-system-8732073?utm_source=chatgpt.com))

* **NTFS (New Technology File System)**: Standard-Dateisystem mit Unterstützung für Berechtigungen, Verschlüsselung und große Dateien.
* **FAT32**: Älteres Dateisystem mit breiter Kompatibilität, jedoch Einschränkungen bei Dateigrößen.
* **exFAT**: Optimiert für Flash-Speicher mit Unterstützung für große Dateien.

Die Auswahl des Dateisystems erfolgt in der Regel bei der Formatierung eines Laufwerks.

***

### Der Ordner Windows\System32

Der Ordner **C:\Windows\System32** ist ein zentraler Bestandteil des Windows-Betriebssystems:([Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/windows-application-ui-development?utm_source=chatgpt.com), [Microsoft Antworten](https://answers.microsoft.com/en-us/windows/forum/all/system-32-folder/34472898-715f-4651-9b6e-141b4a714046?utm_source=chatgpt.com), [Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/get-started/editions-comparison?utm_source=chatgpt.com))

* Enthält essentielle Systemdateien, Treiber und ausführbare Programme.
* Beinhaltet wichtige Tools wie die Eingabeaufforderung (cmd.exe) und den Task-Manager.
* Veränderungen oder Löschungen in diesem Ordner können zu Systeminstabilitäten führen.

***

### Benutzerkonten, Profile und Berechtigungen

Windows ermöglicht die Verwaltung mehrerer Benutzerkonten mit individuellen Einstellungen:([Microsoft Support](https://support.microsoft.com/en-us/windows/manage-user-accounts-in-windows-104dc19f-6430-4b49-6a2b-e4dbd1dcdf32?utm_source=chatgpt.com))

* **Benutzerkonten**: Können als Standardbenutzer oder Administratoren eingerichtet werden.
* **Profile**: Speichern persönliche Einstellungen, Dokumente und Anwendungsdaten.
* **Berechtigungen**: Steuern den Zugriff auf Dateien und Ordner über das Kontextmenü unter "Eigenschaften > Sicherheit".([Microsoft Learn](https://learn.microsoft.com/en-us/answers/questions/1389054/how-do-i-set-up-user-accounts-and-manage-permissio?utm_source=chatgpt.com))

Die Verwaltung erfolgt über **Einstellungen > Konten**.

***

### Benutzerkontensteuerung (UAC)

Die Benutzerkontensteuerung (User Account Control, UAC) schützt das System vor unautorisierten Änderungen:

* Fordert bei administrativen Aktionen eine Bestätigung oder Administratoranmeldedaten an.
* Verhindert, dass schädliche Software ohne Zustimmung Änderungen vornimmt.
* Kann in den **Einstellungen > System > Sicherheit** angepasst werden.

***

### Einstellungen und Systemsteuerung

Windows bietet zwei Hauptschnittstellen zur Systemkonfiguration:

* **Einstellungen**: Moderne Oberfläche für die meisten Konfigurationsoptionen, erreichbar über das Startmenü.
* **Systemsteuerung**: Klassische Oberfläche mit erweiterten Einstellungen, erreichbar über "control" im Ausführen-Dialog (Win + R).

Beide bieten Zugriff auf System-, Hardware- und Benutzerkontoeinstellungen.

***

### Task-Manager

Der Task-Manager bietet Echtzeitinformationen über Systemleistung und laufende Prozesse:

* **Aufruf**: Strg + Umschalt + Esc oder Rechtsklick auf die Taskleiste.
* **Funktionen**:
  * Überwachung von CPU-, Speicher- und Netzwerkauslastung.
  * Beenden nicht reagierender Anwendungen.
  * Verwaltung von Autostart-Programmen.

Ein unverzichtbares Tool für die Systemdiagnose und -verwaltung. ([How-To Geek](https://www.howtogeek.com/405806/windows-task-manager-the-complete-guide/?utm_source=chatgpt.com))

***

### Nutzerkonten

Jeder Benutzer mit administrativen Rechten ist Teil der Gruppe Administratoren. Standardbenutzer sind Teil der Gruppe Benutzer.

* **Administrator**: Höchste Berechtigung, kann jede Systemkonfiguration bearbeiten. **Vollzugriff**
* **Standard User:** Eingeschränkter Zugriff. Meist keine Möglichkeit um permantente Änderungen/ wichtige Änderungen durchzuführen. **Limitiert auf eigene Dateien**.
* **SYSTEM**/**LocalSystem**: Von OS für interne Aufgaben verwendet. Vollzugriff auf alle Dateien/Ressourcen. **Höhere Privilegien als Administrator**.
* **Local Service**: Standardkonto, das zur **Ausführung von Windows-Diensten mit "minimalen" Rechten** verwendet wird. Es werden anonyme Verbindungen über das Netzwerk verwendet.
* **Network Service**: Standardkonto, das zur **Ausführung von Windows-Diensten mit "minimalen" Rechten** verwendet wird. Es **verwendet** die **Anmeldeinformationen des Computers**, um sich über das **Netzwerk zu authentifizieren**.

### Sitelinks

<details>

<summary>Sitelinks</summary>

* [powershell.md](../../cheat-sheets/powershell.md "mention")
* &#x20;[windows-cmd.md](../../cheat-sheets/windows-cmd.md "mention")
* [windows-privesc.md](../../killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/windows-privesc.md "mention")

</details>

### Schwachstellen

<details>

<summary>Schwachstellen</summary>

* [https://learn.microsoft.com/de-de/security-updates/securitybulletins/securitybulletins](https://learn.microsoft.com/de-de/security-updates/securitybulletins/securitybulletins)

</details>
