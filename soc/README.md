# security operations center - 24/7 threat monitoring and incident response.

## core functions
- monitor security alerts from SIEM, EDR, network tools
- analyze incidents: triage, investigate, escalate
- threat hunting: proactive search for hidden threats  
- incident response: contain, eradicate, recover
- forensics: collect evidence, timeline reconstruction

## analyst tiers
- **level 1**: alert triage, basic investigation, escalation
- **level 2**: deep analysis, threat hunting, playbook creation
- **level 3**: advanced forensics, malware analysis, architecture

## essential tools
```bash
# SIEM platforms
splunk, elk stack, qradar, sentinel

# endpoint detection  
crowdstrike, sentinelone, defender ATP

# network monitoring
zeek, suricata, wireshark, networkminer

# threat intelligence
misp, opencti, virustotal, threatconnect

# forensics & IR
volatility, autopsy, sleuthkit, yara
```

## operational procedures
- playbooks for common attack scenarios
- escalation matrix: when to involve L2/L3/management
- evidence handling: chain of custody, preservation
- communication protocols: internal teams, external stakeholders

[!] SOC effectiveness depends on quality of detection rules and analyst training
(._.) alert fatigue from false positives reduces response effectiveness
