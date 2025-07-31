---
description: Address Resolution Protocol
---

# MAC-Adressen & ARP







## ARP

<figure><img src="../../.gitbook/assets/grafik (2) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
#### Erklärung:

* **ARP Request:** Host A sendet eine Broadcast-Nachricht an alle Geräte im lokalen Netzwerk und fragt: „Wer hat die IP-Adresse 192.168.1.10?“ Dabei gibt Host A auch seine eigene MAC-Adresse mit, damit der Empfänger weiß, wohin antworten.
* **ARP Reply:** Host B (der die IP 192.168.1.10 besitzt) antwortet direkt an Host A (Unicast) mit seiner MAC-Adresse.
{% endhint %}

### Funktionsweise

Das **Address Resolution Protocol (ARP)** wird verwendet, um die **MAC-Adresse** (Hardwareadresse) eines Geräts anhand seiner **IP-Adresse** zu ermitteln. Dies ist notwendig, da die Kommunikation im lokalen Netzwerk (Layer 2) über MAC-Adressen läuft, IP-Adressen jedoch für logische Adressierung auf Layer 3 zuständig sind.

#### Ablauf des ARP-Prozesses:

1. **ARP Request** – Wenn ein Gerät (z. B. ein Computer) ein anderes Gerät im gleichen Netzwerk über dessen IP-Adresse erreichen möchte, aber dessen MAC-Adresse nicht kennt, sendet es ein **ARP-Request-Broadcast**:
   * Ziel-IP: Die IP-Adresse des gesuchten Geräts.
   * Quell-IP: Die IP-Adresse des Anfragenden.
   * Ziel-MAC: `ff:ff:ff:ff:ff:ff` (Broadcast).
   * Quell-MAC: Die eigene MAC-Adresse.
   * Das Paket fragt: „Wer hat IP X.X.X.X? Antworte an MAC Y:Y:Y:Y:Y:Y.“
2. **ARP Reply** – Das Gerät mit der angefragten IP-Adresse antwortet mit einem **ARP Reply**:
   * Die Antwort enthält die eigene MAC-Adresse.
   * Sie wird **direkt** (unicast) an das anfragende Gerät geschickt.
   * Nach Erhalt kann das Gerät den Datenverkehr direkt an die richtige MAC-Adresse senden.

#### Beispiel:

Ein PC möchte ein Paket an `192.168.1.10` senden, kennt aber die zugehörige MAC-Adresse nicht. Er sendet:

```
Wer hat 192.168.1.10? Sagt es 192.168.1.20.
```

Das Gerät mit `192.168.1.10` antwortet:

```
192.168.1.10 ist unter aa:bb:cc:dd:ee:ff erreichbar.
```

{% hint style="info" %}
#### Zusatzinformationen:

* &#x20;ARP funktioniert **nur innerhalb eines lokalen Netzwerks (Broadcast-Domain)**.
* Geräte speichern die Zuordnung von IP- zu MAC-Adressen in einem **ARP-Cache**, um häufige Abfragen zu vermeiden.
* Wenn sich eine MAC-Adresse ändert oder veraltet ist, wird eine neue ARP-Anfrage gestellt.
* ARP ist **nicht verschlüsselt oder authentifiziert** – was es anfällig für sogenannte **ARP-Spoofing-Angriffe** macht (z. B. in Man-in-the-Middle-Szenarien).
* Dieses einfache Protokoll ist essenziell für die IPv4-Kommunikation in lokalen Netzwerken.
{% endhint %}

***

### ARP-Paketfelder im Detail:

<figure><img src="../../.gitbook/assets/grafik (1) (1).png" alt=""><figcaption><p>ARP-Packet</p></figcaption></figure>

| Feld           | Beschreibung                                                |
| -------------- | ----------------------------------------------------------- |
| Hardwaretyp    | Typ des Hardware-Netzwerks (Ethernet = 1)                   |
| Protokolltyp   | Typ des Protokolls (IPv4 = 0x0800)                          |
| Hardwaregröße  | Länge der MAC-Adresse (6 Bytes)                             |
| Protokollgröße | Länge der IP-Adresse (4 Bytes)                              |
| Opcode         | 1 = Anfrage, 2 = Antwort                                    |
| Sender MAC     | MAC-Adresse des Absenders                                   |
| Sender IP      | IP-Adresse des Absenders                                    |
| Ziel MAC       | MAC-Adresse des Empfängers (bei Anfrage: 00:00:00:00:00:00) |
| Ziel IP        | IP-Adresse des Empfängers                                   |
