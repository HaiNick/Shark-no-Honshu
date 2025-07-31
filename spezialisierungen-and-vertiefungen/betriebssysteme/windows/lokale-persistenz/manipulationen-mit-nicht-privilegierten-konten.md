# Manipulationen durch nicht privilegierte Benutzerkonten

## Privilegienerweiterung durch Gruppenmitgliedschaft (Windows)

### **1. Direktes Hinzufügen zur Administratorgruppe**

```cmd
net localgroup administrators <Benutzername> /add
```

{% hint style="danger" %}
Achtung: Sehr auffällig – kann durch Monitoring/EDR leicht erkannt werden.
{% endhint %}

* Verleiht vollständige Administratorrechte.
* Ermöglicht Zugriff über RDP, WinRM oder andere Remote-Verwaltungsdienste.

### **2. Alternativ: Backup Operators (unauffälliger)**

```cmd
net localgroup "Backup Operators" <Benutzername> /add
```

Keine Adminrechte, aber vollständiger Zugriff auf Dateien und Registry ( DACL[stichwortverzeichnis.md](../../../../stichwortverzeichnis.md "mention")  wird ignoriert).\
Ermöglicht Kopieren der SAM- und SYSTEM-Hives → Passwort-Hashes extrahieren → Privilegieneskalation möglich.

### **3. Remote-Zugriff ermöglichen (WinRM)**

```cmd
net localgroup "Remote Management Users" <Benutzername> /add
```

Erforderlich, um mit einem nicht privilegierten Konto per WinRM auf das System zuzugreifen.



### 4. Lokale Tokenfilter-Richtlinie deaktivieren (UAC-Bypass für Netzwerkzugriff)

```cmd
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /t REG_DWORD /v LocalAccountTokenFilterPolicy /d 1
```

Erlaubt, dass lokale Benutzer mit Administratorrechten auch bei Remote-Zugriff (z. B. WinRM oder WMI) ihre vollen Rechte behalten.\
Standardmäßig wird der Token durch UAC gefiltert → kein administrativer Zugriff trotz Mitgliedschaft in der Administratorgruppe.\
Durch Setzen dieses Registry-Keys auf `1` wird der Filter deaktiviert.\
Notwendig, um administrativen Zugriff aus der Ferne wiederherzustellen.



## Spezielle Rechte und Sicherheitsdeskriptoren

[https://docs.microsoft.com/en-us/windows/win32/secauthz/privilege-constants](https://docs.microsoft.com/en-us/windows/win32/secauthz/privilege-constants)

Ein ähnlicher Effekt wie das Hinzufügen eines Benutzers zur Gruppe „Backup Operators“ kann erzielt werden, **ohne** die Gruppenmitgliedschaft zu ändern. Spezielle Gruppen sind nur deshalb „speziell“, weil das Betriebssystem ihnen standardmäßig bestimmte **Privilegien** zuweist. Privilegien sind einfach die **Berechtigungen**, bestimmte Aufgaben im System durchzuführen. Diese reichen von einfachen Aktionen wie dem Herunterfahren des Systems bis hin zu sehr weitreichenden Rechten wie der **Übernahme des Besitzes von beliebigen Dateien**.

Eine vollständige Liste verfügbarer Privilegien [hier (Microsoft Docs)](https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/user-rights-assignment).

Die Gruppe **Sicherungs-Operatoren (Backup Operators)** hat standardmäßig folgende zwei Privilegien:

* **SeBackupPrivilege**: Der Benutzer kann _jede Datei_ im System lesen, _unabhängig von vorhandenen DACLs (Discretionary_ [access-control-list-acl.md](../internal/access-control-list-acl.md "mention")_)_.
* **SeRestorePrivilege**: Der Benutzer kann _jede Datei_ im System schreiben, ebenfalls _unabhängig von DACLs_.

#### Benutzer mit Privilegien ausstatten

Wir können diese Privilegien **jedem Benutzer zuweisen**, unabhängig von Gruppenmitgliedschaften. Das funktioniert über den Befehl [secedit.md](../befehle/secedit.md "mention").

1. Exportieren der aktuellen Sicherheitskonfiguration in eine INF-Datei:

```bash
secedit /export /cfg config.inf
```

2. Öffnen die Datei `config.inf` und **hinuzfügen des Benutzer** zu den Zeilen mit `SeBackupPrivilege` und `SeRestorePrivilege`:

```ini
SeBackupPrivilege = <bereits vorhandene Benutzer>, <NEUER_BENUTZER>
SeRestorePrivilege = <bereits vorhandene Benutzer>, <NEUER_BENUTZER>
```

3. Import der Datei zurück ins System:

```bash
secedit /import /cfg config.inf /db config.sdb
```

4. Anwenden der Konfiguration:

```bash
secedit /configure /db config.sdb /cfg config.inf
```

Jetzt hat der Benutzer dieselben Rechte wie ein Sicherungs-Operator – **ohne Mitglied der Gruppe zu sein**.

***

#### Zugriff auf WinRM gewähren (ohne Gruppenmitgliedschaft)

Der Benutzer hat jetzt die benötigten Rechte, kann sich aber **noch nicht per WinRM anmelden**. Statt ihn zur Gruppe „Remote Management Users“ hinzuzufügen, ändert man den **Sicherheitsdeskriptor des WinRM-Dienstes**, um dem Benutzer Zugriff zu geben. Ein Sicherheitsdeskriptor ist quasi eine **ACL für Systemdienste**.

1. Ausführen dieses Befehls **in einer PowerShell-Session mit GUI**:

```powershell
Set-PSSessionConfiguration -Name Microsoft.PowerShell -ShowSecurityDescriptorUI
```

2. Es öffnet sich ein Fenster, in dem der Benutzer (z. B. `low_priv_user`) hinzufügt und ihm dadurch **volle Berechtigungen für WinRM** gegeben wird.

***

#### Lokale Richtlinie anpassen

Damit der Benutzer seine Rechte **vollumfänglich nutzen** kann, muss der folgende Registrierungsschlüssel angepasst sein (was in vielen Fällen bereits erfolgt ist):

```reg
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\LocalAccountTokenFilterPolicy
```

***

#### Ergebnis prüfen

Der Benutzer wirkt weiterhin wie ein ganz normaler Benutzer – **nichts Verdächtiges in den Gruppenmitgliedschaften**:

```cmd
C:\> net user low_priv_user
Benutzername                low_priv_user

Lokale Gruppenmitgliedschaften  *Benutzer  
Globale Gruppenmitgliedschaften *Keine  
```

## RID Hijacking – Administratorrechte ohne offizielle Administratorrolle

Eine Möglichkeit, Administratorrechte zu erlangen, ohne ein Mitglied der Administratorgruppe zu sein, besteht darin, gezielt bestimmte Registrierungseinträge zu manipulieren. Dadurch wird das Betriebssystem dazu gebracht, einem regulären Benutzerkonto dieselben Berechtigungen wie dem integrierten Administratorkonto zuzuweisen.

{% hint style="info" %}
Was ist eine RID?

Beim Erstellen eines Benutzerkontos weist Windows diesem eine sogenannte **Relative Identifier (RID)** zu. Diese RID ist der letzte Teil der **Security Identifier (SID)**, einer eindeutigen Kennung für jedes Benutzerkonto im System. Beim Anmeldevorgang liest der **Local Security Authority Subsystem Service (LSASS)** die RID aus und erstellt auf dieser Basis ein Zugriffstoken.

Der integrierte Administrator hat in jedem Windows-System standardmäßig die RID **500**, während neu erstellte Benutzerkonten in der Regel RIDs ab **1000** aufwärts erhalten.
{% endhint %}

#### RID eines Benutzers ermitteln

Die RID kann über die folgende Kommandozeile abgefragt werden:

```cmd
C:\> wmic useraccount get name,sid
```

**Beispielausgabe:**

```
Name              SID
Administrator     S-1-5-21-...-500
DefaultAccount    S-1-5-21-...-503
Guest             S-1-5-21-...-501
user_1            S-1-5-21-...-1008
user_2            S-1-5-21-...-1009
low_priv_user     S-1-5-21-...-1010
```

Die RID ist jeweils der letzte Abschnitt der SID, in diesem Beispiel also **1010** für das Benutzerkonto `low_priv_user`.

***

#### Ziel: RID 500 → `low_priv_user`

Wird die RID von `low_priv_user` von **1010 auf 500** geändert, stuft Windows das Konto intern als Administrator ein. Dies geschieht unabhängig von der tatsächlichen Gruppenmitgliedschaft.

#### Durchführung

1.  **Registrierungs-Editor mit SYSTEM-Rechten starten**

    Da die SAM-Datenbank (Security Account Manager) durch das System geschützt ist, kann sie nur mit SYSTEM-Rechten bearbeitet werden. Dies kann z. B. mit [psexec64.exe.md](../sysinternals-suite/psexec64.exe.md "mention") aus dem Sysinternals-Toolkit erfolgen:

    ```cmd
    C:\tools\pstools> PsExec64.exe -i -s regedit
    ```
2.  **Navigieren zu folgendem Schlüssel:**

    ```
    HKEY_LOCAL_MACHINE\SAM\SAM\Domains\Account\Users\
    ```
3.  **Den entsprechenden RID-Schlüssel identifizieren**

    Der Unterordner für `low_priv_user` entspricht dessen RID in **hexadezimaler Schreibweise**.\
    Die RID **1010** entspricht in Hex **0x3F2**.
4.  **Den Binärwert „F“ anpassen**

    Im entsprechenden Schlüssel befindet sich ein Binärwert mit dem Namen **`F`**.\
    Innerhalb dieses Werts befindet sich die effektive RID an **Offset 0x30**, codiert im **Little Endian Format**.

    Für RID 500 (0x01F4) muss der Eintrag also lauten: **F4 01**

    Vorher: `F2 03` (entspricht 0x03F2 = 1010)\
    Nachher: `F4 01` (entspricht 0x01F4 = 500)

    Nach dem Speichern dieser Änderung interpretiert Windows das Benutzerkonto `low_priv_user` fortan als Administrator.

<figure><img src="../../../../.gitbook/assets/grafik (10) (1).png" alt=""><figcaption><p>Identifizierung des Accounts (0030) -> (F2 03)</p></figcaption></figure>

***

#### Auswirkungen

Trotz regulärer Gruppenmitgliedschaften verfügt das Konto `low_priv_user` nun über dieselben Berechtigungen wie das integrierte Administratorkonto. Die Ausgabe des folgenden Befehls zeigt jedoch keine Auffälligkeiten:

```cmd
C:\> net user low_priv_user
Benutzername                low_priv_user

Lokale Gruppenmitgliedschaften  *Benutzer  
Globale Gruppenmitgliedschaften *Keine  
```

Dies macht die Methode besonders heimlich und schwer zu erkennen – in der Oberfläche erscheint das Konto weiterhin als gewöhnlicher Benutzer.
