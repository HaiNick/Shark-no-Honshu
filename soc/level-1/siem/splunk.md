# detect threats via splunk log correlation and search queries.

## core detection queries
```spl
# failed login detection
index=security EventCode=4625 | stats count by src_ip | where count > 10

# powershell execution monitoring  
index=windows source=*powershell* | rex field=_raw "(?<command>.*)" | stats count by command

# lateral movement via psexec
index=security EventCode=5145 ShareName="ADMIN$" | stats count by src_ip, dest_ip

# data exfiltration detection
index=network bytes_out > 1000000 | stats sum(bytes_out) by src_ip | where sum > 10000000

# suspicious process execution
index=endpoint process_name=*cmd.exe* OR process_name=*powershell.exe* | eval suspicious=if(match(command_line, "(whoami|net user|nltest)"), "yes", "no") | search suspicious="yes"
```

## threat hunting searches
```spl
# hunt for living off the land binaries (LOLBins)
index=endpoint (process_name=certutil.exe OR process_name=bitsadmin.exe OR process_name=regsvr32.exe)

# detect mimikatz usage
index=security (EventCode=4648 OR EventCode=4624) Logon_Type=9 | stats count by Account_Name

# find beaconing activity
index=network | bucket _time span=1h | stats count by src_ip, dest_ip, dest_port | where count > 10

# credential stuffing detection  
index=web action=login | stats dc(src_ip) as unique_ips by username | where unique_ips > 50
```

## automated alerting
```spl
# malware callback detection
index=network NOT (dest_ip=10.* OR dest_ip=172.* OR dest_ip=192.168.*) | stats count by src_ip, dest_ip | where count > 100

# privilege escalation attempts
index=security EventCode=4672 Special_Logon_Type=*SeDebugPrivilege* | stats count by Account_Name

# ransomware file activity  
index=endpoint file_extension IN (*.locked, *.crypto, *.encrypt) | stats count by hostname
```

## performance optimization
- use summary indexing for frequent searches
- limit time ranges to reduce search scope
- leverage tstats for faster statistical queries
- use lookup tables for IOC matching

## integration points
- threat intelligence feeds via lookup tables
- SOAR platforms for automated response
- email alerts for critical detections
- dashboard creation for executive reporting

[!] test all detection rules in dev environment before production deployment
(._.) high-volume logs can cause performance issues - optimize search syntax