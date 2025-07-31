# Angriffsvektoren

## Sniffing - Attacke

Bei einem Sniffing-Angriff wird ein Tool zum Abfangen von Netzwerkpaketen verwendet, um Informationen über das Ziel zu sammeln. Wenn ein Protokoll im Klartext kommuniziert, können die ausgetauschten Daten von einer dritten Partei aufgefangen und analysiert werden. Ein einfaches Abfangen von Netzwerkpaketen kann Informationen wie den Inhalt privater Nachrichten und Anmeldedaten offenbaren, wenn die Daten bei der Übertragung nicht verschlüsselt werden.

Ein Sniffing-Angriff kann mit einer Ethernet-Netzwerkkarte (802.3) durchgeführt werden, sofern der Benutzer über die entsprechenden Berechtigungen verfügt (Root-Rechte unter Linux und Administratorrechte unter MS Windows)\
Tools die dafür verwendet werden können:

* Tcpdump
* Wireshark
* Tshark

siehe [programme-skripte](programme-skripte/ "mention")

## MITM (Man-in-the-middle)

Ein MITM-Angriff (Man-in-the-Middle) liegt vor, wenn ein Opfer (A) glaubt, dass es mit einem legitimen Ziel (B) kommuniziert, aber unwissentlich mit einem Angreifer (E) kommuniziert. In der folgenden Abbildung fordert A die Überweisung von 20 Dollar an M an. E hat diese Nachricht jedoch verändert und den ursprünglichen Wert durch einen neuen ersetzt. B hat die geänderte Nachricht erhalten und daraufhin gehandelt.

Tools:

* Ettercap
* Bettercap

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/3cb0c4f9dde184bf8dcd6b3a45418a44.png" alt=""><figcaption></figcaption></figure>

MITM kann auch andere Klartextprotokolle wie FTP, SMTP und POP3 betreffen. Um diesen Angriff abzuschwächen, ist der Einsatz von Kryptographie erforderlich. Die Lösung liegt in einer angemessenen Authentifizierung zusammen mit einer Verschlüsselung oder Signierung der ausgetauschten Nachrichten. Mit Hilfe der Public Key Infrastructure (PKI) und vertrauenswürdiger Stammzertifikate schützt Transport Layer Security (TLS) vor MITM-Angriffen.

## Password Attacke

Gewöhnlicherweise:

* Erraten ( Social Engineering )
* Wörterbuch Attacke
* Brute Force
