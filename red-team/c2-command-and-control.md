# C2 - Command & Control

Ein Command-and-Control-Framework (C2-Framework) ist eine Softwarelösung, die es ermöglicht, mehrere Reverse Shells – in diesem Kontext als C2-Agents bezeichnet – zentral zu verwalten und zu steuern. Häufig verfügen diese Frameworks über integrierte Payload-Generatoren, wie etwa MSFVenom im Metasploit Framework.

***

## Struktur eines C2-Frameworks

**C2-Server**

Der C2-Server bildet die zentrale Komponente eines C2-Frameworks. Er fungiert als Kommunikationsschnittstelle zwischen dem Operator und den Agents. Letztere nehmen regelmäßig Kontakt zum Server auf, um Befehle entgegenzunehmen.

**Agents / Payloads**

Ein Agent ist ein vom Framework generiertes Programm, das eine Verbindung zu einem Listener auf dem C2-Server aufbaut. Im Vergleich zu einer einfachen Reverse Shell bietet ein Agent erweiterte Funktionalitäten, etwa den Datei-Upload/-Download oder die Ausführung interner Kommandos. Agents sind in der Regel hochgradig konfigurierbar – insbesondere hinsichtlich der Frequenz und Methodik, mit der sie den Server kontaktieren.

**Listener**

Ein Listener ist ein Dienst auf dem C2-Server, der eingehende Verbindungen über spezifische Ports oder Protokolle (z. B. DNS, HTTP oder HTTPS) entgegennimmt. Er bildet das Gegenstück zum initialen Verbindungsaufbau eines Agents.

**Beaconing**

Als Beaconing bezeichnet man den periodischen Rückruf eines Agents zum C2-Server. Dieser Vorgang dient der Statusübermittlung und dem Empfang neuer Anweisungen.

Bsp: Havoc C2-Beacon: [https://www.virustotal.com/gui/file/99784f28e4e95f044d97e402bbf58f369c7c37f49dc5bf48e6b2e706181db3b7/summary](https://www.virustotal.com/gui/file/99784f28e4e95f044d97e402bbf58f369c7c37f49dc5bf48e6b2e706181db3b7/summary)

Decoding u.a.: [https://www.immersivelabs.com/resources/blog/havoc-c2-framework-a-defensive-operators-guide](https://www.immersivelabs.com/resources/blog/havoc-c2-framework-a-defensive-operators-guide)

***

### Tarnung von Callback-Verbindungen

**Sleep-Timer**

Ein zentrales Merkmal zur Verschleierung von C2-Kommunikation ist die zeitliche Steuerung des Beaconings. Sicherheitslösungen wie Antivirenprogramme oder Firewalls erkennen oft wiederkehrende Verbindungen als verdächtig. Ein definierter Sleep-Timer steuert daher die Intervalle zwischen den Rückrufen.

**Jitter**

Jitter ergänzt den Sleep-Timer um eine Zufallskomponente, sodass das Zeitintervall zwischen den Beaconing-Vorgängen variiert. Dieses unregelmäßige Muster simuliert besser das Verhalten realer Benutzer und erschwert die Erkennung durch Sicherheitsmechanismen.

***

### Arten von Payloads

**Stageless Payloads**

Stageless Payloads enthalten den vollständigen C2-Agent bereits im initialen Code und starten sofort nach Ausführung das Beaconing. Die typischen Schritte bei der Verwendung sind:

1. Der Dropper wird auf das Zielsystem übertragen und ausgeführt.
2. Das Beaconing zum C2-Server beginnt unmittelbar.

**Staged Payloads**

Staged Payloads bestehen aus mehreren Komponenten. Der initiale Code – meist als Dropper bezeichnet – stellt eine Verbindung zum C2-Server her, um weitere Teile des Agents nachzuladen. Diese Methode bietet Vorteile hinsichtlich Codegröße und Tarnung:

1. Der Dropper wird auf dem Zielsystem ausgeführt.
2. Er kontaktiert den C2-Server, um die nächste Stufe (Stage 2) abzurufen.
3. Die zweite Stufe wird auf das Zielsystem übertragen und im Speicher ausgeführt.
4. Das Beaconing beginnt.

Diese Technik reduziert die initiale Codebasis und verbessert die Umgehung von Sicherheitslösungen durch Obfuskation.

**Weitere Payload-Formate**

C2-Frameworks unterstützen in der Regel eine Vielzahl von Payload-Formaten, unter anderem:

* PowerShell-Skripte mit eingebettetem C#-Code
* HTA-Dateien (HTML Applications)
* JScript-Dateien
* Visual Basic-Skripte oder -Anwendungen
* Microsoft Office-Dokumente mit eingebetteten Makros

***

### Module

Module erweitern die Funktionalität eines C2-Frameworks erheblich. Sie werden in verschiedenen Programmiersprachen implementiert, abhängig vom jeweiligen Framework:

* **Cobalt Strike:** Aggressor Scripts (Aggressor Scripting Language)
* **PowerShell Empire:** unterstützt mehrere Skriptsprachen
* **Metasploit:** Ruby-basierte Module

**Post-Exploitation-Module**

Diese Module kommen nach erfolgreicher Kompromittierung eines Zielsystems zum Einsatz. Beispiele reichen vom Ausführen von Reconnaissance-Tools wie _SharpHound.ps1_ bis zur Extraktion und Analyse sensibler Daten (z. B. aus dem Prozess _lsass.exe_).

**Pivoting-Module**

Pivoting-Module ermöglichen die Erweiterung der Zugriffsreichweite innerhalb eines Netzwerks. So kann etwa mit administrativen Rechten ein sogenannter SMB-Beacon eingerichtet werden, der das kompromittierte System als Proxy für andere Hosts im internen Netzwerk nutzt.

***

### Einsatz von C2-Frameworks im Internet

**Domain Fronting**

Domain Fronting ist eine Technik, bei der der Datenverkehr scheinbar über einen vertrauenswürdigen Anbieter (z. B. Cloudflare) geleitet wird. Die eigentliche C2-Kommunikation wird dabei verschleiert, da sie über bekannte IP-Adressen und Domains erfolgt. Aus Sicht der Geolokalisierung erscheint der Datenverkehr unauffällig.

**C2 Profiles**

C2-Profile definieren, wie ein C2-Server HTTP-Anfragen verarbeitet. Diese Technik kann unter verschiedenen Begriffen auftreten, etwa:

* NGINX Reverse Proxy
* Apache Mod\_Proxy / Mod\_Rewrite
* Malleable HTTP C2 Profiles

Durch gezielte Manipulation von HTTP-Headern, z. B. `X-C2-Server`, kann der Server differenzierte Antworten liefern. So kann ein harmloser Webserver für Außenstehende eine generische Webseite darstellen, während er gleichzeitig auf spezifische Anfragen eines Agents reagiert.

***

## Bekannte C2-Frameworks

### Kostenlos

* Metasploit \[[https://www.metasploit.com/](https://www.metasploit.com/)]
* Armitage \[[https://web.archive.org/web/20211006153158/http://www.fastandeasyhacking.com/](https://web.archive.org/web/20211006153158/http://www.fastandeasyhacking.com/)]
* Powershell Empire/ Starkiller \[[https://bc-security.gitbook.io/empire-wiki/](https://bc-security.gitbook.io/empire-wiki/)], \[[https://github.com/BC-SECURITY/Starkiller](https://github.com/BC-SECURITY/Starkiller)]
* Covenant \[[https://github.com/cobbr/Covenant](https://github.com/cobbr/Covenant)]
* Sliver \[[https://github.com/BishopFox/sliver](https://github.com/BishopFox/sliver)]
* Havoc \[[https://github.com/HavocFramework/Havoc](https://github.com/HavocFramework/Havoc?tab=readme-ov-file)]



### Kommerziell

* Cobalt strike \[[https://www.cobaltstrike.com/](https://www.cobaltstrike.com/)]
* Brute Ratel \[[https://bruteratel.com/](https://bruteratel.com/)]



### Weitere Infos

{% embed url="https://howto.thec2matrix.com/" %}

## Methoden





### Identifikation des anfragenden Gerätes ( Agent oder Standard Client )

<div data-full-width="true"><figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5d5a2b006986bf3508047664/room-content/22eac0e3ab2de3f61d57e858cee3e33e.png" alt=""><figcaption></figcaption></figure></div>

Referenzen:

[https://github.com/HavocFramework/Havoc?tab=readme-ov-file](https://github.com/HavocFramework/Havoc?tab=readme-ov-file)







