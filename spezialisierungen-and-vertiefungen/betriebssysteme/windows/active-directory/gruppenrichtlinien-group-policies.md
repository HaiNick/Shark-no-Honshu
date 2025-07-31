# Gruppenrichtlinien (Group Policies)

## Übersicht

Gruppenrichtlinien (Group Policies) sind ein zentrales Verwaltungsinstrument in Microsoft Active Directory (AD), das es Administratoren ermöglicht, systemweite Einstellungen und Sicherheitsvorgaben effizient auf Benutzer und Computer in einer Domäne anzuwenden. Diese Einstellungen werden in sogenannten **Group Policy Objects (GPOs)** zusammengefasst.

GPOs lassen sich granular auf bestimmte Organisationseinheiten (OUs) anwenden, sodass sich unterschiedliche Konfigurationen für verschiedene Abteilungen oder Gerätegruppen umsetzen lassen.

***

## Funktionsweise von GPOs

Ein **Group Policy Object (GPO)** ist eine Sammlung von Konfigurationen, die sich auf zwei Bereiche aufteilen:

* **Computer Configuration:** Einstellungen, die auf Geräte angewendet werden, unabhängig vom angemeldeten Benutzer.
* **User Configuration:** Einstellungen, die auf Benutzerkonten angewendet werden, unabhängig davon, an welchem Gerät sie sich anmelden.

GPOs können mit bestimmten OUs in der AD-Hierarchie verknüpft werden. Sie gelten dann sowohl für die verknüpfte OU als auch für alle untergeordneten OUs.

***

## Verwaltung über die Group Policy Management Console (GPMC)

Die Konfiguration von GPOs erfolgt über die **Group Policy Management Console (GPMC)**, die über das Startmenü aufgerufen werden kann. Beim Öffnen zeigt sie die gesamte OU-Struktur der Domäne.

In der GPMC werden neue GPOs unterhalb von **Group Policy Objects** erstellt und anschließend mit den gewünschten OUs **verknüpft** („linked“).

**Beispiel: Vorhandene GPOs**

In einer typischen Umgebung existieren bereits einige Standard-GPOs, wie:

* **Default Domain Policy:** Wird auf die gesamte Domäne angewendet.
* **Default Domain Controllers Policy:** Gilt ausschließlich für die OU „Domain Controllers“.
* **RDP Policy (optional):** Beispielsweise zur Regelung von Remotezugriffen.

***

## GPO-Inhalte und Struktur

Beim Öffnen eines GPOs lassen sich dessen Konfigurationsbereiche über mehrere Tabs einsehen:

* **Scope:** Zeigt an, wo das GPO verlinkt ist. Hier kann auch ein **Security Filtering** eingerichtet werden, um die Anwendung auf bestimmte Benutzer oder Computer zu beschränken (Standard: _Authenticated Users_).
* **Settings:** Listet die konkreten Richtlinieninhalte auf – getrennt in Benutzer- und Computerkonfigurationen.
* **Details:** Metadaten wie Erstellungsdatum, letzte Änderungen, etc.
* **Delegation:** Zugriffskontrolle für Administratoren oder bestimmte Rollen.

***

## Beispielhafte Richtlinientypen

Einige häufig genutzte Richtlinien innerhalb von GPOs:

* **Passwortrichtlinien:** z. B. Mindestlänge, Komplexitätsanforderungen
* **Account Lockout:** Sperrung nach X fehlgeschlagenen Anmeldeversuchen
* **Desktop- und UI-Einschränkungen:** z. B. Sperrung der Systemsteuerung
* **Automatische Bildschirmsperre:** Lock nach Inaktivität
* **Windows Updates und Firewall-Konfigurationen**

***

## Verteilung und Replikation von GPOs

GPOs werden über die **SYSVOL-Freigabe** auf den Domain Controllern im Netzwerk bereitgestellt. Diese befindet sich standardmäßig unter:

```
C:\Windows\SYSVOL\sysvol\
```

Domänenmitglieder synchronisieren ihre GPOs regelmäßig mit der SYSVOL-Freigabe. Änderungen an GPOs werden dabei **nicht sofort** übernommen – die standardmäßige Replikationszeit beträgt bis zu **90–120 Minuten**.

**Manuelle Aktualisierung**

Eine sofortige Aktualisierung auf einem Zielgerät kann durch folgenden Befehl ausgelöst werden:

```powershell
gpupdate /force
```

***

## GPO-Vererbung und Verlinkung

* GPOs wirken **rekursiv** auf untergeordnete OUs.
* Eine OU kann **mehrere GPOs** zugewiesen bekommen. Die Ausführungsreihenfolge wird über **„Link Order“** und eventuell über **„Enforce“** bzw. **„Block Inheritance“** gesteuert.
* Die **„Default Domain Policy“** wirkt z. B. auf alle Benutzer und Computer innerhalb der gesamten Domäne.

***

## Praxisbeispiele für typische GPOs

**1. Einschränkung des Zugriffs auf die Systemsteuerung**

Ziel: Nur Mitglieder der IT-Abteilung dürfen auf die Systemsteuerung zugreifen.

* GPO-Name: `Restrict Control Panel Access`
* Einstellung:\
  **User Configuration → Administrative Templates → Control Panel → Prohibit access to Control Panel and PC settings**
* Verlinkung: Marketing-, Sales- und Management-OUs
* Sicherheitseinstellung: Ausschluss der IT-Benutzer durch Security Filtering

**2. Automatische Bildschirmsperre nach Inaktivität**

Ziel: Geräte sollen sich nach 5 Minuten Inaktivität automatisch sperren.

* GPO-Name: `Auto Lock Screen`
* Einstellung:\
  **Computer Configuration → Policies → Administrative Templates → Control Panel → Personalization → Screen saver timeout (300 Sekunden)**\
  und\
  **Enable Screen Saver** + **Password protect the screen saver**
* Verlinkung: Entweder direkt auf die OUs „Workstations“, „Servers“ und „Domain Controllers“\
  &#xNAN;_&#x6F;der_ auf die Wurzel der Domäne (`thm.local`) – da alle Ziel-OUs untergeordnet sind

***

{% hint style="warning" %}
Sicherheitshinweise

* Änderungen an GPOs sollten stets dokumentiert werden.
* Kritische Richtlinien sollten zunächst in einer Testumgebung evaluiert werden.
* Die „Default Domain Policy“ sollte **nicht leichtfertig verändert** werden – besser eigene GPOs für neue Anforderungen erstellen.
* Bei vielen GPOs ist auf **Konflikte** und **Vererbungsreihenfolgen** zu achten.
{% endhint %}
