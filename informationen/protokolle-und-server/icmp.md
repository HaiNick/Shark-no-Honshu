# ICMP



<figure><img src="../../.gitbook/assets/grafik (3) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Beschreibung

**ICMP** ist ein Protokoll zur Übertragung von Fehlermeldungen und Diagnoseinformationen im IP-Netzwerk.

Es hilft Geräten, Probleme bei der Kommunikation zu erkennen, z.B. wenn ein Ziel nicht erreichbar ist.

Ein bekanntes Werkzeug basierend auf ICMP ist **Ping**, das durch Echo Request und Echo Reply prüft, ob ein Host erreichbar ist.

Weitere wichtige ICMP-Typen sind:

* **Destination Unreachable**: Ziel kann nicht erreicht werden.
* **Time Exceeded**: Paket wurde verworfen, z.B. wegen Ablauf der TTL.
* **Redirect**: Gibt an, dass ein besserer Router existiert.
{% endhint %}

***

### **Funktionsweise**

Das **Internet Control Message Protocol (ICMP)** ist ein Netzwerkprotokoll, das zur **Diagnose**, **Fehlerbenachrichtigung** und **Kontrolle** der Netzwerkkommunikation verwendet wird. Es gehört zur **Internet-Schicht (Layer 3)** des OSI-Modells und wird typischerweise von IP verwendet.

#### Hauptfunktionen von ICMP:

1. **Fehlermeldungen senden**, wenn ein IP-Paket nicht zugestellt werden kann.
2. **Diagnosewerkzeuge ermöglichen**, z. B. mit `ping` oder `traceroute`.

#### Typische ICMP-Nachrichten:

| ICMP-Typ | Bedeutung               | Beispiel                                              |
| -------- | ----------------------- | ----------------------------------------------------- |
| 0        | Echo Reply              | Antwort auf eine `ping`-Anfrage                       |
| 3        | Destination Unreachable | Ziel nicht erreichbar (z. B. Port, Host oder Netz)    |
| 5        | Redirect                | Routing-Anpassung empfohlen                           |
| 8        | Echo Request            | Anfrage wie bei `ping`, zum Prüfen der Erreichbarkeit |
| 11       | Time Exceeded           | TTL (Time-to-Live) abgelaufen                         |

#### Beispiel: `ping` mit ICMP

Wenn man `ping` auf eine IP-Adresse ausführt:

1. Der Host sendet ein **ICMP Echo Request** an das Ziel.
2. Das Ziel antwortet mit einem **ICMP Echo Reply**.
3. Anhand der Antwortzeit erkennt man, ob das Ziel erreichbar ist und wie lange der Weg dauert.

**Ping-Ablauf (vereinfacht):**

* Host A sendet ICMP Echo Request → Host B
* Host B sendet ICMP Echo Reply → Host A

Falls Host B nicht erreichbar ist oder ein Router dazwischen das Paket nicht zustellen kann, kann ein ICMP **Destination Unreachable** zurückkommen.

{% hint style="info" %}
#### Zusatzinformationen:

* ICMP ist **kein Transportprotokoll** wie TCP oder UDP, sondern dient rein der Netzwerksteuerung.
* ICMP-Nachrichten werden **direkt in IP-Paketen** transportiert (kein Port verwendet).
* Einige Firewalls oder Sicherheitsrichtlinien blockieren ICMP (z. B. `ping`) aus Sicherheitsgründen.
* ICMPv6 existiert als eigene Variante für IPv6 und übernimmt zusätzliche Aufgaben, z. B. für Nachbarschaftserkennung (statt ARP).
{% endhint %}
