# windows operations because enterprise runs on it. reference for when AD breaks.

## user account types
- **administrator**: full system access - highest privilege
- **standard user**: limited access - own files only
- **SYSTEM/LocalSystem**: OS internal use - higher than admin
- **Local Service**: service execution - minimal rights, anonymous network
- **Network Service**: service execution - minimal rights, uses computer creds

## editions
- **Home**: basic features for consumers
- **Pro**: business features - BitLocker, group policies
- **Enterprise**: advanced security/management for large orgs
- **Education**: schools - special licensing

## filesystem
- **NTFS**: standard - permissions, encryption, large files
- **FAT32**: legacy - broad compatibility, 4GB file limit
- **exFAT**: flash storage - large file support

## key locations
- `C:\Windows\System32`: core system files, drivers, tools
- Desktop: workspace for files/shortcuts
- Taskbar: running apps + quick access
- Start Menu: programs, settings, search

## user account control (UAC)
- prompts for admin actions
- blocks unauthorized changes
- adjust in Settings > System > Security

## system interfaces
- **Settings**: modern config interface
- **Control Panel**: classic advanced settings (run `control`)

## task manager
- access: `Ctrl+Shift+Esc` or right-click taskbar
- monitor: CPU, memory, network usage
- kill unresponsive apps
- manage startup programs

## links
- powershell cheat sheet: `../../cheat-sheets/powershell.md`
- cmd reference: `../../cheat-sheets/windows-cmd.md`  
- privilege escalation: `../../killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/windows-privesc.md`
- security bulletins: https://learn.microsoft.com/de-de/security-updates/securitybulletins/securitybulletins

## notes
[!] never delete files from `System32` unless you know what you're doing
[!] UAC can be bypassed - don't rely on it alone for security