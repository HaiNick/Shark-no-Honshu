---
description: Dynamic Host Configuration Protocol
---

# DHCP

DHCP ist ein Protokoll auf **Anwendungsebene**, das sich auf **UDP** stützt; der Server hört auf UDP-Port **67**, und der Client sendet von UDP-Port **68**.

<figure><img src="../../.gitbook/assets/grafik (4).png" alt=""><figcaption></figcaption></figure>

***

### **Funktionsweise**

Das **Dynamic Host Configuration Protocol (DHCP)** ermöglicht es Geräten, automatisch eine gültige Netzwerkkonfiguration zu erhalten – inklusive **IP-Adresse**, **Subnetzmaske**, **Gateway** und **DNS-Server** –, ohne dass eine manuelle Konfiguration nötig ist.

Der Prozess besteht aus vier Schritten:

1. **DHCP Discover** – Der Client, der noch keine IP-Konfiguration besitzt, sendet ein Broadcast-Paket (Absender-IP: `0.0.0.0`, Ziel-IP: `255.255.255.255`) ins Netzwerk, um DHCP-Server zu finden. Auf der Link-Layer-Ebene wird dieses Paket an die Broadcast-MAC-Adresse `ff:ff:ff:ff:ff:ff` gesendet.
2. **DHCP Offer** – Der DHCP-Server antwortet dem Client mit einem Angebot (`DHCPOFFER`), das eine verfügbare IP-Adresse und weitere Netzwerkeinstellungen enthält. Diese Antwort wird an die MAC-Adresse des Clients gerichtet.
3. **DHCP Request** – Der Client akzeptiert das Angebot und sendet erneut ein Broadcast-Paket (`DHCPREQUEST`) – ebenfalls mit `0.0.0.0` als Quell-IP und `255.255.255.255` als Ziel-IP, solange er die angebotene IP noch nicht nutzen darf.
4. **DHCP Acknowledge** – Der Server bestätigt die Zuweisung der IP-Adresse und der Konfiguration mit einem `DHCPACK`. Erst jetzt ist die IP-Adresse offiziell vom Client nutzbar.

{% hint style="info" %}
**Zusatzinformationen:**

* Zu Beginn kennt der Client lediglich seine **MAC-Adresse**, nicht jedoch eine IP-Konfiguration.
* Erst nach der vierten Nachricht (DHCPACK) kann der Client die zugewiesene IP-Adresse nutzen.
* Die Verwendung von `0.0.0.0` als Quelladresse und `255.255.255.255` als Zieladresse ist notwendig, da der Client noch nicht im Netzwerk adressierbar ist.
{% endhint %}
