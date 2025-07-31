# TCP

**TCP (Transmission Control Protocol)** ist ein verbindungsorientiertes Protokoll, das vor der Datenübertragung einen sogenannten **3-Wege-Handshake** durchführt (SYN → SYN-ACK → ACK). Es garantiert die **zuverlässige Übertragung**, stellt sicher, dass Daten **in der richtigen Reihenfolge** ankommen, und implementiert **Fehlerkorrektur** und Wiederholungsmechanismen bei Paketverlust. Durch diese Mechanismen ist TCP langsamer, aber ideal für Anwendungen, bei denen **Datenintegrität wichtig** ist, wie z. B. beim Webverkehr (HTTP/HTTPS), E-Mails oder Dateiübertragungen (FTP).

## Header

<figure><img src="../../.gitbook/assets/grafik (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/79ca8e4acbd573a27cee413cde927769.png" alt=""><figcaption></figcaption></figure>

1. **URG (Urgent)**: Das Urgent-Flag zeigt an, dass das „Urgent Pointer“-Feld relevant ist. Der Urgent Pointer weist darauf hin, dass eingehende Daten als dringend betrachtet werden und ein TCP-Segment mit gesetztem URG-Flag sofort verarbeitet wird – ohne darauf zu warten, dass zuvor gesendete TCP-Segmente eintreffen.
2. **ACK (Acknowledgement)**: Das Acknowledgement-Flag zeigt an, dass die Bestätigungsnummer im Header gültig ist. Es wird verwendet, um den Empfang eines TCP-Segments zu bestätigen.
3. **PSH (Push)**: Das Push-Flag fordert TCP auf, die Daten unverzüglich an die Anwendung weiterzugeben.
4. **RST (Reset)**: Das Reset-Flag wird verwendet, um eine Verbindung zurückzusetzen. Ein anderes Gerät – z. B. eine Firewall – kann es senden, um eine TCP-Verbindung abzubrechen. Dieses Flag wird auch verwendet, wenn Daten an ein Ziel gesendet werden, dort aber kein Dienst verfügbar ist, um darauf zu antworten.
5. **SYN (Synchronize)**: Das Synchronisations-Flag dient zum Einleiten des TCP-3-Wege-Handshakes und zur Synchronisierung der Sequenznummern mit dem Kommunikationspartner. Während des Verbindungsaufbaus sollte die Sequenznummer zufällig gewählt werden.
6. **FIN (Finish)**: Der Absender signalisiert, dass er keine weiteren Daten mehr zu senden hat.



<figure><img src="../../.gitbook/assets/grafik (3) (1) (1).png" alt=""><figcaption><p>Grundlegende Merkmale</p></figcaption></figure>

## 3-Way Handshake, Datenübertragung, Beenden

<figure><img src="../../.gitbook/assets/grafik (4) (1).png" alt=""><figcaption></figcaption></figure>
