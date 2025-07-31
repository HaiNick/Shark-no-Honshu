# command and control — manage your botnet like a sysadmin

frameworks for running multiple reverse shells without losing your sanity. c2 manages agents, deploys payloads, keeps callbacks organized.

## c2 framework components

**c2 server**
central brain that talks to all agents. operators send commands through this.

**agents / payloads**  
reverse shells with extra features — file transfer, process injection, lateral movement. configurable callback intervals to avoid detection patterns.

**listeners**
services waiting for agent connections on specific ports/protocols (DNS, HTTP, HTTPS, custom).

**beaconing**
periodic check-ins from agents to grab new commands and report status.

**beaconing**

beaconing is the periodic callback from an agent to the c2 server. this process transmits status and receives new instructions.

example: havoc c2 beacon: [https://www.virustotal.com/gui/file/99784f28e4e95f044d97e402bbf58f369c7c37f49dc5bf48e6b2e706181db3b7/summary](https://www.virustotal.com/gui/file/99784f28e4e95f044d97e402bbf58f369c7c37f49dc5bf48e6b2e706181db3b7/summary)

decoding among others: [https://www.immersivelabs.com/resources/blog/havoc-c2-framework-a-defensive-operators-guide](https://www.immersivelabs.com/resources/blog/havoc-c2-framework-a-defensive-operators-guide)

***

### hiding callback connections

**sleep timer**

a key feature for concealing c2 communication is temporal control of beaconing. security solutions like antiviruses or firewalls often recognize recurring connections as suspicious. a defined sleep timer controls intervals between callbacks.

**jitter**

jitter adds a random component to the sleep timer, so time intervals between beaconing vary. this irregular pattern better simulates real user behavior and makes detection by security mechanisms harder.

***

### payload types

**stageless payloads**

stageless payloads contain the complete c2 agent in the initial code and start beaconing immediately after execution. typical usage steps:

1. dropper is transferred to target system and executed
2. beaconing to c2 server begins immediately

**staged payloads**

staged payloads consist of multiple components. the initial code - usually called a dropper - establishes a connection to the c2 server to download additional parts of the agent. this method offers advantages in code size and concealment:

1. dropper is executed on target system
2. it contacts the c2 server to retrieve the next stage (stage 2)
3. second stage is transferred to target system and executed in memory
4. beaconing begins

this technique reduces initial code base and improves evasion of security solutions through obfuscation.

**additional payload formats**

c2 frameworks typically support various payload formats including:

* powershell scripts with embedded c# code
* hta files (html applications)
* jscript files
* visual basic scripts or applications
* microsoft office documents with embedded macros

***

### modules

modules significantly extend c2 framework functionality. they're implemented in different programming languages depending on the framework:

* **cobalt strike:** aggressor scripts (aggressor scripting language)
* **powershell empire:** supports multiple scripting languages
* **metasploit:** ruby-based modules

**post-exploitation modules**

these modules are used after successful system compromise. examples range from running reconnaissance tools like _sharphound.ps1_ to extracting and analyzing sensitive data (e.g., from _lsass.exe_ process).

**pivoting modules**

pivoting modules enable extending access reach within a network. for example, with administrative rights, an smb beacon can be set up using the compromised system as a proxy for other hosts in the internal network.

***

### c2 frameworks on the internet

**domain fronting**

domain fronting is a technique where traffic appears to route through a trusted provider (e.g., cloudflare). actual c2 communication is concealed as it occurs via known ip addresses and domains. from a geolocation perspective, traffic appears unremarkable.

**c2 profiles**

c2 profiles define how a c2 server processes http requests. this technique may appear under various terms:

* nginx reverse proxy
* apache mod_proxy / mod_rewrite
* malleable http c2 profiles

through targeted manipulation of http headers like `x-c2-server`, the server can deliver differentiated responses. thus a harmless web server can display a generic webpage to outsiders while simultaneously responding to specific agent requests.

***

## known c2 frameworks

### free

* metasploit [[https://www.metasploit.com/](https://www.metasploit.com/)]
* armitage [[https://web.archive.org/web/20211006153158/http://www.fastandeasyhacking.com/](https://web.archive.org/web/20211006153158/http://www.fastandeasyhacking.com/)]
* powershell empire/ starkiller [[https://bc-security.gitbook.io/empire-wiki/](https://bc-security.gitbook.io/empire-wiki/)], [[https://github.com/BC-SECURITY/Starkiller](https://github.com/BC-SECURITY/Starkiller)]
* covenant [[https://github.com/cobbr/Covenant](https://github.com/cobbr/Covenant)]
* sliver [[https://github.com/BishopFox/sliver](https://github.com/BishopFox/sliver)]
* havoc [[https://github.com/HavocFramework/Havoc](https://github.com/HavocFramework/Havoc?tab=readme-ov-file)]

### commercial

* cobalt strike [[https://www.cobaltstrike.com/](https://www.cobaltstrike.com/)]
* brute ratel [[https://bruteratel.com/](https://bruteratel.com/)]

### more info

{% embed url="https://howto.thec2matrix.com/" %}

## methods

### identifying requesting device (agent or standard client)

<div data-full-width="true"><figure><img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5d5a2b006986bf3508047664/room-content/22eac0e3ab2de3f61d57e858cee3e33e.png" alt=""><figcaption></figcaption></figure></div>

references:

[https://github.com/HavocFramework/Havoc?tab=readme-ov-file](https://github.com/HavocFramework/Havoc?tab=readme-ov-file)
