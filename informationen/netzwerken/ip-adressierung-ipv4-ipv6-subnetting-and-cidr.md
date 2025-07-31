# IP-Adressierung (IPv4/IPv6), Subnetting & CIDR

## IP-Adressierung (IPv4/IPv6),IDR

**IP-Adressierung** ist das grundlegende Verfahren, um Geräten in Netzwerken eindeutige Adressen zuzuweisen. Es gibt zwei Hauptversionen:

* **IPv4** verwendet 32-Bit-Adressen, dargestellt als vier Dezimalzahlen (z.B. 192.168.1.10). IPv4-Adressen sind in folgende Klassen eingeteilt:
  * **Klasse A:** 1.0.0.0 – 126.255.255.255 (Netzwerkpräfix /8, sehr große Netze)
  * **Klasse B:** 128.0.0.0 – 191.255.255.255 (Netzwerkpräfix /16)
  * **Klasse C:** 192.0.0.0 – 223.255.255.255 (Netzwerkpräfix /24)
  * **Klasse D:** 224.0.0.0 – 239.255.255.255 (Multicast)
  * **Klasse E:** 240.0.0.0 – 255.255.255.255 (Reserviert für experimentelle Zwecke)
* **IPv6** nutzt 128-Bit-Adressen, dargestellt in Hexadezimal-Blöcken (z.B. 2001:0db8::1), um den Adressmangel von IPv4 zu beheben.

***

## Subnetting

**Subnetting** teilt ein großes Netzwerk in kleinere Subnetze auf, indem man Bits von der Host-Adresse für Netzwerke nutzt. Ziel ist es, IP-Adressen effizient zu nutzen und Netzwerkverkehr besser zu kontrollieren.

**Beispiel:**\
Netzwerk: 192.168.1.0/24 (klassisches Klasse-C-Netz mit 256 Adressen)

Wir wollen das Netzwerk in 4 gleich große Subnetze aufteilen.

* /24 bedeutet, dass 24 Bits für das Netzwerk reserviert sind, bleiben 8 Bits für Hosts.
* Für 4 Subnetze brauchen wir 2 zusätzliche Bits (2^2=4), also erweitern wir die Maske auf /26 (24+2).

Die Subnetze wären dann:

* 192.168.1.0/26 (Hosts 192.168.1.1–62)
* 192.168.1.64/26 (Hosts 192.168.1.65–126)
* 192.168.1.128/26 (Hosts 192.168.1.129–190)
* 192.168.1.192/26 (Hosts 192.168.1.193–254)

***

## CIDR

**CIDR (Classless Inter-Domain Routing)** ist eine flexible Methode zur Netzwerkeinteilung, die keine festen Klassen verwendet. Eine Adresse mit CIDR-Notation sieht so aus: `192.168.1.0/26`, wobei `/26` angibt, wie viele Bits das Netzwerk definieren.

CIDR ermöglicht eine effiziente Adressvergabe und feinere Segmentierung von Netzwerken.

