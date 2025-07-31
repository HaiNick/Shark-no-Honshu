# passive reconnaissance — gather intel without touching their systems

everything you can learn from public sources without directly interacting with the target. they shouldn't know you exist.

## typical activities

* DNS records from public servers
* job postings from target company  
* news articles, social media about the organization
* public filing, SEC documents, press releases
* employee LinkedIn profiles, conference talks

## whois lookups

RFC 3912 protocol on port 43. gives you domain registration details.

```bash
whois target.com
```

returns:
* registrar information
* contact details (often privacy-protected now)
* creation, update, expiration dates
* nameservers handling DNS for the domain

## nslookup queries

query DNS servers for various record types

```bash
nslookup -type=A google.com 1.1.1.1
nslookup -type=MX target.com 8.8.8.8
nslookup -type=TXT company.com
```

useful public DNS servers:
* cloudflare: `1.1.1.1`, `1.0.0.1`
* google: `8.8.8.8`, `8.8.4.4`

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
