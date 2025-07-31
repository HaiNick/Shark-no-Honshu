# map attacks to detection rules via MITRE ATT&CK framework.

## initial access techniques
```yaml
# T1566.001 - spearphishing attachment
detection:
  - email gateway: scan attachments for malware
  - endpoint: monitor process creation from email clients
  - user training: report suspicious emails
query: "ParentImage=*outlook.exe AND Image=*.exe AND CommandLine=*temp*"

# T1190 - exploit public-facing application  
detection:
  - web application firewall: block exploit attempts
  - vulnerability scanning: identify exposed services
  - log monitoring: unusual http requests, error codes
query: "response_code=500 AND request_uri=*../../* OR request_uri=*cmd*"

# T1078 - valid accounts
detection:
  - unusual login times/locations
  - multiple failed authentication
  - privilege escalation attempts  
query: "EventID=4624 AND LogonType=10 AND TimeGenerated=*02:00:00*"
```

## execution techniques
```yaml
# T1059.001 - powershell
detection:
  - process monitoring: powershell with suspicious arguments
  - script block logging: decode obfuscated commands
  - network monitoring: powershell downloading content
query: "Image=*powershell.exe AND (CommandLine=*-enc* OR CommandLine=*bypass* OR CommandLine=*hidden*)"

# T1053.005 - scheduled task/job
detection:
  - event log monitoring: task creation/modification
  - file system monitoring: new executables in system paths
  - network monitoring: scheduled network connections
query: "EventID=4698 OR (EventID=1 AND Image=*schtasks.exe*)"
```

## persistence techniques  
```yaml
# T1547.001 - registry run keys
detection:
  - registry monitoring: modifications to run keys
  - process monitoring: unexpected startup programs
  - file system monitoring: new executables in startup folders
query: "EventID=13 AND TargetObject=*\\CurrentVersion\\Run*"

# T1543.003 - windows service
detection:
  - service creation events: new services installed
  - process monitoring: services executing unusual binaries
  - network monitoring: services making network connections
query: "EventID=7045 OR (EventID=1 AND ParentImage=*services.exe*)"
```

## defense evasion techniques
```yaml
# T1027 - obfuscated files or information
detection:
  - entropy analysis: high entropy indicates obfuscation
  - string analysis: base64 encoded powershell commands
  - behavior analysis: process injection, code unpacking
query: "CommandLine=*base64* OR CommandLine=*[Convert]::FromBase64String*"

# T1070.001 - indicator removal on host
detection:
  - event log clearing: security log cleared events
  - file deletion monitoring: system file deletions
  - registry monitoring: security setting modifications  
query: "EventID=1102 OR (EventID=4663 AND ObjectName=*Security.evtx*)"
```

## credential access techniques
```yaml
# T1003.001 - LSASS memory
detection:
  - process access monitoring: unusual lsass.exe access
  - tool detection: mimikatz, procdump signatures
  - privilege monitoring: debug privilege usage
query: "EventID=10 AND TargetImage=*lsass.exe* AND CallTrace=*unknown*"

# T1110 - brute force
detection:
  - failed authentication events: multiple failures
  - account lockout events: repeated lockouts
  - time-based analysis: rapid authentication attempts
query: "EventID=4625 AND Account_Name=* | stats count by Account_Name | where count > 10"
```

## lateral movement techniques
```yaml
# T1021.002 - SMB/Windows admin shares
detection:
  - network monitoring: connections to admin shares
  - authentication monitoring: admin share access
  - process monitoring: remote service execution
query: "EventID=5140 AND ShareName=*$ AND IpAddress!=*127.0.0.1*"

# T1047 - windows management instrumentation  
detection:
  - WMI event monitoring: process creation via WMI
  - network monitoring: WMI remote connections
  - command line monitoring: WMI tool usage
query: "EventID=1 AND (Image=*wmic.exe* OR ParentImage=*wmiprvse.exe*)"
```

## detection rule templates
```sigma
# generic powershell obfuscation
title: Obfuscated PowerShell Command
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    Image|endswith: '\powershell.exe'
    CommandLine|contains:
      - ' -e '
      - ' -enc '
      - ' -encoded '
      - 'FromBase64String'
  condition: selection
level: medium
```

## implementation priorities
1. **high-impact techniques**: credential access, lateral movement
2. **common techniques**: powershell, scheduled tasks, registry persistence  
3. **evasion techniques**: obfuscation, log clearing, process injection
4. **exfiltration techniques**: data staging, encrypted channels

[!] focus detection on techniques most relevant to your environment
(._.) MITRE framework updates regularly - review new techniques quarterly

