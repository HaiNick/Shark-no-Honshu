# Kerberoasting

#### 1. Funktionsweise

Kerberoasting nutzt die Tatsache aus, dass jeder authentifizierte Benutzer in einer AD-Domäne **Service Tickets** (TGS-Tickets) für beliebige **Service Principal Names (SPNs)** anfordern kann. Diese Tickets sind mit dem Hash des zugehörigen Dienstkontos verschlüsselt. Wird das Ticket extrahiert, kann es offline analysiert und der Hash per Brute-Force oder Wörterbuchangriff entschlüsselt werden – insbesondere, wenn schwache oder alte Verschlüsselungsverfahren wie RC4 verwendet werden.

**Angriffsverlauf:**

* Auflisten verfügbarer SPNs und Dienstkonten
* Anfordern und Export der verschlüsselten Tickets
* Offline-Knacken des Ticket-Hashes (z. B. mit Hashcat)
* Missbrauch des entschlüsselten Dienstkontos zur Privilegienerweiterung und seitlichen Bewegung im Netzwerk

#### 2. Schutzmaßnahmen

Um Kerberoasting-Angriffe zu erschweren, sollten folgende Abwehrstrategien umgesetzt werden:

* **Verwendung starker Passwörter** (mind. 25 Zeichen) für Dienstkonten
* **Einsatz von gMSAs** (gruppenverwaltete Dienstkonten) zur automatisierten Passwortverwaltung
* **Einschränkung von Berechtigungen** nach dem Prinzip der minimalen Rechte
* **Deaktivierung unsicherer Verschlüsselungen** (z. B. RC4) sofern möglich
* **Überwachung verdächtiger Ticketanforderungen** im Netzwerk
* **Einrichtung von Honeytokens** mit SPNs zur Erkennung von SPN-Scanning oder Ticket-Abfragen
