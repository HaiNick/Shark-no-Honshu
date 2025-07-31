# Windows NT

Windows NT ist der modulare Kernel, der als technologische Grundlage für zahlreiche Windows-Betriebssysteme von Microsoft dient. Ursprünglich für den Intel i860-Prozessor (Codename „N-Ten“) konzipiert, wurde der Name später mit „New Technology“ assoziiert. Seit Version 5.0 (Windows 2000) tritt der Begriff nicht mehr im Produktnamen auf, bleibt aber als interne Versionskennzeichnung erhalten.

***

### Security Identifier (SID)

Ein **Security Identifier (SID)** ist ein zentraler Bestandteil der Sicherheitsarchitektur von Microsoft Windows. Er dient als eindeutiger, unveränderlicher Identifikator für Benutzer, Gruppen, Computer und andere sicherheitsrelevante Objekte innerhalb eines Windows-Netzwerks. SIDs sind tief in das Sicherheitsmodell von Windows NT und seinen Nachfolgern eingebettet und bilden die Grundlage für die Rechtevergabe über Access Control Lists (ACLs).

#### Funktion und Zweck

Jedes Objekt, das sicherheitsrelevant ist – wie ein Benutzerkonto oder ein Dienst – erhält bei seiner Erstellung automatisch einen SID. Diese Identifikatoren werden in den **Zugriffssteuerungslisten (ACLs)** verwendet, um Rechte zuzuweisen. Das bedeutet: Der Zugriff auf Dateien, Ordner, Registrierungsschlüssel oder Dienste erfolgt nicht über Namen, sondern über die zugehörigen SIDs. Dadurch bleiben Zugriffsrechte auch dann erhalten, wenn sich der Anzeigename eines Objekts ändert.

#### Aufbau eines SIDs

Ein SID besteht aus mehreren Komponenten, die seine Herkunft und Zuordnung eindeutig definieren. Beispiel:

```
S-1-5-21-7623811015-3361044348-030300820-1013
```

**Aufgeschlüsselt:**

* **S**: Einleitendes Kennzeichen für „Security Identifier“
* **1**: Revisionsnummer (aktuell immer 1)
* **5**: Identifier Authority – gibt die Quelle des SID an. Hier steht „5“ für „NT Authority“
* **21-7623811015-3361044348-030300820**: Dieser Teil stellt die **Domänen- oder Systemkennung** dar. Bei lokalen Konten ist es der SID des Computersystems. Bei Domänenkonten ist dies der eindeutige Identifikator der Domäne.
* **1013**: Die **Relative ID (RID)**, eine laufende Nummer, die das Objekt innerhalb des Systems oder der Domäne eindeutig identifiziert. Normale Benutzerkonten beginnen typischerweise bei 1000.

**Übersicht häufiger „Identifier Authorities“:**

| Wert | Authority                          |
| ---- | ---------------------------------- |
| 0    | Null Authority                     |
| 1    | World Authority                    |
| 2    | Local Authority                    |
| 3    | Creator Authority                  |
| 4    | Non-unique Authority               |
| 5    | NT Authority (z. B. SYSTEM, ADMIN) |
| 9    | Resource Manager Authority         |
| 16   | Mandatory Level                    |

#### Vergabe von SIDs

Die SID eines Computers wird bereits bei der Installation des Betriebssystems generiert, basierend auf einem Zufallswert, um Eindeutigkeit im Netzwerk zu garantieren. Benutzerkonten erhalten ihren SID entweder:

* **Lokal**: Der Benutzer-SID basiert auf dem System-SID plus einer eindeutigen RID.
* **In einer Domäne**: Der Benutzer-SID wird aus dem Domänen-SID plus RID zusammengesetzt. Wird ein Benutzer in eine andere Domäne verschoben, erhält er zwangsläufig einen neuen SID.

Zusätzlich gibt es sogenannte **well-known SIDs**, die auf jedem System identisch sind – etwa für die Gruppe _Administratoren_ oder das Konto _SYSTEM_.

#### Probleme bei identischen SIDs (Klonen)

Ein häufiges praktisches Problem entsteht bei der Verwendung von **Systemabbildern (Images)**: Klont man ein fertig installiertes Windows-System ohne Vorbereitung, erhalten alle daraus entstandenen Systeme identische SIDs. Dies kann zu schwer zu diagnostizierenden Zugriffsproblemen führen, etwa bei Netzlaufwerken oder Remotediensten.

Microsoft empfiehlt daher den Einsatz des Tools **Sysprep**, welches beim ersten Start eines geklonten Systems neue SIDs generiert. Frühere Drittanbieter-Tools wie **NewSID** sind heute obsolet und werden nicht mehr offiziell empfohlen.

#### Verwaltung und Wiederherstellung

* Mit dem Tool **PsGetSid** (von Sysinternals) lassen sich SIDs lokal oder über das Netzwerk auslesen.
* **ADSIEdit** erlaubt – mit administrativem Aufwand – die manuelle Bearbeitung von SIDs in Active Directory-Umgebungen.
* In Domänen mit **Windows Server 2008 oder höher** lassen sich verlorene AD-Objekte inklusive SID über Shadow Copies wiederherstellen, sofern das Functional Level dies unterstützt.

**Temporäre SIDs bei Anmeldesitzungen**\
Neben festen und bekannten SIDs gibt es auch temporär generierte SIDs, die bei jeder interaktiven Anmeldung erstellt werden. Ein Beispiel ist eine SID in der Form `S-1-5-5-X-Y`, wie etwa `S-1-5-5-0-336530617`. Diese sogenannte **Logon Session SID** identifiziert eine einzelne Anmeldung eines Benutzers auf einem System. Sie wird bei der Authentifizierung generiert und dient unter anderem der sicheren Kommunikation und Rechteverwaltung innerhalb der spezifischen Sitzung. Diese Art von SID ist **nicht dauerhaft** und wird beim nächsten Login neu erzeugt. Sie wird häufig in sicherheitsrelevanten Prozessen verwendet, z. B. bei der Vergabe temporärer Zugriffsrechte oder beim Start von Prozessen im Benutzerkontext.

***

### Referenzen:

* [https://de.wikipedia.org/wiki/Microsoft\_Windows\_NT](https://de.wikipedia.org/wiki/Microsoft_Windows_NT)
* [https://learn.microsoft.com/de-de/windows-server/identity/ad-ds/manage/understand-security-identifiers](https://learn.microsoft.com/de-de/windows-server/identity/ad-ds/manage/understand-security-identifiers) | [https://learn.microsoft.com/en-us/sysinternals/downloads/psgetsid](https://learn.microsoft.com/en-us/sysinternals/downloads/psgetsid)
