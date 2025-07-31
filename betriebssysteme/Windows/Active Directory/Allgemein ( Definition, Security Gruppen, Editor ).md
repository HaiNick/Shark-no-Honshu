# active directory domain service because enterprise authentication.

## core components
**AD DS** = central catalog for domain objects (users, computers, groups)

### object types
- **users**: people + service accounts (security principals)
- **computers**: domain-joined machines with auto-generated passwords (name + $)
- **security groups**: permission containers - inherit rights by membership

## default security groups
```
Domain Admins        # full domain control (including DCs)
Server Operators     # DC management, no admin group changes
Backup Operators     # access all files regardless of permissions
Account Operators    # create/modify user accounts
Domain Users         # all domain users
Domain Computers     # all domain computers  
Domain Controllers   # all DCs in domain
```

## default containers
```
Builtin              # standard Windows groups
Computers            # new computer accounts land here
Domain Controllers   # DC objects
Users                # default user/group location
Managed Service Accounts    # service account storage
```

## management interface
**Active Directory Users and Computers** (ADUC):
- manage users, groups, computers
- organized in **Organizational Units (OUs)**
- OUs = containers for grouping objects with similar policy needs
- users can only be in one OU at a time

## organizational units (OUs)
- group objects by policy requirements (sales vs IT)
- different group policies per OU
- hierarchy for delegation of administration

## notes
[!] **Domain Admins** = keys to the kingdom - protect these accounts
[!] **Backup Operators** can read any file - often overlooked privesc vector
[!] computer accounts (name$) are security principals too
[!] service accounts often have elevated privileges - attractive targets