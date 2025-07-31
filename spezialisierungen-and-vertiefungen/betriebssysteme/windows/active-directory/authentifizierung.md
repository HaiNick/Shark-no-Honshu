# Authentifizierung

## Authentifizierungsprotokolle in Windows-Domänen

### Überblick

In Windows-Domänenumgebungen erfolgt die zentrale Speicherung und Verwaltung von Benutzeranmeldeinformationen (Credentials) auf den **Domain Controllern (DCs)**. Bei jeder Authentifizierung einer Domänenidentität durch einen Dienst (z. B. ein Dateiserver oder Webdienst) muss dieser den Domain Controller kontaktieren, um die übermittelten Anmeldeinformationen zu überprüfen.

Zwei Hauptprotokolle stehen in Windows-Netzwerken zur Verfügung:

| Protokoll               | Status   | Verwendung                                         |
| ----------------------- | -------- | -------------------------------------------------- |
| **Kerberos**            | Standard | Seit Windows 2000, in modernen Netzwerken Standard |
| **NetNTLM (NTLMv1/v2)** | Veraltet | Wird für Abwärtskompatibilität beibehalten         |

***

### Kerberos-Authentifizierung

Kerberos ist das **standardmäßige Authentifizierungsprotokoll** in aktuellen Windows-Versionen. Es basiert auf einem Ticket-System, das erlaubt, Dienste im Netzwerk ohne wiederholte Passwortübermittlung zu nutzen. Der zentrale Bestandteil von Kerberos ist der **Key Distribution Center (KDC)**, welcher in Windows-Domänen auf dem Domain Controller läuft.

**Ablauf der Kerberos-Authentifizierung**

<figure><img src="../../../../.gitbook/assets/grafik (13).png" alt=""><figcaption><p>Sequenzdiagramm Ablauf</p></figcaption></figure>

**1. Initiale Authentifizierung**

* Der Benutzerclient sendet einen mit seinem Passwort abgeleiteten Schlüssel verschlüsselten **Zeitstempel** und **Benutzernamen** an den KDC.
* Der KDC überprüft die Anmeldeinformationen und stellt ein **TGT (Ticket Granting Ticket)** sowie einen **Session Key** aus.

**2. Dienstzugriff anfordern**

* Für den Zugriff auf einen spezifischen Dienst sendet der Client:
  * Das TGT,
  * Einen verschlüsselten Zeitstempel (mittels Session Key),
  * Den **Service Principal Name (SPN)** des gewünschten Dienstes.

**3. Service Ticket erhalten**

* Der KDC stellt ein **TGS (Ticket Granting Service Ticket)** aus, das für den spezifischen Dienst gilt. Dieses Ticket ist mit dem **Hash des Dienstkontos** verschlüsselt.
* Ein **Service Session Key** wird ebenfalls mitgeschickt.

**4. Dienstzugriff**

* Der Client sendet das TGS an den Dienst.
* Der Dienst entschlüsselt es mit seinem Kontoschlüssel, extrahiert den Service Session Key und gewährt bei Erfolg Zugriff.

{% hint style="info" %}
**Vorteile von Kerberos**

* Passwort wird **nicht** wiederholt übertragen.
* Unterstützt **Single Sign-On (SSO)**.
* Tickets haben definierte Gültigkeitsdauer, wodurch Sicherheit erhöht wird.
{% endhint %}

***

### Beispiel: Kerberos-Ticketfluss

<figure><img src="../../../../.gitbook/assets/grafik (12).png" alt=""><figcaption></figcaption></figure>

***

### NTLM und NetNTLM

**New Technology LAN Manager (NTLM)** ist eine Sammlung von Sicherheitsprotokollen, die zur Authentifizierung von Benutzeridentitäten in Active Directory (AD) Umgebungen verwendet werden. Eine der häufigsten Methoden der NTLM-Authentifizierung ist das Challenge-Response-Verfahren, auch bekannt als **NetNTLM**.

#### Verwendung von NetNTLM

NetNTLM (oft auch Windows Authentication oder einfach NTLM Authentication genannt) ist ein Authentifizierungsmechanismus, bei dem eine Anwendung als Vermittler zwischen dem Client und dem Domain Controller fungiert. Dabei werden Authentifizierungsdaten in Form einer Herausforderung (Challenge) an den Domain Controller weitergeleitet. Erfolgt die Authentifizierung erfolgreich, wird der Benutzer von der Anwendung als authentifiziert betrachtet.

Dieser Mechanismus ermöglicht es der Anwendung, die Identität des Benutzers zu bestätigen, ohne die AD-Zugangsdaten lokal zu speichern. Die sensiblen Anmeldeinformationen verbleiben ausschließlich auf dem Domain Controller, was die Sicherheit erhöht.

#### Typische Einsatzbereiche von NetNTLM

NetNTLM wird in vielen netzwerkbasierten Diensten verwendet, die sowohl intern als auch über das Internet zugänglich sein können. Beispiele hierfür sind:

* Intern gehostete Mailserver mit einem Webmail-Loginportal (z. B. Outlook Web App)
* Remote Desktop Protocol (RDP) Dienste, die von außen erreichbar sind
* VPN-Endpunkte, die in Active Directory integriert sind
* Webanwendungen mit Internetzugang, welche NetNTLM zur Authentifizierung nutzen

***

**Ablauf der NetNTLM-Authentifizierung**

<figure><img src="../../../../.gitbook/assets/grafik (14).png" alt=""><figcaption><p>Authentifizierungsablauf</p></figcaption></figure>

Im Authentifizierungsablauf fungiert die Anwendung als Vermittler zwischen Client und Domain Controller:

1. Der Client initiiert eine Anmeldung bei der Anwendung.
2. Die Anwendung fordert eine NTLM-Challenge an.
3. Der Client beantwortet die Challenge mit einem Hash (NetNTLM-Response).
4. Die Anwendung leitet die Challenge und Antwort an den Domain Controller weiter.
5. Der Domain Controller prüft die Authentizität und gibt die Entscheidung zurück.
6. Die Anwendung authentifiziert den Benutzer basierend auf der Antwort des Controllers.

{% hint style="info" %}
**Sicherheit**

* Der Hash wird **nicht direkt** über das Netzwerk gesendet.
* Dennoch ist NetNTLM **anfällig für Relay-Angriffe**, insbesondere wenn SMB-Signing deaktiviert ist.
* Moderne Umgebungen sollten NetNTLM **nach Möglichkeit deaktivieren** oder den Einsatz stark beschränken.
{% endhint %}

***

### Vergleich: Kerberos vs. NetNTLM

| Merkmal                                | Kerberos            | NetNTLM                 |
| -------------------------------------- | ------------------- | ----------------------- |
| Standard in modernen Windows-Versionen | ✅                   | ❌ (veraltet)            |
| Single Sign-On (SSO) möglich           | ✅                   | ❌                       |
| Challenge-Response-Verfahren           | ❌                   | ✅                       |
| Passwortübertragung über das Netzwerk  | ❌                   | ❌                       |
| Ticket-basiert                         | ✅                   | ❌                       |
| Verwundbar durch Relay-Angriffe        | ⚠️ Nur begrenzt     | ⚠️ Hoch (insb. bei SMB) |
| Sichtbarkeit in Logs                   | Gut nachvollziehbar | Eingeschränkter Kontext |
| Typische Ports                         | 88 (TCP/UDP)        | 139, 445 (TCP)          |

***

{% hint style="info" %}
#### Sicherheitsempfehlungen

* **Kerberos bevorzugen**, wo immer möglich.
* **NetNTLM deaktivieren**, sofern keine Legacy-Systeme bestehen.
* **SMB Signing aktivieren**, um NTLM-Relay-Angriffe zu verhindern.
* **SPNs sauber verwalten**, um Kerberos-Missbrauch zu verhindern.
* **Audit-Logs aktivieren**, um Authentifizierungsversuche nachverfolgen zu können.
{% endhint %}

<details>

<summary>Weitere Ressourcen</summary>

* [Microsoft Docs: Kerberos Authentication](https://learn.microsoft.com/en-us/windows-server/security/kerberos/)

- [Microsoft Docs: NTLM Authentication](https://learn.microsoft.com/en-us/windows-server/security/windows-authentication/ntlm)

* [MIT Kerberos Project](https://web.mit.edu/kerberos/)

</details>
