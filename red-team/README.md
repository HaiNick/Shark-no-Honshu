# Red Team

* [ ] **Man-in-the-Middle (MITM)**\
  – ARP-Spoofing, DNS-Spoofing, SSL Stripping
* [ ] **Port Scanning & Service Fingerprinting**\
  – SYN Scan, ACK Scan, Banner Grabbing
* [ ] **Packet Sniffing & Injection**\
  – z. B. mit Wireshark, Scapy, Bettercap
* [ ] **Rogue Services**\
  – Rogue DHCP, DNS Poisoning, Fake SMB
* [ ] **Network Segmentation Bypassing**\
  – VLAN Hopping, L2 Attacks
* [ ] **Lateral Movement via Network Services**\
  – SMB, RDP, WinRM, WMI

<details>

<summary>Links:</summary>

[https://redteam.guide/docs/definitions/](https://redteam.guide/docs/definitions/)

[https://redteam.guide/docs/templates/roe\_template/](https://redteam.guide/docs/templates/roe_template/)

[https://vectr.io/](https://vectr.io/)

</details>

## Vorgehen

**\[ Kritische Informationen ]** Wir sprechen von den Programmen, dem Betriebssystem (OS) und der virtuellen Maschine (VM) zusammen.&#x20;

**\[ Bedrohungsanalyse ]** Das blaue Team sucht nach bösartigen oder anormalen Aktivitäten im Netzwerk. Je nach dem Dienst, zu dem wir eine Verbindung herstellen, ist es möglich, dass der Name und die Version des Programms, das wir verwenden, sowie die Betriebssystemversion und der Hostname der VM protokolliert werden.

**\[ Schwachstellenanalyse ]** Wenn das für eine bestimmte Aktivität gewählte Betriebssystem zu eindeutig ist, könnte es leichter sein, Aktivitäten mit Ihrem Betrieb in Verbindung zu bringen. Das Gleiche gilt für VMs mit auffälligen Hostnamen. Wenn beispielsweise in einem Netzwerk aus physischen Laptops und Desktops ein neuer Host mit dem Hostnamen kali2021vm hinzukommt, sollte er für das Blue Team leicht zu erkennen sein. Das Gleiche gilt, wenn Sie verschiedene Sicherheitsscanner einsetzen oder beispielsweise keinen gemeinsamen Benutzeragenten für webbasierte Aktivitäten verwenden.&#x20;

**\[ Risikobewertung ]** Das Risiko hängt hauptsächlich davon ab, mit welchen Diensten wir uns verbinden. Wenn wir zum Beispiel eine VPN-Verbindung aufbauen, wird der VPN-Server eine Menge Informationen über uns protokollieren. Das Gleiche gilt für andere Dienste, mit denen wir eine Verbindung herstellen können.

**\[ Gegenmaßnahmen ]** Wenn das von uns verwendete Betriebssystem ungewöhnlich ist, lohnt es sich, die notwendigen Änderungen vorzunehmen, um unser Betriebssystem als ein anderes zu tarnen. Bei VMs und physischen Hosts lohnt es sich, die Hostnamen in etwas Unauffälliges oder mit den Namenskonventionen des Clients übereinstimmendes zu ändern, da man nicht möchte, dass ein Hostname wie AttackBox in den DHCP-Serverprotokollen erscheint. Was Programme und Tools betrifft, so lohnt es sich, die Signaturen zu kennen, die jedes Tool in den Serverprotokollen hinterlässt.









