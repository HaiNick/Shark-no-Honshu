# detect threats via windows event log correlation and analysis.

## critical security events
```powershell
# account logon events
4624 - successful logon (monitor for unusual times/locations)
4625 - failed logon (brute force detection)  
4634 - account logged off
4647 - user initiated logoff
4648 - logon with explicit credentials (runas, psexec)

# account management
4720 - user account created
4722 - user account enabled
4724 - password reset attempt
4738 - user account changed
4740 - account locked out

# privilege escalation
4672 - special privileges assigned to new logon
4673 - privileged service called
4674 - operation performed on privileged object

# process/object access
4688 - new process created
4689 - process terminated  
4698 - scheduled task created
4699 - scheduled task deleted
```

## detection queries
```powershell
# brute force detection
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625} | 
Group-Object -Property {$_.Properties[19].Value} | 
Where-Object {$_.Count -gt 10} | 
Select-Object Name, Count

# privilege escalation monitoring
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4672} |
Where-Object {$_.Message -match "SeDebugPrivilege|SeTcbPrivilege|SeBackupPrivilege"}

# lateral movement detection
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4648} |
Where-Object {$_.Properties[5].Value -ne $_.Properties[1].Value}

# suspicious process creation
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4688} |
Where-Object {$_.Message -match "powershell.*-enc|cmd.*net user|wmic.*process"}
```

## log correlation rules
```yaml
# credential stuffing attack
rule_name: credential_stuffing
events:
  - event_id: 4625
    threshold: 50 failures
    timeframe: 5 minutes
    groupby: source_ip
alert: high

# golden ticket usage  
rule_name: golden_ticket
events:
  - event_id: 4624
    logon_type: 3
    authentication_package: kerberos
    workstation_name: ""
alert: critical

# pass the hash attack
rule_name: pass_the_hash  
events:
  - event_id: 4624
    logon_type: 3
    logon_process: NtLmSsp
    authentication_package: NTLM
alert: high
```

## powershell logging
```powershell
# enable detailed powershell logging
Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" -Name "EnableScriptBlockLogging" -Value 1

# monitor powershell events
Get-WinEvent -FilterHashtable @{LogName='Microsoft-Windows-PowerShell/Operational'; ID=4104} |
Where-Object {$_.Message -match "invoke-expression|downloadstring|bypass|hidden"}
```

## automated response
```batch
# disable compromised account
net user [username] /active:no

# isolate system from network
netsh advfirewall set allprofiles state on
netsh advfirewall set allprofiles firewallpolicy blockinbound,blockoutbound

# collect evidence
wevtutil epl Security C:\forensics\security.evtx
wevtutil epl System C:\forensics\system.evtx
```

## log retention recommendations
- security logs: 90+ days minimum
- system logs: 30+ days minimum  
- application logs: 30+ days minimum
- setup/forward logs: 7+ days minimum

## common blind spots
- event log clearing (event 1102)
- service installation without logging
- powershell execution policy bypass
- WMI event subscriptions

[!] enable verbose logging on critical systems - audit policy changes
(._.) windows event forwarding (WEF) centralizes collection from multiple hosts

