---
icon: magnifying-glass
---

# Recon & Initial Access

<details>

<summary>Links:</summary>

[https://dnsdumpster.com/](https://dnsdumpster.com/)

[https://juggernaut-sec.com/manual-enumeration-lpe/](https://juggernaut-sec.com/manual-enumeration-lpe/)

[https://hackertarget.com/](https://hackertarget.com/)

[https://github.com/carlospolop/legion](https://github.com/carlospolop/legion)

[https://www.kalilinux.in/2019/08/seeker-trace-mobile-location-of-anyone.html](https://www.kalilinux.in/2019/08/seeker-trace-mobile-location-of-anyone.html)

</details>

### **1. Zielerkennung**

**Info:** Host erreichbar, DNS vorhanden, Whois\
**Tools:** `ping`, `nslookup`, `dig`, `whois`

***

### **2. Port- und Dienst-Scan**

**Info:** Offene Ports, Dienste, OS-Fingerprint\
**Tools:** `nmap`, `masscan`, `xprobe2`

***

### **3. Banner Grabbing**

**Info:** Service-Versionen\
**Tools:** `netcat`, `telnet`, `curl`, `nmap -sV`

***

### **4. DNS & Zonen-Analyse**

**Info:** Interne Hosts, Subdomains, Zonentransfer\
**Tools:** `dig`, `dnsrecon`, `fierce`, `dnsenum`

***

### **5. SMB/NetBIOS (Windows)**

**Info:** Shares, Benutzer, Gruppen, SID\
**Tools:** `enum4linux`, `smbclient`, `rpcclient`, `nmap --script smb-*`

***

### **6. LDAP/AD (Windows Domain Controller)**

**Info:** Benutzer, Gruppen, OU-Struktur\
**Tools:** `ldapsearch`, `ldapdomaindump`, `GetNPUsers.py`, `crackmapexec`

***

### **7. Webdienste (beide Systeme)**

**Info:** Webserver-Version, Dateien, Schwachstellen\
**Tools:** `whatweb`, `nikto`, `gobuster`, `dirb`

***

### **8. Remote-Management**

**Info:** Fernzugriff möglich? (SSH, WinRM)\
**Tools:** `nmap`, `evil-winrm`, `ssh`, `crackmapexec`

***

### **9. Schwachstellenprüfung**

**Info:** CVEs, unsichere Dienste, Exploits\
**Tools:** `nmap --script vuln`, `OpenVAS`, `Nessus`, `searchsploit`

***

### **10. Zusatz für Windows-AD**

**Info:** Trusts, Sessions, ACLs, Privilege Paths\
**Tools:** `BloodHound`, `SharpHound`, `ADRecon`, `PowerView`

***

### **11. Benutzer-/Login-Prüfung**

**Info:** Vorhandene Benutzer, Login-Möglichkeiten, Default-Accounts\
**Tools:** `hydra`, `medusa`, `nmap --script ssh-brute/smb-brute`, `kerbrute` (AD)

***

### **12. SNMP-Analyse (falls aktiv)**

**Info:** Netzwerktopologie, Interfaces, Systemdaten\
**Tools:** `snmpwalk`, `snmpcheck`, `onesixtyone`

***

### **13. Mailserver-Analyse**

**Info:** SMTP-Konfiguration, Benutzervalidierung\
**Tools:** `swaks`, `smtp-user-enum`, `nmap --script smtp-*`

***

### **14. Virtualisierung & Cloud-Erkennung**

**Info:** Hypervisor, Azure AD, AWS-Footprint\
**Tools:** `lscpu`, `dmidecode`, Metadaten-Endpunkte prüfen

***

### **15. VPN / RAS / Tunnel-Erkennung**

**Info:** Zugangsmöglichkeiten, Protokolle, Split-Tunneling\
**Tools:** `ike-scan`, `openvpn fingerprint`, Traffic-Analyse

***

### **16. Wireless & Bluetooth (on-site oder via Dump)**

**Info:** AP-Namen, Auth-Typen, BLE-Geräte\
**Tools:** `airmon-ng`, `airodump-ng`, `bluetoothctl`, `bettercap`

***

### **17. Dateisystemfreigaben & Netzlaufwerke**

**Info:** NFS, AFP, WebDAV, iSCSI\
**Tools:** `showmount`, `nmap --script nfs-*`, `davtest`, `iscsi-cli`

***

### **18. Zeitsynchronisierung & NTP-Server**

**Info:** Zeitquelle, Angreifbarkeit durch NTP\
**Tools:** `ntpq`, `ntpdate`, `nmap --script ntp-*`

***

### **19. Physikalische Interfaces & USB (bei phys. Zugang)**

**Info:** Offene Ports, HID-Attacken möglich?\
**Tools:** `lsusb`, `usb-devices`, `hidattack`, `USB Rubber Ducky`

***

### **20. Passive Recon**

**Info:** Öffentliche Informationen, Metadaten, Subdomains\
**Tools:** `theHarvester`, `Shodan`, `crt.sh`, `recon-ng`, `Amass`
