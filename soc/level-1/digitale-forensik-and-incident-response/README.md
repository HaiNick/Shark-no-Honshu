# respond to incidents via forensic investigation and evidence analysis.

## core DFIR capabilities
- **incident response**: contain, eradicate, recover from security incidents
- **digital forensics**: collect and analyze evidence from compromised systems
- **malware analysis**: reverse engineer malicious code and techniques
- **threat hunting**: proactive search for hidden threats in environment
- **timeline analysis**: reconstruct attack progression and impact

## evidence collection priorities
1. **volatile data**: memory, running processes, network connections
2. **system state**: registry, event logs, file system metadata
3. **disk images**: full forensic copies for offline analysis
4. **network data**: packet captures, flow records, proxy logs
5. **application logs**: database, web server, custom application events

## essential forensic tools
- **autopsy**: disk image analysis and artifact recovery
- **volatility**: memory forensics and malware detection
- **kape**: artifact collection and parsing automation
- **redline**: endpoint analysis and IOC detection
- **velociraptor**: distributed endpoint investigation
- **thehive**: incident response case management

## investigation workflow
1. **initial triage**: scope incident and preserve volatile evidence
2. **evidence acquisition**: collect disk images, memory dumps, logs
3. **timeline creation**: correlate events across multiple systems
4. **malware analysis**: understand attack tools and techniques
5. **impact assessment**: determine data/systems compromised
6. **attribution**: link to known threat actors or campaigns

## artifact analysis focus
- **windows**: registry, prefetch, event logs, user profiles
- **linux**: bash history, log files, cron jobs, user artifacts  
- **network**: DNS queries, HTTP traffic, file transfers
- **email**: phishing attachments, malicious links, account compromise

[!] preserve evidence integrity - use write-blocked copies for analysis
(._.) legal requirements may dictate specific collection and handling procedures