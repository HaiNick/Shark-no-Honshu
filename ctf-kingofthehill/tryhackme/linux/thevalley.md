---
description: https://tryhackme.com/room/valleype
---

# TheValley

## nmap

```
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
37370/tcp open  ftp     vsftpd 3.0.3
Service Info: OS: Unix
```



```
---- Scanning URL: http://10.10.230.85/ ----
==> DIRECTORY: http://10.10.230.85/gallery/                                    
+ http://10.10.230.85/index.html (CODE:200|SIZE:1163)                          
==> DIRECTORY: http://10.10.230.85/pricing/                                    
+ http://10.10.230.85/server-status (CODE:403|SIZE:277)                        
==> DIRECTORY: http://10.10.230.85/static/                                     
                                                                               
---- Entering directory: http://10.10.230.85/static/ ----
+ http://10.10.230.85/static/00 (CODE:200|SIZE:127)                            
+ http://10.10.230.85/static/1 (CODE:200|SIZE:2473315)                         
+ http://10.10.230.85/static/10 (CODE:200|SIZE:2275927)                        
+ http://10.10.230.85/static/11 (CODE:200|SIZE:627909)                         
+ http://10.10.230.85/static/12 (CODE:200|SIZE:2203486)                        
+ http://10.10.230.85/static/13 (CODE:200|SIZE:3673497)                        
+ http://10.10.230.85/static/14 (CODE:200|SIZE:3838999)                        
+ http://10.10.230.85/static/15 (CODE:200|SIZE:3477315)                        
+ http://10.10.230.85/static/2 (CODE:200|SIZE:3627113)                         
+ http://10.10.230.85/static/3 (CODE:200|SIZE:421858)                          
+ http://10.10.230.85/static/4 (CODE:200|SIZE:7389635)                         
+ http://10.10.230.85/static/5 (CODE:200|SIZE:1426557)                         
+ http://10.10.230.85/static/6 (CODE:200|SIZE:2115495)                         
+ http://10.10.230.85/static/7 (CODE:200|SIZE:5217844)                         
+ http://10.10.230.85/static/8 (CODE:200|SIZE:7919631)                         
+ http://10.10.230.85/static/9 (CODE:200|SIZE:1190575) 
```

## User

```
J
RP
```

## Hidden

```
# devNotes
<ip>/static/00

# Login
/dev1243224123123

# Hardcoded Username und Passwort in <ip>/dev1243224123123/dev.js
siemDev:california

# Versteckte Notizen in <ip>/dev1243224123123/devNotes37370.txt
info über reusing cred. + patching vernachlässigt
```

## FTP

```
ftp $IP 37370
+> login als siemDev erfolgreich
- Wireshark-Captures gefunden

# siemFTP.pcapng
AnnualReport.txt
BusinessReport.txt
CISOReport.txt
HrReport.txt
ItReport.txt
SecurityReport.txt

# siemHTTP1.pcapng



# siemHTTP2.pcapng

```









