# Windows

```shell-session
tasklist /m /fi "pid eq 1304"
```



## Nutzerkonten

Jeder Benutzer mit administrativen Rechten ist Teil der Gruppe Administratoren. Standardbenutzer sind Teil der Gruppe Benutzer.

* **Administrator**: Höchste Berechtigung, kann jede Systemkonfiguration bearbeiten. **Vollzugriff**
* **Standard User:** Eingeschränkter Zugriff. Meist keine Möglichkeit um permantente Änderungen/ wichtige Änderungen durchzuführen. **Limitiert auf eigene Dateien**.
* **SYSTEM**/**LocalSystem**: Von OS für interne Aufgaben verwendet. Vollzugriff auf alle Dateien/Ressourcen. **Höhere Privilegien als Administrator**.
* **Local Service**: Standardkonto, das zur **Ausführung von Windows-Diensten mit "minimalen" Rechten** verwendet wird. Es werden anonyme Verbindungen über das Netzwerk verwendet.
* **Network Service**: Standardkonto, das zur **Ausführung von Windows-Diensten mit "minimalen" Rechten** verwendet wird. Es **verwendet** die **Anmeldeinformationen des Computers**, um sich über das **Netzwerk zu authentifizieren**.

## PowerShell

[https://www.comparitech.com/net-admin/powershell-cheat-sheet/](https://www.comparitech.com/net-admin/powershell-cheat-sheet/)



## Wichtige Links

### Befehlsliste

{% content-ref url="../cheat-sheets/windows-cmd.md" %}
[windows-cmd.md](windows-cmd.md)
{% endcontent-ref %}

### Privilege Escalation

{% content-ref url="../privilege-escalation/windows-privesc.md" %}
[windows-privesc.md](windows-privesc.md)
{% endcontent-ref %}

### Schwachstellen

{% embed url="https://learn.microsoft.com/de-de/security-updates/securitybulletins/securitybulletins" %}
