---
description: Lightweight Directory Access Protocol
---

# LDAP

LDAP (Lightweight Directory Access Protocol) ist ein Netzwerkprotokoll, das für den Zugriff auf und die Verwaltung von verteilten Verzeichnisdiensten genutzt wird. Es ermöglicht Clients, Informationen aus einem Verzeichnis wie Benutzer, Gruppen, Computer oder andere Ressourcen abzurufen und zu bearbeiten. LDAP kommuniziert typischerweise über TCP/UDP Port 389, beziehungsweise Port 636 bei SSL/TLS-gesicherten Verbindungen. Im Bereich des Pentestings und Blue Teamings spielt LDAP eine zentrale Rolle, da es in Active Directory (AD)-Umgebungen weit verbreitet ist, um Benutzerdaten und Gruppenrichtlinien abzurufen. Eine unsachgemäße Konfiguration kann zu Informationslecks führen, und durch unzureichend validierte Eingaben sind LDAP-Injection-Angriffe möglich.

Die LDAP-Authentifizierung stellt eine häufig genutzte Methode dar, um Benutzer gegen Active Directory zu authentifizieren – insbesondere bei Drittanbieter-Anwendungen wie GitLab, Jenkins, VPNs, Netzwerkdruckern oder eigens entwickelten Webanwendungen. Im Gegensatz zur NTLM-Authentifizierung überprüft die Anwendung bei LDAP die Anmeldedaten der Benutzer direkt, meist unter Verwendung eigener AD-Zugangsdaten, mit denen die Benutzeranmeldung verifiziert wird.

Besonders kritisch wird es, wenn diese Anwendungen oder Dienste aus dem Internet erreichbar sind, da sie dann ähnliche Angriffsflächen wie NTLM-basierte Systeme bieten. Da die Dienste eigene AD-Zugangsdaten benötigen, eröffnen sich zusätzliche Angriffsmöglichkeiten, insbesondere der Versuch, diese Dienst-Anmeldeinformationen auszulesen, um sich uneingeschränkten Zugriff auf das Active Directory zu verschaffen.

***

### Angriffsmöglichkeit: LDAP Pass-back Attack

Eine spezielle und interessante Angriffsmethode ist der sogenannte **LDAP Pass-back Angriff**. Er kommt häufig bei Netzwerkgeräten wie Druckern zum Einsatz, wenn man Zugriff auf das interne Netzwerk erhält (z. B. durch Einschleusen eines Rogue-Devices).

**Vorgehen:**

* Man erhält Zugriff auf das Gerät, etwa über dessen Webinterface, in dem LDAP-Konfigurationsparameter gespeichert sind.
* Häufig sind Admin-Zugangsdaten zu solchen Interfaces Standardwerte (z. B. admin:admin).
* Obwohl man das LDAP-Passwort meist nicht direkt auslesen kann, lässt sich die LDAP-Serveradresse ändern.
* Ändert man die LDAP-Serveradresse auf die eigene Angreifer-IP, versucht das Gerät bei einem „Test“ der LDAP-Verbindung, sich an den gefälschten Server zu authentifizieren.
* Damit kann man die LDAP-Anmeldedaten abfangen, sofern die Authentifizierung nicht durch sichere Methoden geschützt ist.

***

### Technische Umsetzung eines LDAP Pass-back Angriffs

* Ein einfacher Netcat Listener auf Port 389 (Standard-LDAP-Port) kann für erste Tests genutzt werden, um Verbindungsversuche zu erkennen.
* Da moderne LDAP-Clients oft sichere Authentifizierungsmechanismen verwenden, wird das Passwort nicht im Klartext übertragen.
* Um dennoch die Anmeldedaten abzugreifen, muss man einen **Rogue LDAP-Server** aufsetzen, der unsichere Authentifizierungsmechanismen (z. B. PLAIN und LOGIN) unterstützt und damit die Kommunikation auf Klartext herunterstufen kann.

***

### Einrichtung eines Rogue LDAP Servers (Beispiel mit OpenLDAP)

* OpenLDAP wird installiert und konfiguriert (z. B. Domain: `za.company.com`).
* Die Sicherheitsstufe wird mit einer `ldif`-Datei (z.B. `olcSaslSecProps.ldif`) heruntergesetzt, um nur einfache Authentifizierungsmechanismen zuzulassen.
* Mit `ldapmodify` wird die Konfiguration angepasst und der Dienst neu gestartet.
* Ein `ldapsearch`-Befehl kann genutzt werden, um die unterstützten Authentifizierungsmechanismen zu überprüfen (sollten PLAIN und LOGIN sein).

***

### Abfangen der Anmeldedaten

* Durch wiederholtes Auslösen der LDAP-Verbindungstests am Zielgerät werden die Anmeldeinformationen in Klartext an den Rogue LDAP Server gesendet.
* Mit Tools wie `tcpdump` kann der Netzwerkverkehr auf Port 389 analysiert und das Passwort extrahiert werden.
* So erhält man valide AD-Anmeldedaten des Dienstes, was weiteren Zugriff ermöglicht.

***

Siehe [windows-recon.md](../../killchain/recon-and-initial-access/reconnaissance-aufklarung/windows-recon.md "mention")
