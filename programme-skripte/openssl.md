# openssl



### Anzeige des Hash-Werts (fingerprint) eines Zertifikates

```
openssl x509 -fingerprint -sha256 -noout -in /path/to/elastic-stack-ca.crt.pem
```

### Anzeige Hash-Wert einer Schnittstelle

{% code overflow="wrap" %}
```
openssl s_client -connect localhost:9200 -servername localhost -showcerts </dev/null 2>/dev/null | openssl x509 -fingerprint -sha256 -noout -in /dev/stdin
```
{% endcode %}



