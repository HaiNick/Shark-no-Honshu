# respond to incidents via forensic analysis and evidence collection.

## artifact collection priority
1. **volatile memory**: RAM, running processes, network connections
2. **system state**: registry, event logs, prefetch, user artifacts  
3. **file system**: disk images, file metadata, deleted files
4. **network data**: packet captures, flow records, proxy logs

## evidence preservation
```bash
# create forensic disk image
dd if=/dev/sda of=/mnt/evidence/disk_image.dd bs=4096 conv=noerror,sync
md5sum /mnt/evidence/disk_image.dd > /mnt/evidence/disk_image.md5

# memory acquisition
LiME or WinPmem for live memory capture
volatility for memory analysis

# write-protect evidence
mount -o ro,loop /mnt/evidence/disk_image.dd /mnt/analysis/
```

## timeline creation
```bash
# filesystem timeline
fls -r -m / /mnt/analysis/ > filesystem_timeline.txt
mactime -b filesystem_timeline.txt > timeline_readable.txt

# log correlation timeline  
log2timeline.py --parsers="winevtx,prefetch,webhist" timeline.plaso /mnt/analysis/
psort.py -o l2tcsv timeline.plaso > events_timeline.csv
```

## malware analysis workflow
1. **static analysis**: file hashes, strings, imports, metadata
2. **dynamic analysis**: sandbox execution, network traffic, file changes
3. **code analysis**: disassembly, reverse engineering, IOC extraction
4. **attribution**: malware family, threat actor, campaign linkage

## key forensic tools
```bash
# file analysis
file malware.exe
strings malware.exe | grep -i "http\|ftp\|\.dll"
exiftool malware.exe

# registry analysis  
rip.pl -r /mnt/analysis/Windows/System32/config/SYSTEM -p userassist
rip.pl -r /mnt/analysis/Windows/System32/config/SOFTWARE -p run

# network artifact analysis
bulk_extractor -o /tmp/output /mnt/analysis/
tshark -r capture.pcap -T fields -e ip.src -e ip.dst -e tcp.port
```

## incident response procedures
### initial response (first 30 minutes)
1. **identify scope**: affected systems, data, users
2. **contain threat**: isolate systems, disable accounts  
3. **preserve evidence**: memory dumps, disk images, logs
4. **notify stakeholders**: management, legal, customers
5. **begin documentation**: timeline, actions taken, evidence

### investigation phase (hours to days)
1. **analyze artifacts**: malware, logs, network traffic
2. **determine root cause**: attack vector, vulnerability exploited
3. **assess impact**: data exfiltrated, systems compromised
4. **track attacker TTPs**: techniques, tools, procedures used
5. **develop IOCs**: file hashes, domains, IP addresses

### recovery phase (days to weeks)  
1. **eradicate threats**: remove malware, close vulnerabilities
2. **restore systems**: clean rebuilds, backup restoration
3. **strengthen defenses**: new rules, monitoring, training
4. **validate recovery**: testing, monitoring for reinfection
5. **lessons learned**: incident review, process improvements

## chain of custody
- document all evidence handling: who, what, when, where
- use tamper-evident bags and labels
- maintain access logs for all evidence
- provide detailed transfer documentation

## common artifacts by OS
### windows
- prefetch files: recent program execution
- registry hives: persistence, configuration, user activity
- event logs: system events, security events, application logs
- user profile data: browser history, recent documents, email

### linux  
- bash history: command execution history
- log files: /var/log/* for system and application events  
- cron jobs: scheduled task persistence
- user artifacts: .ssh, .bash_profile, browser data

[!] collect volatile evidence first - memory contents lost on reboot
(._.) maintain detailed documentation - required for legal proceedings