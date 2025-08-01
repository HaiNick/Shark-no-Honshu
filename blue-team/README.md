# defend networks via monitoring and detection. spot intrusions before they spread.

## network monitoring & logging
- IDS/IPS: `snort`, `suricata` for signature-based detection
- netflow analysis: identify traffic patterns, exfiltration
- syslog centralization: aggregate logs for correlation
- packet capture: `tcpdump`, `wireshark` for deep inspection

## network segmentation
- firewall rules: block lateral movement between vlans  
- ACLs: restrict protocol access by user/system role
- network access control (NAC): authenticate before network access
- zero trust: verify every connection, assume breach

## threat hunting in network data
- scan detection: identify reconnaissance attempts
- anomaly detection: baseline normal vs suspicious traffic  
- lateral movement detection: east-west traffic analysis
- data exfiltration detection: large outbound transfers

## SIEM integration 
- centralize network logs with host/application data
- correlation rules for multi-stage attack detection
- automated response: block IPs, isolate systems
- threat intelligence feeds: known bad indicators

## authentication attack defenses
- account lockout: lock after `n` failed attempts
- rate limiting: delay authentication responses  
- multi-factor authentication: SMS/TOTP/hardware tokens
- certificate-based auth: PKI for SSH/VPN access  
- geolocation blocking: restrict login locations
- CAPTCHA: human verification for web portals

[!] password policies alone insufficient against credential stuffing
(._.) geolocation easily bypassed with VPNs, combine with other factors