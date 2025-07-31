In Windows-Domänen werden alle Anmeldeinformationen auf den Domain Controllern gespeichert. Wenn ein Benutzer versucht, sich mit Domänenanmeldeinformationen bei einem Dienst zu authentifizieren, muss der Dienst den Domain Controller zur Überprüfung der Anmeldedaten anfragen. Windows-Domänen unterstützen zwei Authentifizierungsprotokolle:

- **Kerberos**: Standardprotokoll für aktuelle Windows-Versionen.
- **NetNTLM**: Ein älteres Protokoll, das hauptsächlich aus Kompatibilitätsgründen weiterhin unterstützt wird.

Obwohl **NetNTLM** als veraltet gilt, ist es oft weiterhin aktiviert.

### Kerberos-Authentifizierung

Die Kerberos-Authentifizierung ist das Standardprotokoll in aktuellen Windows-Domänen. Benutzer, die sich über Kerberos anmelden, erhalten sogenannte **Tickets** als Bestätigung ihrer Authentifizierung. Diese Tickets ermöglichen es dem Benutzer, ohne erneute Passwortabfrage auf Dienste zuzugreifen.

#### Ablauf der Kerberos-Authentifizierung

1. **Anfrage an das Key Distribution Center (KDC)**: Der Benutzer sendet seinen Benutzernamen und einen Zeitstempel, verschlüsselt mit einem Schlüssel, der aus seinem Passwort abgeleitet ist, an das KDC. Das KDC ist ein Dienst auf dem Domain Controller, der für die Vergabe von Kerberos-Tickets zuständig ist.
    
2. **Ticket Granting Ticket (TGT)**: Das KDC erstellt ein **Ticket Granting Ticket (TGT)**, das es dem Benutzer ermöglicht, weitere Tickets für bestimmte Dienste anzufordern, ohne jedes Mal seine Anmeldeinformationen erneut angeben zu müssen. Neben dem TGT erhält der Benutzer einen **Session Key**, der für nachfolgende Anfragen erforderlich ist.
    
3. **Sicherheit des TGT**: Das TGT ist mit dem Passwort-Hash des `krbtgt`-Kontos verschlüsselt, sodass der Benutzer dessen Inhalt nicht einsehen kann. Das KDC muss den Session Key nicht speichern, da dieser bei Bedarf durch Entschlüsselung des TGTs wiederhergestellt werden kann.

![[Pasted image 20241103210523.png]]Wenn ein Benutzer sich mit einem Netzwerkdienst wie einer Freigabe, Website oder Datenbank verbinden möchte, nutzt er sein **Ticket Granting Ticket (TGT)**, um beim **Key Distribution Center (KDC)** ein **Ticket Granting Service (TGS)** zu beantragen. Ein TGS erlaubt nur die Verbindung zu dem spezifischen Dienst, für den es erstellt wurde.

#### Ablauf der TGS-Anfrage

1. **Anfrage für ein TGS**: Der Benutzer sendet seinen Benutzernamen und einen Zeitstempel, verschlüsselt mit dem Session Key, zusammen mit dem TGT und einem **Service Principal Name (SPN)** an das KDC. Der SPN gibt den gewünschten Dienst und den Server an, auf den zugegriffen werden soll.
    
2. **Antwort des KDC**: Das KDC sendet dem Benutzer das **TGS** sowie einen **Service Session Key**. Dieser Service Session Key wird für die Authentifizierung beim angeforderten Dienst benötigt.
    
3. **Verschlüsselung des TGS**: Das TGS ist mit einem Schlüssel verschlüsselt, der aus dem **Service Owner Hash** abgeleitet ist. Der **Service Owner** ist das Benutzer- oder Computerkonto, unter dem der Dienst läuft. Da das TGS eine Kopie des Service Session Keys enthält, kann der Service Owner diesen durch Entschlüsselung des TGS abrufen und den Zugriff entsprechend autorisieren.

![[Pasted image 20241103210547.png]]
Das TGS kann anschließend an den gewünschten Dienst gesendet werden, um die Authentifizierung durchzuführen und eine Verbindung herzustellen. Der Dienst verwendet den Passwort-Hash des konfigurierten Dienstkontos, um das TGS zu entschlüsseln und den Service Session Key zu validieren.


![[Pasted image 20241103210604.png]]



### NetNTLM-Authentifizierung

Die NetNTLM-Authentifizierung nutzt einen **Challenge-Response-Mechanismus**. Der Ablauf gestaltet sich wie folgt:


![[Pasted image 20241103210726.png]]

1. **Authentifizierungsanfrage**: Der Client sendet eine Authentifizierungsanfrage an den Server, auf den er zugreifen möchte.
    
2. **Challenge-Erstellung**: Der Server generiert eine zufällige Zeichenfolge (Challenge) und sendet diese an den Client.
    
3. **Antwortgenerierung durch den Client**: Der Client kombiniert den NTLM-Passwort-Hash mit der Challenge und weiteren bekannten Daten, um eine **Antwort** (Response) zu generieren, die er an den Server zurücksendet.
    
4. **Weiterleitung zur Verifizierung**: Der Server leitet die Challenge und die generierte Antwort an den Domain Controller weiter, um die Authentifizierung zu überprüfen.
    
5. **Überprüfung durch den Domain Controller**: Der Domain Controller nutzt die Challenge, um die Antwort neu zu berechnen und vergleicht diese mit der vom Client gesendeten Antwort. Stimmen beide überein, wird der Client authentifiziert; andernfalls wird der Zugriff verweigert. Das Authentifizierungsergebnis wird an den Server zurückgesendet.
    
6. **Übermittlung des Ergebnisses an den Client**: Der Server leitet das Ergebnis der Authentifizierung an den Client weiter.
    

> **Wichtig**: Das Passwort oder dessen Hash wird aus Sicherheitsgründen nie direkt über das Netzwerk übertragen.

**Hinweis**: Dieser Ablauf gilt für Domain-Konten. Bei lokalen Konten kann der Server die Antwort auf die Challenge selbst überprüfen, da er den Passwort-Hash lokal in seiner SAM-Datenbank gespeichert hat und keine Kommunikation mit dem Domain Controller erforderlich ist.