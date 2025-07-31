---
description: Quick Links
cover: >-
  https://images.unsplash.com/photo-1526374965328-7f61d4dc18c5?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw5fHxtYXRyaXh8ZW58MHx8fHwxNzQ3MzQ3NjYxfDA&ixlib=rb-4.1.0&q=85
coverY: 0
---

# SHARK NO HONSHU

Documentation aggregated and curated by shift000 — https://github.com/shift000

# aggregated pentesting logs, exploit chains, and operational notes

these are working notes. not tutorials, not guides, not polished documentation. assume you know your shell, know how to read docs, and can figure out the gaps. when something is broken or cursed, it's marked clearly. when something might burn your lab down, there's a warning.

structure is loose but logical: basics first, then offense, then defense. cheat sheets everywhere because nobody remembers syntax at 3 AM. ctf writeups because they're actual working examples.

<details>

<summary>systems & fundamentals</summary>

# jump into system internals and core commands

[linux](betriebssysteme/linux/ "mention")

* [bash](cheat-sheets/bash/ "mention") | [bash-commands](betriebssysteme/linux/bash-befehle/ "mention")
* [linux-cmd](cheat-sheets/linux-cmd/ "mention")
* [file permissions & attributes](betriebssysteme/linux/dateiberechtigungen-und-attribute.md "mention")
* [directory structure](betriebssysteme/linux/ordnerstruktur/ "mention")
* [distributions](spezialisierungen-and-vertiefungen/betriebssysteme/linux/distributionen/ "mention")

[windows](betriebssysteme/windows/ "mention")

* [active directory](spezialisierungen-and-vertiefungen/betriebssysteme/windows/active-directory/ "mention")
* [commands](spezialisierungen-and-vertiefungen/betriebssysteme/windows/befehle/ "mention")
* [internals](spezialisierungen-and-vertiefungen/betriebssysteme/windows/internal/ "mention")
* [local persistence](spezialisierungen-and-vertiefungen/betriebssysteme/windows/lokale-persistenz/ "mention")
* [password storage & registry hives](spezialisierungen-and-vertiefungen/betriebssysteme/windows/passwortspeicher-and-registry-hives.md "mention")
* [sysinternals suite](spezialisierungen-and-vertiefungen/betriebssysteme/windows/sysinternals-suite/ "mention")
* [windows nt](betriebssysteme/windows/windows-nt.md "mention")

</details>

<details>

<summary>cheat sheets & commands</summary>

# compact references for tools, languages & shells

SHELLS

* [bash](cheat-sheets/bash/ "mention")
* [powershell](cheat-sheets/powershell.md "mention")
* [linux cmd](cheat-sheets/linux-cmd/ "mention") | [windows cmd](cheat-sheets/windows-cmd.md "mention")

TOOLS & UTILITIES

* [curl](cheat-sheets/curl.md "mention")
* [git](cheat-sheets/git.md "mention")
* [telnet](cheat-sheets/telnet.md "mention")

[reverse engineering](cheat-sheets/reverseengineering/ "mention")

* [malware analysis](killchain/exploitation-persistence-and-privilege-escalation/malware/ "mention")
* [malicious pdf analysis - js & exe extraction](cheat-sheets/reverseengineering/analyse-von-bosartigen-pdf-dateien-javascript-and-exe-extraktion.md "mention")
* [suspicious office docs with vmonkey](cheat-sheets/reverseengineering/analyse-von-verdachtigen-microsoft-office-dokumenten-mit-vmonkey.md "mention")

</details>

<details>

<summary>red teaming</summary>

# offensive security & attack vectors

[privilege escalation](cheat-sheets/privilege-escalation.md "mention")

* [linux privesc](killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/linux-privesc.md "mention")
* [windows privesc](killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/windows-privesc.md "mention")
* [gtfobins](killchain/exploitation-persistence-and-privilege-escalation/privilege-escalation/gtfobin.md "mention")

COMMAND & CONTROL

* [c2 frameworks](red-team/c2-command-and-control.md "mention")
* [tools](soc/level-1/digitale-forensik-and-incident-response/tools/ "mention") | [cti tools](soc/level-1/cyber-threat-intelligence/tools.md "mention")

EXPLOITATION & INITIAL ACCESS

* [recon & initial access](recon-and-initial-access/ "mention")
* [upload vulnerabilities](angriffsszenarien/upload-vulnerability/ "mention")

</details>

<details>

<summary>blue teaming / defense</summary>

# defense strategies & monitoring

[soc](soc/ "mention") LEVEL 1 & 2

* [tools](soc/level-1/digitale-forensik-and-incident-response/tools/ "mention")
* [network security & traffic analysis](soc/level-1/netzwerk-security-and-traffic-analyse/ "mention")
* [endpoint security monitoring](soc/level-1/endpoint-security-monitoring/ "mention")
* [siem](soc/level-1/siem/ "mention")

CYBER THREAT INTELLIGENCE

* [misp](programme-skripte/misp.md "mention") | [misp deep dive](soc/level-1/cyber-threat-intelligence/misp.md "mention")
* [yara rules](soc/level-1/cyber-threat-intelligence/yara.md "mention")
* [opencti](soc/level-1/cyber-threat-intelligence/opencti.md "mention")

</details>

<details>

<summary>general knowledge & references</summary>

# theory, protocols & concepts

BEST PRACTICES & FUNDAMENTALS

* [threat modeling & incident response](security-allgemein/threat-modelling-and-incident-response.md "mention")
* [principles of privileges](security-allgemein/principles-of-privileges.md "mention")
* [frameworks](security-allgemein/frameworks.md "mention")
* [governance & regulation](operational-security-and-teams/governance-and-regulation/ "mention")

PROTOCOLS & NETWORKING

* [tcp](informationen/protokolle-und-server/tcp.md "mention") | [udp](informationen/protokolle-und-server/udp.md "mention")
* [quic (quick udp internet connections)](informationen/protokolle-und-server/quic-quick-udp-internet-connections.md "mention")
* [ports](informationen/netzwerken/ports.md "mention") | [protocols & servers](informationen/protokolle-und-server/ "mention")

FILE FORMATS

* [xml](informationen/xml.md "mention") | [pdf](informationen/pdf.md "mention") | [certificates](informationen/zertifikate.md "mention")

</details>

<details>

<summary>exercises & writeups</summary>

# ctfs, tryhackme, practical challenges

BOOT2ROOT

* [boot2root](ctf-kingofthehill/tryhackme/boot2root/ "mention")

KOTH (KING OF THE HILL)

* [koth](ctf-kingofthehill/tryhackme/koth/ "mention")

WRITEUPS

* [writeups](ctf-kingofthehill/tryhackme/writeups/ "mention")

</details>

<details>

<summary>tools & programs</summary>

# quick reference for offensive/defensive tools

* [burp suite](programme-skripte/burp-suite.md "mention")
* [metasploit](programme-skripte/metasploit.md "mention")
* [netcat](programme-skripte/netcat.md "mention") | [nmap](programme-skripte/nmap.md "mention")
* [enum4linux](programme-skripte/enum4linux.md "mention")
* [sqlmap](programme-skripte/sqlmap.md "mention")
* [elastic stack (elk)](programme-skripte/elastic-stack-elk/ "mention")
* [wireshark](programme-skripte/wireshark.md "mention")
* [vim](programme-skripte/vim.md "mention") | [dpkg](programme-skripte/dpkg.md "mention") | [openssl](programme-skripte/openssl.md "mention")

</details>
