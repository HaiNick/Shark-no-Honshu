# HTTP/S

### Standard-Request-Header

| Header                      | Beschreibung                                               |
| --------------------------- | ---------------------------------------------------------- |
| `Host`                      | Zielhost (v.a. für Virtual Host Routing)                   |
| `User-Agent`                | Client-Kennung (Manipulation zur Filterumgehung, Spoofing) |
| `Accept`                    | Vom Client unterstützte Medientypen                        |
| `Accept-Language`           | Bevorzugte Sprache (z. B. für LFI-Bypass)                  |
| `Accept-Encoding`           | Komprimierungsmethoden (`gzip`, `deflate`, etc.)           |
| `Referer`                   | Ursprungsseite, oft zur Autorisierung verwendet            |
| `Origin`                    | Ursprung der Anfrage (relevant bei CORS)                   |
| `Connection`                | `keep-alive` oder `close`                                  |
| `Upgrade-Insecure-Requests` | Forciert HTTPS oder klärt Umgebungsdetails                 |

***

### Authentifizierungs-/Session-Header

| Header                  | Beschreibung                                                  |
| ----------------------- | ------------------------------------------------------------- |
| `Authorization`         | `Basic`, `Bearer`, `Digest`, `NTLM`, `Negotiate`, etc.        |
| `Cookie`                | Sitzungstoken, CSRF-Token, App-Zustände                       |
| `X-Csrf-Token`          | Für CSRF-Schutzmechanismen                                    |
| `X-Api-Key`             | API-Zugriffsschlüssel                                         |
| `Set-Cookie` (Response) | Relevant für Session-Fingerprinting und Secure/HttpOnly-Audit |

***

### Proxy-/Spoofing-relevante Header

| Header                       | Beschreibung                                                           |
| ---------------------------- | ---------------------------------------------------------------------- |
| `X-Forwarded-For`            | Ursprungs-IP-Adresse (oft vertrauenswürdig)                            |
| `X-Forwarded-Host`           | Angeforderter Hostname                                                 |
| `X-Forwarded-Proto`          | Schema (`http` / `https`)                                              |
| `X-Real-IP`                  | Alternative Client-IP                                                  |
| `Forwarded`                  | Standardisierter Header: `for=192.0.2.43; proto=http; by=203.0.113.43` |
| `Client-IP`                  | Client-IP (nicht standardisiert, aber von manchen Servern akzeptiert)  |
| `True-Client-IP`             | Von CDNs verwendet                                                     |
| `Via`                        | Proxy-Informationen                                                    |
| `X-Original-URL`             | Teilweise zur URL-Manipulation nutzbar                                 |
| `X-Host`, `X-Originating-IP` | Teilweise intern vom Backend ausgewertet                               |

***

### Methodenmanipulation / Debug / Interne Infos

| Header                      | Beschreibung                                                |
| --------------------------- | ----------------------------------------------------------- |
| `X-HTTP-Method-Override`    | Ermöglicht Methoden-Tunneling über POST (z. B. DELETE, PUT) |
| `X-Request-ID`              | Interne Request-Trace-ID                                    |
| `X-Debug`                   | Aktiviert Debug-Logik (falls falsch konfiguriert)           |
| `X-Original-Method`         | Ursprüngliche Methode bei Proxys                            |
| `X-Custom-IP-Authorization` | Teilweise zur Authentifizierung (Custom-Lösungen)           |

***

### Security-relevante Response-Header

| Header                             | Bedeutung                                                           |
| ---------------------------------- | ------------------------------------------------------------------- |
| `Content-Security-Policy`          | Schutz vor XSS, JS-Injection                                        |
| `Strict-Transport-Security`        | HTTPS-Only durchsetzbar                                             |
| `X-Content-Type-Options`           | MIME-Sniffing verhindern                                            |
| `X-Frame-Options`                  | Schutz vor Clickjacking                                             |
| `X-XSS-Protection`                 | Legacy-XSS-Filter-Steuerung                                         |
| `Access-Control-Allow-Origin`      | CORS-Konfiguration (prüfen auf `*` oder fehlerhafte Origin-Prüfung) |
| `Access-Control-Allow-Credentials` | Relevanz bei CORS mit Session-Cookies                               |
| `Referrer-Policy`                  | Kontrolle über Weitergabe des Referrers                             |

***

### Datei- und Pfadmanipulation

* `Range: bytes=0-1024`\
  → Test auf **unsichere Dateiübertragung**, LFI mit `Range`
* `Content-Disposition` (Response)\
  → Dateinamen und Download-Handling prüfen
* URL-Encoding, Unicode-Encoding, Double-Encoding\
  → `%2e%2e/`, `%c0%ae%c0%ae/`, `%252e%252e/`

***

### Caching- und Timing-Header

| Header              | Beschreibung                                             |
| ------------------- | -------------------------------------------------------- |
| `If-Modified-Since` | Caching-Verhalten beobachten                             |
| `If-None-Match`     | In Verbindung mit `ETag`                                 |
| `Cache-Control`     | `no-cache`, `private`, etc. (auch bei Response relevant) |
| `ETag` (Response)   | Versionskontrolle, kann Information leaken               |
| `Expires`           | Ablaufdatum von Caches                                   |

***

### HTTP Method Override / Extended Techniques

* `POST` mit Header: `X-HTTP-Method-Override: DELETE`
* `HEAD`, `TRACE`, `OPTIONS` verwenden für Erkennung und Enumeration
* `CONNECT` und `PUT` prüfen (z. B. für SSRF / WebDAV)

***

### Beispiel für vollständige `curl`-Manipulation:

```bash
curl -X GET http://target.local/flag.txt \
  -H "User-Agent: InternalScanner" \
  -H "X-Forwarded-For: 127.0.0.1" \
  -H "X-Host: admin.target.local" \
  -H "Authorization: Bearer test123" \
  -H "Referer: https://trusted.local/dashboard" \
  -H "X-Original-URL: /admin"
```
