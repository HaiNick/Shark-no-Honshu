# detect endpoint threats via sysmon event monitoring and correlation.

## essential event IDs
```xml
<!-- sysmon config snippet -->
<Sysmon schemaversion="4.21">
  <EventFiltering>
    <!-- process creation -->
    <ProcessCreate onmatch="include">
      <Image condition="contains">powershell</Image>
      <Image condition="contains">cmd</Image>
      <CommandLine condition="contains">-enc</CommandLine>
    </ProcessCreate>
    
    <!-- network connections -->
    <NetworkConnect onmatch="include">
      <DestinationPort condition="is">443</DestinationPort>
      <DestinationPort condition="is">80</DestinationPort>
    </NetworkConnect>
    
    <!-- file creation -->
    <FileCreate onmatch="include">
      <TargetFilename condition="contains">temp</TargetFilename>
      <TargetFilename condition="endswith">.exe</TargetFilename>
    </FileCreate>
  </EventFiltering>
</Sysmon>
```

## key detection queries
```powershell
# suspicious process execution (event ID 1)
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=1} | 
Where-Object {$_.Message -match "powershell.*-enc|cmd.*whoami|certutil.*download"}

# network beaconing detection (event ID 3)
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=3} |
Where-Object {$_.Message -match "DestinationPort.*443" -and $_.Message -notmatch "firefox|chrome"}

# lateral movement via psexec (event ID 13)
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=13} |
Where-Object {$_.Message -match "HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run"}
```

## malware detection patterns
```bash
# process hollowing detection
EventID=1 AND (ParentImage=*svchost.exe OR ParentImage=*explorer.exe) AND Image=*\\Windows\\System32\\*

# mimikatz execution
EventID=1 AND (CommandLine=*sekurlsa* OR CommandLine=*lsadump* OR CommandLine=*kerberos*)

# living off the land binaries
EventID=1 AND (Image=*certutil.exe OR Image=*bitsadmin.exe OR Image=*regsvr32.exe)

# WMI persistence
EventID=1 AND Image=*wmiprvse.exe AND CommandLine=*powershell*
```

## network communication monitoring
```yaml
# C2 beacon detection
event_id: 3
destination_port: [80, 443, 8080, 8443]
frequency: >5 connections per minute
exclude_processes: [firefox.exe, chrome.exe, iexplore.exe]

# data exfiltration detection  
event_id: 3
destination_port_name: [http, https, ftp]
upload_size: >100MB
exclude_processes: [backup.exe, sync.exe]
```

## file system monitoring
```sql
-- suspicious file creation
SELECT * FROM sysmon_events 
WHERE event_id = 11 
AND (target_filename LIKE '%.tmp.exe' 
     OR target_filename LIKE '%\\temp\\%' 
     OR target_filename LIKE '%.scr')

-- registry persistence
SELECT * FROM sysmon_events
WHERE event_id = 13 
AND target_object LIKE '%CurrentVersion\\Run%'
AND details NOT LIKE '%Program Files%'
```

## response procedures
1. **isolate system**: disconnect from network if malware confirmed
2. **collect artifacts**: memory dump, disk image, process list
3. **analyze timeline**: correlate events across multiple systems
4. **hunt for persistence**: check startup locations, services, tasks
5. **validate cleanup**: ensure malware completely removed

[!] sysmon generates high volume logs - tune configuration to reduce noise
(._.) process injection techniques may bypass sysmon detection

