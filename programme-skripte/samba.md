---
description: Interaktion mit Windows-Netzen (SMB/CIFS, Active Directory, Freigaben etc.)
---

# samba

## ne&#x74;**-Befehl Cheatsheet**

{% hint style="info" %}
Der Linux-`net`-Befehl hat **nichts mit dem Windows-`net` zu tun**, auch wenn der Name gleich ist. Er ist ein Werkzeug zur Verwaltung von **Samba/SMB/AD-Ressourcen**.
{% endhint %}

### **Domänen & Active Directory**

| Befehl                   | Beschreibung                                |
| ------------------------ | ------------------------------------------- |
| `net ads join -U admin`  | Linux-Host einer AD-Domäne beitreten        |
| `net ads leave -U admin` | AD-Domäne verlassen                         |
| `net ads status`         | AD-Status anzeigen                          |
| `net ads info`           | Infos zur aktuellen AD-Mitgliedschaft       |
| `net ads user`           | Benutzerliste aus Active Directory anzeigen |
| `net ads group`          | Gruppenliste anzeigen                       |
| `net ads dns register`   | DNS-Registrierung für den Host              |
| `net ads testjoin`       | Testet Domänenmitgliedschaft                |
| `net ads changetrustpw`  | Vertrauenskennwort neu setzen               |

### **Authentifizierung / Anmeldung**

| Befehl                  | Beschreibung                                            |
| ----------------------- | ------------------------------------------------------- |
| `net login`             | Login mit gespeicherten Zugangsdaten (selten verwendet) |
| `net logout`            | Abmeldung                                               |
| `net changetrustpw`     | Vertrauenskennwort zur Domäne ändern                    |
| `net rpc trustdom list` | Vertrauensstellungen anzeigen (RPC)                     |

### **Freigaben anzeigen und verwalten**

| Befehl                             | Beschreibung                        |
| ---------------------------------- | ----------------------------------- |
| `net view \\Server`                | Freigaben auf einem Server anzeigen |
| `net rpc share list -I IP -U user` | Freigaben über RPC anzeigen         |
| `net usershare list`               | Liste lokaler Benutzerfreigaben     |
| `net usershare add`                | Neue Benutzerfreigabe hinzufügen    |
| `net usershare delete`             | Freigabe löschen                    |
| `net usershare info`               | Infos zu einer Freigabe anzeigen    |

### **Benutzer- und Gruppenverwaltung (meist mit RPC)**

| Befehl                               | Beschreibung                   |
| ------------------------------------ | ------------------------------ |
| `net rpc user`                       | Benutzerliste anzeigen         |
| `net rpc group`                      | Gruppen anzeigen               |
| `net rpc user add username -U admin` | Benutzer anlegen               |
| `net rpc user delete username`       | Benutzer löschen               |
| `net rpc group add members`          | Benutzer zur Gruppe hinzufügen |

> ⚠️ RPC-Funktionen setzen häufig `smb.conf`-Konfiguration und Domänenmitgliedschaft voraus.

***

### **RPC-Dienstaktionen**

| Befehl                               | Beschreibung                                 |
| ------------------------------------ | -------------------------------------------- |
| `net rpc service list -I IP -U user` | Dienste auf Remote-Windows-Rechner auflisten |
| `net rpc service start Dienstname`   | Dienst starten                               |
| `net rpc service stop Dienstname`    | Dienst stoppen                               |

### &#x20;**Zeit & Statistik**

| Befehl                 | Beschreibung                              |
| ---------------------- | ----------------------------------------- |
| `net time \\Server`    | Aktuelle Zeit eines SMB-Servers anzeigen  |
| `net time \\Server -S` | Systemzeit mit Server synchronisieren     |
| `net status`           | Zeigt Samba-Server-Status                 |
| `net rpc stats`        | RPC-Statistiken anzeigen (wenn verfügbar) |

### **Druckerverwaltung**

| Befehl                   | Beschreibung                  |
| ------------------------ | ----------------------------- |
| `net rpc printer`        | Drucker anzeigen              |
| `net rpc printer status` | Status eines Druckers abrufen |

### **Hilfs- & Diagnosebefehle**

| Befehl                   | Beschreibung                 |
| ------------------------ | ---------------------------- |
| `net help`               | Allgemeine Hilfe anzeigen    |
| `net help [Unterbefehl]` | Hilfe zu spezifischem Befehl |
| `net lookup`             | DNS-/NetBIOS-Namensauflösung |
| `net getlocalsid`        | Lokale SID anzeigen          |
| `net getdomainsid`       | Domänen-SID anzeigen         |
| `net lookup host`        | Namensauflösung prüfen       |

### Voraussetzungen

* Paket: `samba` oder `samba-client` (abhängig von Distribution)
* Datei: `/etc/samba/smb.conf` muss korrekt konfiguriert sein
* Häufig als Root oder mit `sudo` ausführen
* Authentifizierung oft mit `-U user` erforderlich
