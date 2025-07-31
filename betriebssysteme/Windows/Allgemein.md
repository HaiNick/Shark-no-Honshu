# windows quick reference because corporate environments.

## check process modules
```cmd
tasklist /m /fi "pid eq 1304"
```

## user account levels
- **administrator**: full system control
- **standard user**: limited to own files/settings  
- **SYSTEM/LocalSystem**: OS internal - higher than admin privileges
- **Local Service**: service account - minimal rights, anonymous network connections
- **Network Service**: service account - minimal rights, uses computer credentials for network auth

## useful links
- powershell reference: https://www.comparitech.com/net-admin/powershell-cheat-sheet/
- command reference: `../cheat-sheets/windows-cmd.md`
- privilege escalation: `../privilege-escalation/windows-privesc.md` 
- security updates: https://learn.microsoft.com/de-de/security-updates/securitybulletins/securitybulletins

## notes
[!] SYSTEM account has more privileges than local admin
[!] Local/Network Service accounts designed for "minimal" rights but definitions vary