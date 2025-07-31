# Passive Reconnaissance

Alles an Wissen, das über öffentliche Ressourcen ermittelt werden kann. Dabei wird mit dem Ziel/Zielsystem nicht agiert.

Typische Aktivitäten beinhalten:

* DNS-Einträge auslesen ( Domain von öffentlichem DNS-Server )
* Werbung für Stellenangebote des Zielunternehmens
* Zeitungsartikel über das Zielunternehmen ( facebook, etc. )

### Whois

Request und Response-Protokoll nach RFC 3912 ( [https://www.ietf.org/rfc/rfc3912.txt](https://www.ietf.org/rfc/rfc3912.txt) ) Port : 43

Whois liefert ausführliche Informationen über registrierte Domain.

* Registrar
* Kontaktinformationen
* Zeitstempel ( creation, update, expiration )
* Nameserver ( welcher Server zum Auflösen des Domain-Name verwendet wird )

```
$> whois url.com
```

### nslookup ( Name Server Look Up )

Auslesen des DNS-Servers&#x20;

```
$> nslookup OPTIONS DOMAIN_NAME SERVER
$> nslookup -type=A google.de 1.1.1.1    # Beispiel mit google.de auf Cloudflare DNS
```

* OPTIONS : enthält Art der Anfrage bzw. gewünschte Information ( Tabelle )
* DOMAIN\_NAME : enthält die Zieldomain
* SERVER : ist der DNS-Server, welcher durchsucht werden soll ( die Anfrage erhält ). Hier können alle öffentlichen DNS-Server angefragt werden. ( [https://duckduckgo.com/?q=public+dns](https://duckduckgo.com/?q=public+dns) )

Google : `8.8.8.8, 8.8.4.4`&#x20;

Cloudflare : `1.1.1.1, 1.0.0.1`&#x20;

Quad9 : `9.9.9.9, 149.112.112.112`&#x20;

| Query Type (options) | Bedeutung          |
| -------------------- | ------------------ |
| A                    | IPv4 Adressen      |
| AAAA                 | IPv6 Adressen      |
| CNAME                | Canonical Name     |
| MX                   | Mail Server        |
| SOA                  | Start of Authority |
| TXT                  | TXT Einträge       |

### dig ( Domain Information Groper )

Erweiterte DNS-Abfragen und zusätzliche Funktionalität

```
$> dig @SERVER DOMAIN_NAME TYPE
```

* SERVER : DNS-Server zum Durchsuchen
* DOMAIN\_NAME : zu prüfender Domain-Name
* TYPE : DNS-Record Typ der gewünscht ist

```
$> dig TryHackMe.com MX
$> dig @1.1.1.1 TryHackMe.com MX
```

### DNSDumpster ( [https://dnsdumpster.com/](https://dnsdumpster.com/) )

Onlineservice zum Durchsuchen der Informationen öffentlicher DNS-Server und grafische Darstellung.

<figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/844a43bc37547bbb13f94894605992d3.png" alt=""><figcaption><p>Darstellung einer kompletten Struktur inklusive Subdomains</p></figcaption></figure>

### Shodan.io ( [https://www.shodan.io/](https://www.shodan.io/) )

Ausführliche Informationen über ein Zielsystem, damit verbundene Geräte, etc:

* IP-Adresse
* Hosting-Anbieter
* Geographischer Standort
* Servertyp und Version
