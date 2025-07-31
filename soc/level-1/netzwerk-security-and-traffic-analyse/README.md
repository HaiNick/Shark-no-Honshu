# analyze network traffic for threat detection and incident investigation.

## core analysis tools
- **wireshark**: interactive packet analysis and protocol decoding
- **zeek**: network monitoring and protocol analysis framework  
- **snort**: signature-based intrusion detection and prevention
- **networkminer**: network forensics and packet reconstruction
- **tcpflow**: TCP stream reconstruction and content extraction

## traffic analysis techniques
- **protocol analysis**: decode and inspect application layer protocols
- **flow analysis**: identify communication patterns and data volumes
- **behavioral analysis**: detect anomalies in network communication
- **content inspection**: extract files, credentials, and malicious payloads
- **timeline analysis**: reconstruct network events and attack progression

## detection focus areas
- **C2 communication**: command and control traffic patterns
- **data exfiltration**: large uploads, DNS tunneling, encrypted channels
- **lateral movement**: east-west traffic, admin share access, remote execution
- **reconnaissance**: port scans, service enumeration, vulnerability probes
- **malware delivery**: exploit kits, drive-by downloads, email attachments

## automated monitoring
- **netflow analysis**: identify top talkers and traffic patterns
- **DNS monitoring**: detect malicious domains and tunneling attempts
- **TLS inspection**: analyze certificate anomalies and encrypted traffic
- **HTTP analysis**: inspect web traffic for threats and data loss
- **email security**: monitor SMTP traffic for phishing and malware

## incident response applications
- **network forensics**: reconstruct attack timeline from packet captures
- **IOC validation**: confirm presence of threat indicators in traffic
- **scope determination**: identify all affected systems and data flows
- **attribution**: analyze traffic patterns for threat actor identification
- **containment support**: block malicious IPs and domains

[!] encrypted traffic limits deep packet inspection - focus on metadata analysis
(._.) high-volume networks require sampling and flow-based analysis for performance

