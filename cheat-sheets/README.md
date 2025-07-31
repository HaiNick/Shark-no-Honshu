# cheat sheets for operators who need commands at 3am. example: copy/paste ready attack chains, no explanations, just working code

## core tool references
- [curl](curl.md) - http requests with curl because wget sucks for apis
- [git](git.md) - manage git repos with version control commands
- [docker](docker.md) - run containers because vms are overkill
- [kerberos](kerberos.md) - exploit kerberos authentication with ticket attacks
- [telnet](telnet.md) - test network services because netcat isn't always installed
- [windows-cmd](windows-cmd.md) - windows command line operations because powershell isn't always available
- [linux-forensik](linux-forensik.md) - investigate linux systems for incident response
- [privilege-escalation](privilege-escalation.md) - escalate privileges on compromised systems
- [links](links.md) - reference links organized by category

## specialized references by category

### bash scripting and string manipulation
- [bash/](bash/) - bash command references and string operations
- [bash/manipulation-von-strings](bash/manipulation-von-strings.md) - extract, search, replace text

### linux command references
- [linux-cmd/](linux-cmd/) - linux system administration commands
- [linux-cmd/zusatz](linux-cmd/zusatz.md) - additional linux utilities

### powershell operations
- [powershell/](powershell/) - windows powershell command references
- [powershell/system-netzwerkinformationen](powershell/system-netzwerkinformationen.md) - system and network info
- [powershell/remoting-wsman-winrm](powershell/remoting-wsman-winrm.md) - remote powershell access
- [powershell/filtering-sorting-per-pipe](powershell/filtering-sorting-per-pipe.md) - data processing
- [powershell/scripting](powershell/scripting.md) - powershell automation
- [powershell/echtzeitanalyse](powershell/echtzeitanalyse.md) - real-time analysis

### assembly and reverse engineering
- [assembler/](assembler/) - assembly language references
- [assembler/x86-64-architektur](assembler/x86-64-architektur.md) - x86-64 instruction set
- [reverseengineering/](reverseengineering/) - reverse engineering tools and techniques

## usage notes
- all commands tested and working
- copy/paste ready - no modification needed
- warnings marked with [!] for dangerous operations
- gotchas marked with (._.) for common mistakes
- TODO sections show missing coverage areas

## style guide applied
- lowercase unless proper names/commands need caps
- no fluff, intros, or explanatory text beyond necessity  
- blunt, clear, immediately usable
- start each file with quick purpose line
- flat command lists, tables, one-liners
- treat as operator quick reference - not tutorials
- dangerous commands marked plainly
- ascii over emojis for warnings
