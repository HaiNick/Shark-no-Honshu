
**Active Directory Domain Service (AD DS)** ist der zentrale Bestandteil jeder Windows-Domäne. AD DS dient als Katalog und speichert Informationen zu allen Objekten im Netzwerk, wie Benutzer, Gruppen, Computer, Drucker und Freigaben.

### Benutzer

Benutzer sind eine der häufigsten Objektarten in AD und gehören zu den sogenannten **Security Principals**, die authentifiziert werden können und Berechtigungen für Ressourcen erhalten. Sie repräsentieren meist:

- **Personen**: Mitarbeiter, die Zugriff auf das Netzwerk benötigen.
- **Dienste**: Spezifische Benutzer für Dienste wie IIS oder MSSQL mit den erforderlichen Berechtigungen für ihre Aufgaben.

### Computer

Computer, die einer Domäne beitreten, erhalten ein eigenes Konto als **Security Principal**. Dieses Konto wird automatisch erstellt und mit einem zufällig generierten Passwort verwaltet. Computerkonten sind meist nur für die jeweilige Maschine selbst zugänglich und folgen der Namenskonvention **Computername + $ (z. B. DC01$).

### Security Groups

**Security Groups** ermöglichen die Vergabe von Zugriffsrechten auf Ressourcen an Gruppen statt an einzelne Benutzer, was die Verwaltung vereinfacht. Benutzer, die einer Gruppe hinzugefügt werden, erben automatisch deren Berechtigungen. Als **Security Principals** können Sicherheitsgruppen also auch Zugriffsrechte auf Netzwerkressourcen besitzen.

Gruppen können sowohl Benutzer als auch Computer enthalten und bei Bedarf weitere Gruppen einschließen.

In jeder Domäne gibt es einige vordefinierte Gruppen mit speziellen Rechten:

|**Gruppe**|**Beschreibung**|
|---|---|
|**Domain Admins**|Besitzt administrative Rechte über die gesamte Domäne, einschließlich aller Computer und Domain Controller (DCs).|
|**Server Operators**|Dürfen Domain Controller verwalten, jedoch keine Mitglieder in administrativen Gruppen ändern.|
|**Backup Operators**|Können auf alle Dateien zugreifen (unabhängig von Berechtigungen) und sind für Datensicherungen vorgesehen.|
|**Account Operators**|Dürfen Benutzerkonten in der Domäne erstellen oder ändern.|
|**Domain Users**|Beinhaltet alle Benutzerkonten der Domäne.|
|**Domain Computers**|Beinhaltet alle Computerkonten der Domäne.|
|**Domain Controllers**|Beinhaltet alle Domain Controller in der Domäne.|

### Zugriff auf AD

**Active Directory Users and Computers** ist das Tool, um Benutzer, Gruppen und Computer in Active Directory zu verwalten. Dazu meldet man sich am Domain Controller an und startet ==Active Directory Users and Computers==. 

**Startmenü: AD Users and Computers**

Beim Starten von **Active Directory Users and Computers** öffnet sich ein Fenster, in dem die Hierarchie der Benutzer, Computer und Gruppen der Domäne sichtbar wird. Diese Objekte sind in **Organisationseinheiten (OUs)** organisiert, die als Container zur Klassifizierung von Benutzern und Computern dienen. 

OUs werden hauptsächlich genutzt, um Benutzergruppen mit ähnlichen Richtlinienanforderungen zu definieren. Beispielsweise erhalten Mitarbeiter im Vertrieb andere Richtlinien als das IT-Personal. Ein Benutzer kann jedoch nur Mitglied einer einzigen OU sein.

Windows erstellt automatisch bestimmte Container:

- **Builtin**: Enthält Standardgruppen für alle Windows-Hosts.
- **Computers**: Standardcontainer für neu beigetretene Computer; bei Bedarf verschiebbar.
- **Domain Controllers**: Enthält die Domain Controller der Domäne.
- **Users**: Standardbenutzer und -gruppen für die gesamte Domäne.
- **Managed Service Accounts**: Speichert Konten für Dienste in der Windows-Domäne.

### Unterschied zwischen Sicherheitsgruppen und OUs
- **OUs** (Organisationseinheiten) dienen dazu, Richtlinien auf Benutzer und Computer anzuwenden. Sie ermöglichen spezifische Konfigurationen je nach Rolle der Benutzer im Unternehmen. Ein Benutzer kann jedoch nur Mitglied einer OU sein, um widersprüchliche Richtlinien zu vermeiden.
- **Sicherheitsgruppen** hingegen regeln Zugriffsrechte auf Ressourcen wie freigegebene Ordner oder Netzwerkdrucker. Ein Benutzer kann mehreren Gruppen angehören, um Zugriff auf verschiedene Ressourcen zu erhalten.

### OUs löschen
OUs sind standardmäßig gegen löschen geschützt. Das kann durch Rechtsklick auf das Object > Advanced Features > Object "Protect object from accidental deletion" - Häckchen im Kästchen entfernen

### Delegation
Eine nützliche Funktion in Active Directory ist die Möglichkeit, bestimmten Benutzern eingeschränkte Rechte über bestimmte OUs zu geben. Dieser Vorgang wird **Delegation** genannt und ermöglicht es, Benutzern gezielte Berechtigungen für Aufgaben in OUs zu erteilen, ohne dass ein Domänenadministrator eingreifen muss.

Ein häufiges Anwendungsbeispiel ist die Vergabe von Rechten an den IT-Support, damit dieser die Passwörter von anderen Benutzern mit niedrigen Berechtigungen zurücksetzen kann.