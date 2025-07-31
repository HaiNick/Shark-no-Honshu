# Active Directory

## Active Directory Objekte: Benutzer, Computer und Gruppen

Das Herzstück jeder Windows-Domäne ist der Verzeichnisdienst **Active Directory Domain Services (AD DS)**. Er dient als zentrales Verzeichnis, das sämtliche **Objekte** im Netzwerk verwaltet – darunter Benutzer, Computer, Gruppen, Drucker, Dateifreigaben und viele weitere. Diese Objekte werden in einer hierarchischen Struktur organisiert und über Tools wie **Active Directory-Benutzer und -Computer (ADUC)** administriert.

### Benutzer ([user.md](user.md "mention"))

Benutzer sind eines der häufigsten Objektarten in AD und zählen zu den sogenannten **Security Principals**. Das bedeutet, sie können authentifiziert werden und spezifische Zugriffsrechte auf Netzwerkressourcen (z. B. Dateien, Drucker) erhalten.

**Benutzerobjekte können zwei Arten von Entitäten repräsentieren:**

* **Personen:** In der Regel reale Mitarbeiter, die Zugriff auf das Netzwerk benötigen.
* **Dienste:** Bestimmte Dienste wie IIS oder MSSQL benötigen eigene Benutzerkonten, sogenannte **Service Accounts**, die mit minimalen Rechten ausgestattet sind, um nur ihre jeweilige Funktion auszuführen.

### Computer (Machines)

Für jeden Computer, der einer AD-Domäne beitritt, wird automatisch ein **Computerobjekt** erstellt. Auch Computer gelten als **Security Principals** und erhalten ein eigenes Konto mit limitierten Rechten innerhalb der Domäne.

**Besonderheiten von Computerkonten:**

* Sie sind standardmäßig lokale Administratoren auf dem eigenen Gerät.
* Der Benutzerzugriff auf Computerkonten ist unüblich, aber technisch möglich, wenn das Kennwort bekannt ist.
* Passwörter von Computerkonten bestehen aus ca. 120 zufälligen Zeichen und werden automatisch regelmäßig geändert.
* Der Kontoname entspricht dem Computernamen mit angehängtem `$` (z. B. `DC01$`).

### Sicherheitsgruppen (Security Groups)

Sicherheitsgruppen dienen der Verwaltung von Berechtigungen. Durch sie kann man Rechte zentral für ganze Benutzer- oder Computergruppen definieren, anstatt einzelnen Konten individuell Berechtigungen zuzuweisen.

**Eigenschaften:**

* Gruppen sind ebenfalls **Security Principals**.
* Mitglieder können Benutzer, Computer oder sogar andere Gruppen sein.
* Benutzer können gleichzeitig Mitglied mehrerer Gruppen sein.

**Beispiele wichtiger vordefinierter Gruppen:**

| Gruppe                 | Beschreibung                                                                          |
| ---------------------- | ------------------------------------------------------------------------------------- |
| **Domain Admins**      | Vollzugriff auf die gesamte Domäne, inkl. aller Domain Controller.                    |
| **Server Operators**   | Verwaltung von Domain Controllern, ohne Gruppenmitgliedschaften ändern zu können.     |
| **Backup Operators**   | Zugriff auf alle Dateien zur Durchführung von Backups, unabhängig von Berechtigungen. |
| **Account Operators**  | Dürfen Benutzerkonten erstellen oder ändern.                                          |
| **Domain Users**       | Enthält alle Benutzerkonten der Domäne.                                               |
| **Domain Computers**   | Enthält alle Computerkonten der Domäne.                                               |
| **Domain Controllers** | Enthält alle Domain Controller.                                                       |

Die vollständige Liste findet sich in der Microsoft-Dokumentation.

***

## Active Directory-Benutzer und -Computer

Zur Verwaltung von Benutzern, Gruppen und Computern wird die MMC-Konsole **"Active Directory-Benutzer und -Computer"** verwendet (`dsa.msc`). Sie bietet eine grafische Oberfläche, in der Objekte übersichtlich dargestellt und bearbeitet werden können.

### Organisationseinheiten (Organizational Units, OUs)

OUs sind Containerobjekte, die zur logischen Strukturierung von AD-Objekten verwendet werden – häufig entsprechend der Organisationsstruktur (z. B. Abteilungen wie IT, Vertrieb, HR).

**Funktion von OUs:**

* Gruppierung von Benutzern oder Computern mit ähnlichen Anforderungen.
* Anwendung von Gruppenrichtlinien (GPOs) auf gesamte OUs.
* Eine Person oder ein Gerät kann immer nur Mitglied **einer** OU sein.

**Beispielstruktur:**

```
PSY
├── IT
├── Management
├── Marketing
└── Sales
```

Zusätzliche OUs (z. B. "Students") können manuell hinzugefügt werden.

### Vordefinierte Container

Neben den vom Administrator erstellten OUs existieren auch automatisch generierte Standardcontainer:

| Container                    | Funktion                                                 |
| ---------------------------- | -------------------------------------------------------- |
| **Builtin**                  | Enthält systemdefinierte Gruppen für alle Windows-Hosts. |
| **Computers**                | Standardziel für neue Computerkonten.                    |
| **Domain Controllers**       | Beinhaltet alle Domain Controller.                       |
| **Users**                    | Enthält standardmäßige Benutzer und Gruppen.             |
| **Managed Service Accounts** | Speichert Konten für Dienste in der Domäne.              |

***

## Unterschied: Sicherheitsgruppen vs. Organisationseinheiten

Obwohl sowohl Sicherheitsgruppen als auch OUs der Kategorisierung von Objekten dienen, haben sie unterschiedliche Zwecke:

| Merkmal           | Sicherheitsgruppen                                  | Organisationseinheiten (OUs)          |
| ----------------- | --------------------------------------------------- | ------------------------------------- |
| Zweck             | Zuweisung von Berechtigungen                        | Anwendung von Richtlinien             |
| Mitglieder        | Benutzer, Computer, andere Gruppen                  | Objekte (Benutzer, Computer)          |
| Mehrfachzugehörig | Ja – Benutzer können Mitglied mehrerer Gruppen sein | Nein – nur eine OU pro Objekt möglich |
| Verwendung        | Zugriff auf Freigaben, Drucker etc.                 | GPOs, administrative Delegation       |

{% hint style="info" %}
* **OUs** strukturieren die Domäne und sind Grundlage für **Richtlinienverwaltung**.
* **Gruppen** regeln **Zugriffsrechte** auf Ressourcen und lassen sich flexibel kombinieren.
{% endhint %}
