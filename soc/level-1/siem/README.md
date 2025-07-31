# correlate security events via centralized log analysis and alerting.

## core SIEM platforms
- **splunk**: commercial platform with SPL query language
- **elastic (ELK)**: open source with elasticsearch backend  
- **qradar**: IBM enterprise SIEM with flow analysis
- **sentinel**: microsoft cloud-native SIEM solution

## essential functions
- log aggregation: collect events from all security tools
- correlation rules: link related events across systems
- threat detection: identify attack patterns and IOCs
- incident response: automate containment and alerting
- compliance reporting: generate audit trails and reports

## deployment priorities
1. **high-value systems**: domain controllers, critical servers
2. **network perimeter**: firewalls, IDS/IPS, web proxies  
3. **user endpoints**: workstations, mobile devices
4. **cloud infrastructure**: AWS/Azure/GCP security logs
5. **application logs**: custom apps, databases, web servers

[!] SIEM effectiveness depends on quality of log sources and correlation rules
(._.) alert fatigue from poor tuning reduces analyst response effectiveness

