# detect network intrusions via packet inspection and signature matching.

## detection methods
- signature-based: match known attack patterns
- anomaly-based: identify deviation from baseline traffic
- hybrid: combine signatures with behavioral analysis

## deployment modes
- network IDS (NIDS): monitor network segments via span ports
- host IDS (HIDS): monitor individual system activity
- inline IPS: block malicious traffic in real-time

## common tools
- `snort`: signature-based detection with custom rules
- `suricata`: multi-threaded IDS/IPS with lua scripting  
- `zeek` (bro): protocol analysis and traffic logging
- `ossec`: host-based intrusion detection

## detection rules
```bash
# snort rule example - detect nmap scan
alert tcp any any -> $HOME_NET any (msg:"NMAP scan detected"; flags:S; detection_filter:track by_src, count 10, seconds 60; sid:1000001;)

# suricata rule - detect DNS tunneling
alert dns any any -> any any (msg:"DNS tunneling detected"; dns.query; content:".example.com"; sid:2000001;)
```

## response actions
- log events: timestamp, source, destination, payload
- alert security team: automated notifications
- block traffic: firewall integration for active response
- isolate systems: VLAN changes for containment

[!] signature-based detection misses zero-day attacks
(._.) high false positive rates require tuning and validation
