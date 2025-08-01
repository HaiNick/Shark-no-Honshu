# get system and network information with powershell. example: enumerate users, check ips, find services because cmd is limited

## system information
```powershell
# basic system details
Get-ComputerInfo                    # comprehensive system info
Get-ComputerInfo | Select-Object WindowsProductName, TotalPhysicalMemory, CsProcessors
$env:COMPUTERNAME                   # computer name
$env:USERNAME                       # current user
[Environment]::OSVersion            # os version
Get-WmiObject Win32_OperatingSystem # wmi os info

# hardware information
Get-WmiObject Win32_ComputerSystem  # system manufacturer, model
Get-WmiObject Win32_Processor       # cpu information
Get-WmiObject Win32_PhysicalMemory  # memory modules
Get-WmiObject Win32_DiskDrive       # disk drives
Get-WmiObject Win32_LogicalDisk     # logical drives with space
```

## user and group enumeration
```powershell
# local users
Get-LocalUser                       # all local users
Get-LocalUser | Where-Object Enabled -eq $true  # enabled users only
Get-LocalUser -Name "administrator" # specific user
Get-WmiObject Win32_UserAccount     # wmi user accounts

# local groups
Get-LocalGroup                      # all local groups
Get-LocalGroupMember -Group "Administrators"  # admin group members
Get-LocalGroupMember -Group "Remote Desktop Users"  # rdp users
net localgroup administrators      # cmd alternative for admins

# domain information (if domain joined)
Get-ADUser -Filter *                # all domain users (requires RSAT)
Get-ADGroup -Filter *               # all domain groups
whoami /groups                      # current user groups
```

## network configuration
```powershell
# ip configuration
Get-NetIPConfiguration              # network adapter configuration
Get-NetIPAddress                    # ip addresses
Get-NetIPAddress -AddressFamily IPv4  # ipv4 only
ipconfig /all                       # classic command still works

# network adapters
Get-NetAdapter                      # network adapters
Get-NetAdapter | Where-Object Status -eq "Up"  # active adapters
Get-WmiObject Win32_NetworkAdapterConfiguration

# routing and dns
Get-NetRoute                        # routing table
Get-DnsClientServerAddress          # dns servers
nslookup google.com                 # dns lookup
Test-NetConnection google.com -Port 443  # test connectivity
```

## running processes and services
```powershell
# processes
Get-Process                         # all running processes
Get-Process | Sort-Object CPU -Descending  # by cpu usage
Get-Process -Name "notepad"         # specific process
Get-WmiObject Win32_Process         # wmi process info

# services
Get-Service                         # all services
Get-Service | Where-Object Status -eq "Running"  # running services
Get-Service -Name "spooler"         # specific service
Get-WmiObject Win32_Service         # wmi service info
sc query                           # cmd alternative
```

## network connections
```powershell
# active connections
Get-NetTCPConnection                # tcp connections
Get-NetTCPConnection -State Listen  # listening ports
Get-NetTCPConnection -LocalPort 80  # specific port
netstat -an                         # classic netstat

# network shares
Get-SmbShare                        # smb shares
net share                           # cmd alternative
Get-WmiObject Win32_Share           # wmi shares
```

## installed software
```powershell
# installed programs
Get-WmiObject Win32_Product         # installed software (slow)
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*  # registry method
Get-Package                         # package manager installed

# windows features
Get-WindowsFeature                  # server features (server only)
Get-WindowsOptionalFeature -Online  # optional features
dism /online /get-features          # dism alternative
```

## file system information
```powershell
# disk usage
Get-WmiObject Win32_LogicalDisk | Select-Object DeviceID, Size, FreeSpace
Get-PSDrive -PSProvider FileSystem # powershell drives

# file shares and permissions
Get-Acl C:\                         # file permissions
Get-ChildItem C:\ | Get-Acl         # permissions for all items
icacls C:\                          # cmd alternative
```

## event logs
```powershell
# event log enumeration
Get-EventLog -List                  # available event logs
Get-EventLog -LogName System -Newest 10  # latest system events
Get-EventLog -LogName Security | Where-Object EventID -eq 4624  # logon events
Get-WinEvent -ListLog *             # all event logs (newer)
```

## registry information
```powershell
# registry enumeration
Get-ChildItem HKLM:\Software        # software registry keys
Get-ItemProperty HKLM:\System\CurrentControlSet\Services\*  # services in registry
reg query HKLM\Software             # cmd alternative
```

## environment and variables
```powershell
# environment variables
Get-ChildItem Env:                  # all environment variables
$env:PATH                           # path variable
$env:TEMP                           # temp directory
[Environment]::GetEnvironmentVariables()  # .net method

# powershell variables
Get-Variable                        # powershell variables
$PSVersionTable                     # powershell version info
$Host                               # host information
```

## domain reconnaissance (if applicable)
```powershell
# domain information
$domain = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
$domain.Name                        # domain name
$domain.Forest                      # forest name
nltest /domain_trusts               # domain trusts

# domain controllers
$domain.DomainControllers           # domain controllers
nslookup -type=SRV _ldap._tcp.dc._msdcs.domain.com  # find dcs via dns
nltest /dclist:domain.com           # list domain controllers
```

## quick reconnaissance one-liners
```powershell
# system overview
Get-ComputerInfo | Select-Object WindowsProductName, TotalPhysicalMemory, CsName, CsDomain

# find interesting files
Get-ChildItem C:\ -Recurse -Include *.txt,*.pdf,*.docx -ErrorAction SilentlyContinue

# check for admin privileges
([Security.Principal.WindowsPrincipal] [Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole] "Administrator")

# network overview
Get-NetTCPConnection -State Listen | Select-Object LocalAddress, LocalPort, OwningProcess

# find writable directories
Get-ChildItem C:\ -Directory | Where-Object { (Get-Acl $_.FullName).Access | Where-Object { $_.FileSystemRights -match "Write" -and $_.AccessControlType -eq "Allow" } }
```

## output formatting
```powershell
# format output
Get-Process | Format-Table Name, CPU, WorkingSet  # table format
Get-Service | Format-List Name, Status           # list format
Get-Process | Out-GridView                       # gui grid view
Get-Process | Export-Csv processes.csv           # export to csv
Get-Process | ConvertTo-Json                     # convert to json
```

# TODO: add firewall rules enumeration, scheduled tasks, certificate information, wmi persistence detection