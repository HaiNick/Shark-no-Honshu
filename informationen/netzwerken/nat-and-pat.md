# NAT & PAT

<figure><img src="../../.gitbook/assets/grafik (1) (1) (1).png" alt=""><figcaption><p>NAT</p></figcaption></figure>

## **NAT (Network Address Translation)**

**Network Address Translation (NAT)** ist ein Verfahren, das in Routern und Firewalls verwendet wird, um private IP-Adressen innerhalb eines lokalen Netzwerks (LAN) in öffentliche IP-Adressen umzuwandeln – und umgekehrt. NAT ermöglicht es mehreren Geräten in einem privaten Netzwerk, über eine einzige öffentliche IP-Adresse mit dem Internet zu kommunizieren.

NAT operiert auf **Layer 3** des OSI-Modells (Netzwerkschicht) und modifiziert die IP-Header von Paketen, wenn sie das Netzwerk verlassen oder betreten.

***

### **Funktionsweise**

Wenn ein Client im LAN eine Verbindung zum Internet aufbaut:

1. Das Paket wird vom Client mit einer privaten Quell-IP-Adresse gesendet (z. B. 192.168.1.10).
2. Der NAT-Router ersetzt im ausgehenden Paket die Quell-IP-Adresse durch seine öffentliche IP-Adresse (z. B. 203.0.113.5).
3. Beim Rückweg des Antwortpakets ersetzt der Router die Ziel-IP-Adresse wieder durch die ursprüngliche private IP, basierend auf einer gespeicherten NAT-Tabelle.

***

### **Typen von NAT**

* **Static NAT (SNAT):** 1:1-Zuordnung zwischen interner und externer IP-Adresse. Wird selten genutzt, meist für Server.
* **Dynamic NAT:** Eine öffentliche IP wird dynamisch aus einem Pool zugewiesen.
* **PAT (Port Address Translation)** / **NAT Overload:** Die häufigste Form. Mehrere interne Geräte teilen sich eine öffentliche IP-Adresse, unterscheidbar über unterschiedliche **Portnummern**.

***

### **Beispiel für PAT (häufigster NAT-Typ)**

<table><thead><tr><th width="131">Internes Gerät</th><th>Quell-IP</th><th>Quell-Port</th><th width="159">Öffentliche IP</th><th>NAT-Port</th></tr></thead><tbody><tr><td>PC1</td><td>192.168.0.2</td><td>12345</td><td>203.0.113.5</td><td>60001</td></tr><tr><td>PC2</td><td>192.168.0.3</td><td>12345</td><td>203.0.113.5</td><td>60002</td></tr></tbody></table>

Der Router speichert diese Zuordnung und kann eingehende Antworten korrekt weiterleiten.

***

**Vorteile von NAT**

* Spart öffentliche IPv4-Adressen
* Erhöht Sicherheit durch Maskierung interner IPs
* Ermöglicht einfache Netzwerkerweiterung

**Nachteile**

* NAT ist **nicht transparent für alle Protokolle** (z. B. einige VoIP-Anwendungen, IPsec)
* **Breaks end-to-end connectivity** – direkte Kommunikation zwischen Hosts kann erschwert sein
* Erfordert häufig Zusatzmechanismen wie **ALGs** (Application Layer Gateways) oder **NAT Traversal**

***

**Verwendung in IPv6**

In IPv6 wird NAT nicht mehr als notwendig angesehen, da durch den großen Adressraum jeder Host eine globale Adresse erhalten kann. Sicherheitsfunktionen werden hier über Firewalls und Routing-Regeln realisiert.
