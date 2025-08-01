# share and correlate threat intelligence via structured IOC management.

## IOC data types
```json
// IP address indicators
{
  "category": "Network activity",
  "type": "ip-dst",
  "value": "192.168.1.100",
  "comment": "C2 server for emotet campaign",
  "to_ids": true,
  "tags": ["tlp:amber", "emotet", "banking-trojan"]
}

// file hash indicators  
{
  "category": "Payload delivery",
  "type": "sha256", 
  "value": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
  "comment": "malicious document dropper",
  "to_ids": true
}

// domain indicators
{
  "category": "Network activity",
  "type": "domain",
  "value": "malicious-domain.com",
  "comment": "phishing campaign infrastructure", 
  "to_ids": true
}
```

## automated IOC feeds
```python
# python script for IOC ingestion
import pymisp
import requests

misp = pymisp.PyMISP('https://misp.company.com', 'your-api-key')

# create new event
event = misp.new_event(distribution=1, threat_level_id=2, analysis=1, info="Daily IOC Feed")

# add IOCs from threat feed
feed_url = "https://threatfeed.provider.com/api/iocs"
response = requests.get(feed_url, headers={'Authorization': 'Bearer token'})

for ioc in response.json():
    misp.add_attribute(event['Event']['id'], {
        'type': ioc['type'],
        'value': ioc['value'], 
        'category': 'Network activity',
        'to_ids': True,
        'comment': f"Source: {ioc['source']}"
    })

# publish event
misp.publish(event['Event']['id'])
```

## SIEM integration
```bash
# export IOCs for splunk
curl -k -H "Authorization: YOUR_API_KEY" \
"https://misp.company.com/attributes/restSearch/returnFormat:csv/type:ip-dst/to_ids:1" \
> /opt/splunk/etc/apps/search/lookups/misp_ips.csv

# elasticsearch integration
curl -X GET "https://misp.company.com/attributes/restSearch" \
-H "Authorization: YOUR_API_KEY" \
-H "Accept: application/json" \
-d '{"returnFormat":"json","type":["ip-dst","domain"],"to_ids":"1"}' | \
jq '.response.Attribute[] | {value: .value, type: .type}' > iocs.json
```

## automated detection rules
```yaml
# sigma rule generation from MISP
title: MISP IOC Detection
logsource:
    category: network
detection:
    selection_ip:
        dst_ip: 
            - "192.168.1.100"  # from MISP feed
            - "10.0.0.50"      # from MISP feed
    selection_domain:
        query:
            - "malicious-domain.com"
            - "c2-server.net"
    condition: selection_ip or selection_domain
level: high
```

## threat attribution
```json
// galaxy cluster for threat actor
{
  "value": "APT29",
  "description": "Russian cyber espionage group",
  "meta": {
    "synonyms": ["Cozy Bear", "The Dukes"],
    "country": "Russia",
    "motivation": "espionage"
  },
  "related": [
    {
      "dest-uuid": "899ce53f-13a0-479b-a0e4-67d46e241542",
      "type": "similar"
    }
  ]
}

// link IOCs to threat actor
{
  "category": "Attribution", 
  "type": "threat-actor",
  "value": "APT29",
  "Galaxy": [{
    "type": "mitre-attack-pattern",
    "value": "T1566.001"
  }]
}
```

## IOC validation workflow
```python
# validate IOCs before sharing
def validate_ioc(ioc_value, ioc_type):
    if ioc_type == 'ip-dst':
        # check if IP is internal/private
        import ipaddress
        ip = ipaddress.ip_address(ioc_value)
        if ip.is_private:
            return False, "Private IP address"
    
    elif ioc_type == 'domain':
        # check domain reputation
        vt_response = check_virustotal(ioc_value)
        if vt_response['positives'] < 3:
            return False, "Low confidence domain"
    
    return True, "Valid IOC"

# quality scoring
def score_ioc(attribute):
    score = 50  # base score
    
    # increase score for multiple sources
    if len(attribute.get('tags', [])) > 2:
        score += 20
    
    # increase score for recent sightings
    if attribute.get('timestamp') > (time.time() - 86400):
        score += 15
        
    return min(score, 100)
```

## collaborative analysis
```json
// proposal for attribute modification
{
  "type": "edit", 
  "attribute_id": "12345",
  "value": "updated-ioc-value.com",
  "comment": "Updated based on sandbox analysis",
  "email": "analyst@company.com"
}

// discussion thread
{
  "event_id": "67890",
  "comment": "This campaign targets financial institutions",
  "distribution": "1",
  "date": "2023-12-01"
}
```

## automated enrichment
```python
# enrich IOCs with external sources
def enrich_ioc(ioc_value, ioc_type):
    enrichment = {}
    
    if ioc_type == 'ip-dst':
        # geoip enrichment
        geo_data = geoip_lookup(ioc_value)
        enrichment['country'] = geo_data.get('country')
        enrichment['asn'] = geo_data.get('asn')
        
        # reputation check
        rep_data = check_reputation(ioc_value)
        enrichment['malicious_confidence'] = rep_data.get('score')
    
    elif ioc_type == 'domain':
        # passive DNS
        pdns_data = passive_dns_lookup(ioc_value)
        enrichment['first_seen'] = pdns_data.get('first_seen')
        enrichment['last_seen'] = pdns_data.get('last_seen')
    
    return enrichment
```

## feed synchronization
```bash
# sync with external MISP instances
misp-modules -l 127.0.0.1 -p 6666

# configure feed pulling
curl -k -X POST https://misp.company.com/feeds/add \
-H "Authorization: YOUR_API_KEY" \
-H "Content-Type: application/json" \
-d '{
  "name": "External Threat Feed",
  "url": "https://feeds.threatprovider.com/misp.json", 
  "distribution": "1",
  "default": "true"
}'
```

[!] validate IOC quality before sharing - false positives damage trust
(._.) coordinate with legal team on data sharing agreements and attribution claims

