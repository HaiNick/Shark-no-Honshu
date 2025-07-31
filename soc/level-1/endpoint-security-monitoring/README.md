# detect endpoint threats via host-based monitoring and behavioral analysis.

## core detection technologies
- **EDR platforms**: crowdstrike, sentinelone, carbon black
- **system monitoring**: sysmon, wazuh, osquery  
- **log analysis**: windows event logs, syslog, auditd
- **process monitoring**: sysinternals, process hacker
- **file integrity**: tripwire, aide, samhain

## essential monitoring areas
- process execution: command lines, parent-child relationships
- network connections: outbound traffic, C2 communication
- file system changes: malware drops, persistence mechanisms
- registry modifications: startup entries, configuration changes
- user activity: logons, privilege escalation, lateral movement

## deployment strategy
1. **critical systems first**: domain controllers, file servers
2. **user workstations**: high-risk users, executives, IT staff
3. **servers**: web, database, email, application servers
4. **remote access**: VPN endpoints, RDP gateways
5. **cloud workloads**: EC2, virtual machines, containers

[!] endpoint detection requires agent deployment and maintenance
(._.) encrypted malware and fileless attacks may evade traditional detection

