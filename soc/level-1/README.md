# level 1 SOC analyst - first line alert triage and basic incident response.

## primary responsibilities
- monitor SIEM dashboards for security alerts
- triage incidents: validate, categorize, prioritize
- basic investigation: gather initial evidence
- escalate complex threats to level 2 analysts
- document findings in ticketing system

## essential skills
- log analysis: recognize attack patterns in events
- network basics: understand protocols, traffic flow
- windows/linux fundamentals: processes, files, registry
- incident handling: follow established playbooks
- communication: clear reporting to stakeholders

## core tools
```bash
# SIEM platforms
splunk, elastic, qradar, sentinel

# endpoint detection
crowdstrike falcon, sentinelone, windows defender

# network analysis  
wireshark, zeek logs, netflow data

# threat intelligence
virustotal, misp feeds, abuse.ch

# ticketing systems
servicenow, jira, remedy
```

## investigation process
1. **validate alert**: check for false positive indicators
2. **gather context**: affected systems, users, timeframe  
3. **assess impact**: data, systems, business operations
4. **contain threat**: isolate systems if needed
5. **document findings**: who, what, when, where, why
6. **escalate or close**: based on complexity and impact

## escalation criteria
- malware confirmed on critical systems
- evidence of data exfiltration  
- lateral movement detected
- unknown attack techniques
- business-critical systems affected

[!] follow playbooks exactly - document deviations and rationale
(._.) when in doubt, escalate - better safe than sorry