# Allgemein

### Windows Domains

Eine Windows-Domäne ist ein logischer Zusammenschluss von Computern, Benutzern und Ressourcen innerhalb eines Netzwerks, der zentral über einen Domain Controller (DC) verwaltet wird. Diese Struktur ermöglicht eine einheitliche Authentifizierung, Autorisierung und Sicherheitsrichtlinienverwaltung.

**Eigenschaften einer Windows-Domäne:**

* Benutzer melden sich an einem zentralen Verzeichnisdienst (z. B. Active Directory) an, nicht lokal am PC.
* Policies, Updates und Berechtigungen können zentral verwaltet werden.
* Ein Benutzerkonto kann sich auf jedem Domain-Mitgliedscomputer anmelden, sofern berechtigt.
* Geräte wie Drucker, Dateifreigaben und Server lassen sich effizient organisieren und zuweisen.

**Verwaltung:**\
Domänen werden mit Werkzeugen wie der **Active Directory-Domänen- und -Vertrauensstellung**, **Active Directory-Benutzer und -Computer (ADUC)** sowie über PowerShell verwaltet.

***

### Active Directory

**Active Directory (AD)** ist Microsofts Verzeichnisdienst, der innerhalb einer Windows-Domäne die zentrale Verwaltung von Benutzern, Gruppen, Computern und anderen Ressourcen ermöglicht.

**Kernfunktionen:**

* Speicherung und Strukturierung von Objekten (z. B. Benutzer, Gruppen, Geräte) in einer hierarchischen Struktur.
* Einsatz des **Lightweight Directory Access Protocol (LDAP)** zur Abfrage und Verwaltung von Daten.
* Verwendung des **Kerberos-Protokolls** für Authentifizierung.
* Unterstützung von Gruppenrichtlinien zur Durchsetzung sicherheitsrelevanter Einstellungen.

**Verwaltungstools:**

* `dsa.msc` (Active Directory-Benutzer und -Computer)
* `adsiedit.msc` (zur erweiterten Bearbeitung von AD-Objekten)
* PowerShell-Module wie `ActiveDirectory`

Active Directory ist unverzichtbar für größere Netzwerke und bietet die Basis für die meisten modernen Windows-Infrastrukturen.

***

### Managing Users in AD

Siehe [user.md](user.md "mention")

Die Benutzerverwaltung in Active Directory ist eine Kernaufgabe für Administratoren. Benutzerobjekte stellen reale Personen dar, die auf Netzwerkressourcen zugreifen dürfen.

**Typische Verwaltungsaufgaben:**

* Erstellung von Benutzerkonten mit spezifischen Eigenschaften (z. B. E-Mail, Telefonnummer, Gruppenmitgliedschaften).
* Zuweisung von Benutzerrechten und -rollen.
* Konfiguration von Passwortrichtlinien, Ablaufdaten, Anmeldezeiten und -bereichen.
* Verwaltung über die GUI (ADUC) oder PowerShell (`New-ADUser`, `Set-ADUser`, `Get-ADUser`).

**Best Practices:**

* Organisieren von Benutzern in Organisationseinheiten (OUs).
* Nutzung von Gruppen zur Zuweisung von Berechtigungen.
* Deaktivierung oder Löschung inaktiver Konten.
* Durchsetzung starker Passwortvorgaben und MFA, sofern möglich.

***

### Managing Computers in AD

Siehe [strukturierung-von-computerkonten.md](strukturierung-von-computerkonten.md "mention")

Computerobjekte in AD repräsentieren physische oder virtuelle Windows-Systeme, die Mitglieder der Domäne sind. Sie ermöglichen zentrale Richtlinienanwendung, Softwarebereitstellung und Sicherheitskonfigurationen.

**Verwaltungsmöglichkeiten:**

* Join eines Computers zur Domäne (über Systemsteuerung oder PowerShell `Add-Computer`).
* Zuordnung zu Organisationseinheiten für gezielte Gruppenrichtlinienanwendung.
* Automatische oder manuelle Namensvergabe und Kategorisierung.
* Verwendung von `Active Directory-Benutzer und -Computer` zur Verwaltung oder PowerShell (`Get-ADComputer`, `Move-ADObject`).

**Wichtige Maßnahmen:**

* Regelmäßige Überprüfung veralteter oder inaktiver Computerobjekte.
* Strukturierung nach Standort oder Abteilung.
* Absicherung durch Gruppenrichtlinien und Endpoint Security.

***

### Group Policies

Siehe [gruppenrichtlinien-group-policies.md](gruppenrichtlinien-group-policies.md "mention")

**Gruppenrichtlinien (Group Policies, GPOs)** sind ein zentrales Verwaltungswerkzeug zur Steuerung von Benutzer- und Computereinstellungen in einem Active Directory-Umfeld.

**Funktion:**

* Anwendung von Konfigurationen auf Benutzer und Computer innerhalb von OUs.
* Durchsetzung von Sicherheitsrichtlinien, Anmelde- und Passwortvorgaben, Netzwerk- und Softwareeinstellungen.
* Automatische Installation von Software oder Scripten beim Start oder bei Benutzeranmeldung.

**Verwaltung:**

* GPMC (`gpmc.msc`) – Gruppenrichtlinienverwaltungs-Konsole.
* Lokale Gruppenrichtlinien mit `gpedit.msc`.
* Wichtige Befehle: `gpupdate /force`, `gpresult /r`, `Get-GPO`.

**Vererbung und Priorität:**

* GPOs folgen einer Hierarchie: Local > Site > Domain > OU.
* Konflikte werden durch die sogenannte "LSDOU"-Reihenfolge und "Enforce" bzw. "Block Inheritance" geregelt.

***

### Authentication Methods

Siehe [authentifizierung.md](authentifizierung.md "mention")

Windows unterstützt mehrere Authentifizierungsmethoden für die Anmeldung an Systemen oder Netzwerkdiensten. Die Sicherheit der Authentifizierung ist zentral für den Schutz von Ressourcen.

**Gängige Methoden:**

* **Kerberos** (Standard in AD): Ticket-basiert, sicher, verschlüsselt.
* **NTLM** (älter, weniger sicher): Wird noch genutzt, wenn Kerberos nicht verfügbar ist.
* **Smartcard-basierte Anmeldung**: Verlangt physische Karte + PIN.
* **Biometrische Anmeldung**: z. B. Windows Hello (Fingerabdruck, Gesichtserkennung).
* **Passwort + MFA (Multi-Faktor-Authentifizierung)**: Zusätzliche Sicherheitsstufe.

**Verwaltung:**

* Authentifizierungskonfigurationen erfolgen über Gruppenrichtlinien oder Sicherheitsrichtlinien.
* Tools wie `secpol.msc`, `net accounts`, `Set-ADAccountAuthenticationPolicy`.

**Sicherheitshinweis:**\
Wenn möglich, sollte NTLM deaktiviert und MFA flächendeckend aktiviert werden.

***

### Trees, Forests and Trusts

Siehe [trees-forests-und-trusts.md](trees-forests-und-trusts.md "mention")

In einer komplexeren Active Directory-Infrastruktur sind **Bäume (Trees)**, **Wälder (Forests)** und **Vertrauensstellungen (Trusts)** zentrale Konzepte zur Skalierung und Zusammenarbeit mehrerer Domänen.

**Trees (Bäume):**

* Eine Gruppe von AD-Domänen, die durch Namensraumhierarchie miteinander verbunden sind (z. B. `corp.example.com`, `sales.corp.example.com`).
* Innerhalb eines Baums existiert automatisch eine bidirektionale Vertrauensstellung.

**Forests (Wälder):**

* Der höchste logische Container in AD, bestehend aus einem oder mehreren Bäumen.
* Geteiltes Schema und globaler Katalog.
* Trennung von Forests meist aus organisatorischen oder Sicherheitsgründen.

**Trusts (Vertrauensstellungen):**

* Erlauben Authentifizierungs- und Autorisierungsanfragen zwischen Domänen oder Forests.
* Typen: One-way / Two-way, Transitive / Non-transitive.
* Konfiguration via „Active Directory Domains and Trusts“ (`domain.msc`).

**Verwaltungsaspekte:**

* Planung und Verwaltung von Forests und Trusts ist ein hochkritischer Bestandteil der AD-Architektur.
* Vertrauensstellungen sollten regelmäßig getestet und überprüft werden (`nltest`, `netdom trust`).

