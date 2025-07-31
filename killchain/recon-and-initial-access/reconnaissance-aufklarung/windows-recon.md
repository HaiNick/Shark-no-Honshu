# Windows-Recon

## **Portscan – Analyse typischer Dienste in Active Directory-Umgebungen**

Beim Scannen von Windows-basierten Netzwerken ist die Identifikation spezifischer Dienste über TCP-Ports essenziell. Diese Dienste bilden häufig die Angriffsfläche für Authentifizierungsangriffe, Lateral Movement oder Privilegieneskalation innerhalb von Active Directory-Umgebungen.

### **Typische Ziel-Ports und ihre sicherheitsrelevante Bedeutung:**

* **TCP 88 – Kerberos (Authentifizierung)**\
  Wird für die Authentifizierung über Kerberos verwendet.\
  → Relevant für Angriffe wie _Kerberoasting_, _Pass-the-Ticket_, _Overpass-the-Hash_.\
  → Tools: `impacket`, `Rubeus`, `hashcat`
* **TCP 135 – RPC Endpoint Mapper**\
  Zuständig für Remote Procedure Calls und DCOM-Kommunikation.\
  → Ermöglicht Enumeration von Diensten und potentiellen _RCE_-Angriffen.\
  → Tools: `rpcdump.py`, `rpcclient`, `CrackMapExec`
* **TCP 139 – NetBIOS Session Service**\
  Wird in älteren Windows-Systemen für Datei- und Druckfreigaben genutzt.\
  → Angreifbar über _Null Sessions_, Informationsgewinnung und Authentifizierungsangriffe.\
  → Tools: `enum4linux`, `smbclient`, `nbtscan`
* **TCP 389 – LDAP (unverschlüsselt)**\
  Dient der Abfrage des Active Directory per LDAP-Protokoll.\
  → Ideal zur Enumeration von Benutzern, Gruppen und Gruppenrichtlinien.\
  → Tools: `ldapsearch`, `ldapdomaindump`, `bloodhound`
* **TCP 445 – SMB (Server Message Block)**\
  Dient der Dateiübertragung und Fernadministration.\
  → Ziel für Angriffe wie _SMB-Relay_, _EternalBlue_, _NTLM-Relay_, _Pass-the-Hash_.\
  → Tools: `smbclient`, `crackmapexec`, `nmap`, `Metasploit`
* **TCP 636 – LDAPS (LDAP über SSL/TLS)**\
  Verschlüsselte Variante des LDAP-Protokolls.\
  → Bei Fehlkonfiguration angreifbar, z. B. durch _AD CS_-basierte Zertifikatsangriffe.\
  → Tools: `certipy`, `ldapsearch`, `pkinittools`

***

**LDAP- und RPC-Enumeration – Benutzer- und Dienstidentifikation in Active Directory**

Im Rahmen eines Active Directory-Assessments ist die Enumeration von Benutzern, Gruppen und Diensten über Protokolle wie LDAP, SMB und RPC ein essenzieller erster Schritt. Diese Techniken ermöglichen es, ohne Authentifizierung wertvolle Informationen zu extrahieren – insbesondere bei fehlerhafter oder zu großzügiger Serverkonfiguration.

***

### LDAP Enumeration (Anonymous Bind)

**Lightweight Directory Access Protocol (LDAP)** ermöglicht den Zugriff auf zentrale Verzeichnisdienste wie Microsoft Active Directory. Es dient zur Abfrage und Organisation von Informationen zu Benutzern, Gruppen, Geräten und Organisationseinheiten.

Viele LDAP-Server sind so konfiguriert, dass sie **anonyme, lesende Bindings** erlauben. Dies kann ohne Authentifizierung Zugriff auf sensible Objekte ermöglichen.

**Test auf anonymen LDAP-Zugriff:**

```bash
ldapsearch -x -H ldap://10.211.11.10 -s base
```

**Parameter:**

* `-x` – Anonyme (simple) Authentifizierung
* `-H` – LDAP-Serveradresse
* `-s base` – Begrenzung der Abfrage auf das Basisobjekt

```
ldapsearch -x -H ldap://10.211.11.10 -b "dc=company,dc=loc" "(objectClass=person)"
```

***

### Erweiterte Enumeration mit `enum4linux-ng`

`enum4linux-ng` automatisiert Enumerationstechniken gegen Windows-Systeme unter Verwendung von SMB und RPC. Es kann Informationen zu Benutzern, Gruppen, Freigaben, Betriebssystemdetails und Passwort-Richtlinien extrahieren.

**Kommando für vollständige Enumeration:**

```bash
enum4linux-ng -A 10.211.11.10 -oA results.txt
```

**Parameter:**

* `-A` – Führt alle verfügbaren Enumerationsmethoden aus
* `-oA` – Speichert Ausgabe als YAML und JSON-Dateien

***

### RPC Enumeration (Null Sessions)

**Microsoft RPC** erlaubt Programmen, über das Netzwerk Dienste anderer Programme zu nutzen. Wenn SMB für **Null Sessions** (anonyme Verbindungen) offen ist, kann man über `IPC$`-Freigaben auf Informationen zugreifen – ohne Authentifizierung.

**Verbindungstest über Null Session:**

```bash
rpcclient -U "" 10.211.11.10 -N
```

**Parameter:**

* `-U ""` – Leerer Benutzername für anonymen Login
* `-N` – Keine Passwortabfrage

Nach erfolgreichem Zugriff können über Befehle wie `enumdomusers` oder `queryuser` Benutzer- und Gruppeninformationen extrahiert werden.

***

### Username Enumeration mit Kerbrute

**Kerberos** ist das Standard-Authentifizierungsprotokoll in Windows-Domänen. Tools wie **Kerbrute** nutzen das Verhalten des Kerberos Pre-Authentication-Mechanismus zur Validierung von Benutzernamen – ohne Passwort.

**Anwendungsfall:** Validierung von Benutzern, die durch `enum4linux-ng` oder `rpcclient` identifiziert wurden (z. B. Ausschluss von Fake-Accounts).

***

### RID Cycling

Active Directory weist Benutzern und Gruppen sogenannte **Relative Identifiers (RIDs)** zu. Diese RIDs sind Bestandteil des **Security Identifiers (SID)** und folgen einem bekannten Schema:

* `500` – Administrator
* `501` – Gast
* `512–514` – Domain Admins, Users, Guests
* `1000+` – Benutzerkonten

Durch sogenanntes **RID Cycling** kann man gültige Benutzerkonten aufdecken, indem man systematisch durch mögliche RIDs iteriert und auf gültige Antworten prüft. Dies ist besonders nützlich bei anonymem Zugriff, etwa zur Sicherheitsprüfung (z. B. Penetration Testing).

#### Tools zur RID Enumeration

* `enum4linux-ng` (automatisiert)
* `rpcclient` (manuell, wie unten gezeigt)

***

#### Beispiel: Manuelle RID Enumeration mit `rpcclient`

```bash
for i in $(seq 500 2000); do
  echo "queryuser $i" | rpcclient -U "" -N 10.211.11.10 2>/dev/null | grep -i "User Name"
done
```

**Was passiert hier:**

* `for i in $(seq 500 2000)`: Durchläuft mögliche RID-Werte von `500` bis `2000`
* `echo "queryuser $i"`: Baut den RPC-Befehl zur Benutzerabfrage
* `rpcclient -U "" -N 10.211.11.10`: Stellt eine Verbindung zum Zielsystem her – anonym (`-U "" -N`)
* `2>/dev/null`: Unterdrückt Fehlermeldungen (z. B. "Access denied" bei ungültigen RIDs)
* `grep -i "User Name"`: Gibt nur Zeilen mit gefundenen Benutzern aus

**Beispielausgabe:**

```
User Name   :	gerald.burgess
User Name   :	nigel.parsons
User Name   :	barbara.jones
```

***

#### Optional: Nur die Benutzernamen extrahieren

```bash
for i in $(seq 500 2000); do
  echo "queryuser $i" | rpcclient -U "" -N 10.211.11.10 2>/dev/null | grep -i "User Name" | cut -d':' -f2 | xargs
done
```

Dies filtert die Ausgabe weiter und zeigt nur den reinen Benutzernamen ohne Zusatzinformationen an.
