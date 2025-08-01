# level 2 SOC analyst - advanced investigation and threat hunting.

## primary responsibilities  
- deep dive investigation of escalated incidents
- threat hunting: proactive search for hidden threats
- playbook development: create procedures for new attack types
- malware analysis: static and dynamic analysis
- mentor level 1 analysts on complex cases

## advanced skills
- malware reverse engineering: understand attack techniques
- memory forensics: analyze volatile artifacts
- network forensics: reconstruct attack timeline  
- script analysis: powershell, bash, python payloads
- threat intelligence: IOC development and sharing

## specialized tools
```bash
# malware analysis
ida pro, ghidra, x64dbg, wireshark

# memory forensics  
volatility, rekall, windbg

# network forensics
zeek, networkminer, tcpreplay, bro

# threat hunting
osquery, velociraptor, sigma rules

# scripting & automation
python, powershell, bash, yara rules
```

## investigation methodology
1. **incident scoping**: determine full attack timeline
2. **artifact collection**: memory dumps, disk images, logs
3. **timeline analysis**: correlate events across systems
4. **attribution**: link to known threat groups/techniques
5. **impact assessment**: data compromised, systems affected
6. **remediation guidance**: provide containment strategies

## threat hunting techniques
- hypothesis-driven: test specific attack scenarios
- IOC hunting: search for known bad indicators
- behavioral analysis: identify suspicious patterns
- anomaly detection: statistical deviation from baseline

## playbook creation
- document successful investigation procedures
- include detection queries and response actions  
- specify escalation criteria and decision points
- provide troubleshooting for common issues

[!] thorough documentation enables knowledge transfer to other analysts
(._.) balance speed with accuracy - rushing leads to missed evidence

