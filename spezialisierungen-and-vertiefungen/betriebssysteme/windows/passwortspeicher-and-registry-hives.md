# Passwortspeicher & Registry Hives

<details>

<summary>Links:</summary>

[https://github.com/roo7break/impacket/blob/master/examples/secretsdump.py](https://github.com/roo7break/impacket/blob/master/examples/secretsdump.py) (original)

[https://github.com/fortra/impacket/blob/master/examples/secretsdump.py](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py)

[https://github.com/fin3ss3g0d/secretsdump.py](https://github.com/fin3ss3g0d/secretsdump.py) (modifiziert -> Extraktion von Secrets über mehrere Windows-Systeme)

</details>

### N**TDS.dit**

* **Ort:** `C:\Windows\NTDS\NTDS.dit` (nur auf Domain Controllern)
* **Funktion:** Enthält die **Active Directory-Datenbank**, inkl. aller Benutzerkonten, Passwort-Hashes, Gruppen, Richtlinien etc.
* **Passwortspeicher:** Enthält **NTLM-Hashes**, manchmal auch **Kerberos-Keys**.
* **Extraktionstools:** `ntdsutil`, `secretsdump.py`, `NTDSXtract`, `Impacket`

### **SAM (Security Account Manager)**

* **Ort:** `C:\Windows\System32\Config\SAM`
* **Funktion:** Speichert lokale Benutzerkonten und deren **NTLM-Hashes** auf Nicht-Domain-Systemen.
* **Nur lesbar mit SYSTEM-Hive**, da der **SysKey (BootKey)** im SYSTEM-Hive steckt.
* **Extraktionstools:** `mimikatz`, `secretsdump.py`, `pwdump`

### **SYSTEM Hive**

* **Ort:** `C:\Windows\System32\Config\SYSTEM`
* **Funktion:** Enthält Systemkonfiguration inkl. **BootKey (SysKey)**, der zur Entschlüsselung von SAM und Teilen von NTDS.dit nötig ist.
* **Wichtig für:** Kombinierte Extraktion mit SAM oder NTDS.dit.



**Typische Kombination für Extraktion:**

```bash
secretsdump.py -system SYSTEM -sam SAM LOCAL
```

Oder für AD:

```bash
secretsdump.py -system SYSTEM -ntds NTDS.dit LOCAL
```



### **Dump mit secretsdump.py & Impacket – Analyse & Struktur**

**Beispielausgabe (`secretsdump.py`):**

```
[*] Target system bootKey: 0x36c8d26ec0df8b23ce63bcefa6e2d821
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:f3118544a831e728781d780cfdb9c1fa:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
...
[*] Cleaning up...
```

#### **Bestandteile**

* **bootKey:** Entschlüsselungsschlüssel aus der SYSTEM-Registry-Hive, notwendig zur Freigabe der SAM-Daten.
* **Format:** `Benutzername:RID:LM-Hash:NT-Hash:::`

**Felder im Detail:**

| Feld     | Bedeutung                                            |
| -------- | ---------------------------------------------------- |
| Benutzer | Name des lokalen Benutzerkontos                      |
| RID      | Relative Identifier (500 = Admin etc.)               |
| LM-Hash  | Veraltetes Hashformat, meist deaktiviert (`aad3...`) |
| NT-Hash  | Aktueller Passworthash (NTLM)                        |
| `:::`    | Platzhalter für optionale Felder                     |

***

#### **Typische Einträge**

| Benutzer             | RID   | Bedeutung                                |
| -------------------- | ----- | ---------------------------------------- |
| `Administrator`      | 500   | Lokaler Admin                            |
| `Guest`              | 501   | Gastkonto (oft deaktiviert)              |
| `DefaultAccount`     | 503   | Standardkonto für interne Systemvorgänge |
| `WDAGUtilityAccount` | 504   | Konto für Windows Defender Sandbox       |
| `thmuser*`           | 1008+ | Benutzerdefinierte lokale Konten         |

***

#### **Verwendung**

* **Offline-Cracking:** NTLM-Hashes können mit Hashcat oder John the Ripper gecrackt werden.
* **Pass-the-Hash:** Direkte Authentifizierung ohne Passwort, z. B. mit `evil-winrm`, `wmiexec.py`, `pth-winexe`.

**Tools:**

```bash
hashcat -m 1000 -a 0 hashes.txt wordlist.txt
john --format=NT hashes.txt --wordlist=rockyou.txt
```
