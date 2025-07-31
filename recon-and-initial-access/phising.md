# Phising

## Infrastruktur für Phising-Kampagne

**Domain-Name:**\
Registrierung eines geeigneten Domain-Namens, entweder authentisch wirkend oder an eine bestehende Domäne angelehnt.

**SSL/TLS-Zertifikate:**\
Erstellung von SSL/TLS-Zertifikaten für die gewählte Domain zur Erhöhung der Glaubwürdigkeit bei verschlüsselten Verbindungen.

**E-Mail-Server/-Konto:**\
Einrichtung eines eigenen Mailservers oder Nutzung eines SMTP-Dienstanbieters zur Versendung von E-Mails.

**DNS-Einträge:**\
Einrichtung relevanter DNS-Einträge (wie SPF, DKIM, DMARC), um die Zustellbarkeit der E-Mails zu verbessern und zu verhindern, dass Nachrichten im Spam-Ordner landen.

**Webserver:**\
Bereitstellung eines Webservers oder Nutzung von Webhosting-Diensten zum Hosten von Webseiten. Der Einsatz von SSL/TLS-Zertifikaten erhöht auch hier die Glaubwürdigkeit.

**Analysewerkzeuge:**\
Für die Auswertung einer simulierten Kampagne (z. B. bei Red Team Assessments) wird ein Analysetool benötigt, um z. B. den Versandstatus, Öffnungen und Klicks von E-Mails sowie das Verhalten auf der Webseite (z. B. Eingabe persönlicher Daten oder Download von Software) zu erfassen



Perfekt – hier ist dein gewünschter Inhalt im passenden Format, angelehnt an die Struktur "Infrastruktur für Phishing-Kampagnen":

***

## Auswahl der passenden Domain

**Abgelaufene Domains:**\
Registrierung von Domains mit historischer Nutzung (z. B. ehemals aktive Webseiten), um die Vertrauenswürdigkeit gegenüber Spam-Filtern zu erhöhen. Neue Domains werden von vielen Filtern als potenziell verdächtig eingestuft.

**Typosquatting:**\
Registrierung von Domains, die bekannten Marken oder Zielseiten ähneln – häufig durch typische Tippfehler oder visuelle Ähnlichkeiten. Beispiele:

* `goggle.com` statt `google.com` (Schreibfehler)
* `go.ogle.com` statt `google.com` (zusätzlicher Punkt/Subdomain)
* `g00gle.com` statt `google.com` (Zahlen statt Buchstaben)
* `googles.com` statt `google.com` (Phrasenvariation)
* `googleresults.com` statt `google.com` (zusätzliche Wörter)\
  Ziel ist es, beim schnellen Lesen über Unterschiede hinwegzutäuschen.

**TLD-Varianten:**\
Verwendung der gleichen Domain mit einer anderen Top-Level-Domain (TLD), um Verwechslungen zu erzeugen. Beispiel:

* `tryhackme.co.uk` statt `tryhackme.com`\
  Nutzer achten oft nicht auf die genaue Endung der Domain, was Angreifern zugutekommt.

**IDN-Homograph-Angriffe (Script Spoofing):**\
Registrierung von Domains mit Zeichen aus fremden Schriftsystemen, die optisch identisch zu lateinischen Zeichen sind. Beispiel:

* `а` (kyrillisch) vs. `a` (lateinisch)\
  Durch solche Zeichenmanipulationen entstehen täuschend echte Domains, die mit bloßem Auge nicht von der Originaladresse zu unterscheiden sind.

## Phising mithilfe

### Angriffsvektor:Dropper

Dropper sind Programme, die Phishing-Opfer oft dazu verleiten, sie herunterzuladen und auf ihrem System auszuführen. Der Dropper gibt sich häufig als nützlich oder legitim aus, beispielsweise als Codec zum Ansehen eines bestimmten Videos oder als Software zum Öffnen einer bestimmten Datei.

\
Dropper selbst sind in der Regel nicht bösartig und bestehen daher meist Antiviren-Prüfungen. Nach der Installation wird die Schadsoftware entweder entpackt oder von einem Server heruntergeladen und auf dem Computer des Opfers installiert. In der Regel verbindet sich die Schadsoftware mit der Infrastruktur des Angreifers. So kann der Angreifer die Kontrolle über den Computer des Opfers übernehmen und das lokale Netzwerk weiter erkunden und ausnutzen.

### Angriffsvektor: Office-Dokumente mit Makros

**Zielsysteme:**\
Microsoft Office-Anwendungen wie Word, Excel und PowerPoint – insbesondere, wenn Makros aktiviert oder aktivierbar sind.

**Beschreibung:**\
Office-Dokumente werden als E-Mail-Anhänge verschickt und enthalten eingebettete **Makros**. Diese kleinen Skripte (z. B. in VBA geschrieben) können beim Öffnen oder Aktivieren im Dokument **Systembefehle ausführen**.

**Legitimer Nutzen:**\
Makros werden im Unternehmensumfeld oft zur Automatisierung von Arbeitsprozessen eingesetzt (z. B. Berichte, Formulare, Berechnungen).

**Missbrauchspotenzial:**\
Angreifer nutzen Makros, um:

* Schadsoftware auf dem Zielsystem zu installieren,
* eine Verbindung zu einem Command-and-Control-Server (C2) aufzubauen,
* Fernzugriff auf das betroffene System zu erhalten.

**Angriffsweg:**\
Das Opfer erhält eine E-Mail mit einem angehängten Office-Dokument, z. B. „Rechnung.docm“ oder „Bewerbung.xlsm“. Nach dem Öffnen wird das Opfer aufgefordert, „Makros zu aktivieren“, um den Inhalt vollständig zu sehen. Mit der Aktivierung startet der eingebettete Schadcode.

**Besonderheit:**\
Makro-basierte Angriffe sind besonders wirksam bei schlecht geschützten Clients, bei denen Makros nicht standardmäßig deaktiviert sind oder Sicherheitsrichtlinien fehlen.

### Angriffsvektor: Browser-Exploits

**Zielsysteme:**\
Webbrowser wie Internet Explorer, Microsoft Edge, Mozilla Firefox, Google Chrome, Safari u. a. – insbesondere, wenn veraltete Versionen im Einsatz sind.

**Beschreibung:**\
Ausnutzung von Schwachstellen im Browser selbst, um Schadcode aus der Ferne auszuführen. Dies ermöglicht einem Angreifer, Kontrolle über das System des Opfers zu erlangen, ohne dass zusätzlicher Code heruntergeladen werden muss.

**Voraussetzungen:**\
In der Regel nur erfolgversprechend, wenn bekannt ist, dass beim Zielsystem alte oder nicht mehr unterstützte Browsertechnologien verwendet werden. Dies ist häufiger der Fall in großen Organisationen mit fest integrierter, nicht aktualisierbarer Software – z. B. im Bildungswesen, im öffentlichen Sektor oder im Gesundheitswesen.

**Einschränkungen:**

* Moderne Browser sind schwer auszunutzen aufgrund regelmäßiger Updates und verbesserter Sicherheitsarchitektur.
* Exploits für aktuelle Browser-Versionen sind selten öffentlich und oft sehr wertvoll im Rahmen von Bug-Bounty-Programmen.
* Einsatz im Rahmen von Red-Teaming-Kampagnen ist selten und meist nur mit Hintergrundinformationen über veraltete Systeme sinnvoll.

**Angriffsweg:**\
Das Opfer wird z. B. über eine Phishing-E-Mail dazu verleitet, eine vom Angreifer kontrollierte Webseite zu besuchen. Der Browser führt beim Aufruf dieser Seite automatisch den Exploit aus, was dem Angreifer ermöglicht, beliebige Befehle auf dem System auszuführen.

**Beispiel:**

* **CVE-2021-40444** (September 2021): Schwachstelle in Microsoft-Systemen, bei der allein der Besuch einer manipulierten Webseite zur Ausführung von Schadcode führen konnte – ganz ohne Benutzerinteraktion.



## Tools:

* [https://getgophish.com/](https://getgophish.com/)
* [https://www.trustedsec.com/tools/the-social-engineer-toolkit-set/](https://www.trustedsec.com/tools/the-social-engineer-toolkit-set/)
