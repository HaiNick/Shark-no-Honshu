---
description: Netzwerkverbindungen, Benutzerverwaltung, Dienstesteuerung
---

# net

{% hint style="info" %}
Alle `net`-Befehle müssen **mit Adminrechten** ausgeführt werden, wenn sie Änderungen vornehmen.

In Domänenumgebungen werden einige Befehle (z. B. `net group`) über den Domain Controller verwaltet.
{% endhint %}

### **Netzlaufwerke und Netzwerkressourcen**

| Befehl                                  | Beschreibung                                           |
| --------------------------------------- | ------------------------------------------------------ |
| `net use`                               | Verbindungen zu Netzlaufwerken oder Druckern verwalten |
| `net use [Laufwerk]: \\Server\Freigabe` | Laufwerk verbinden                                     |
| `net use /delete`                       | Verbindung trennen                                     |
| `net view`                              | Zeigt Computer/Freigaben im Netzwerk                   |
| `net view \\Computername`               | Zeigt Freigaben eines bestimmten Computers             |

### **Benutzer- und Gruppenverwaltung**

| Befehl                                       | Beschreibung                              |
| -------------------------------------------- | ----------------------------------------- |
| `net user`                                   | Benutzerkonten anzeigen                   |
| `net user [Benutzer] [Passwort] /add`        | Benutzer hinzufügen                       |
| `net user [Benutzer] /delete`                | Benutzer entfernen                        |
| `net localgroup`                             | Lokale Gruppen anzeigen                   |
| `net localgroup [Gruppe] [Benutzer] /add`    | Benutzer zur Gruppe hinzufügen            |
| `net localgroup [Gruppe] [Benutzer] /delete` | Benutzer aus Gruppe entfernen             |
| `net group`                                  | Domänengruppen verwalten (nur in Domänen) |

### &#x20;**Dienste verwalten**

| Befehl                      | Beschreibung                              |
| --------------------------- | ----------------------------------------- |
| `net start`                 | Startet einen Dienst                      |
| `net stop`                  | Stoppt einen Dienst                       |
| `net pause [Dienstname]`    | Pausiert einen Dienst (falls unterstützt) |
| `net continue [Dienstname]` | Fortsetzen eines pausierten Dienstes      |

### &#x20;**Druckerverwaltung**

| Befehl                                               | Beschreibung               |
| ---------------------------------------------------- | -------------------------- |
| `net print [\\Computer\Drucker]`                     | Zeigt Druckerwarteschlange |
| `net print \\Computer\Warteschlange [JobID] /delete` | Druckauftrag löschen       |

### &#x20;**Freigaben verwalten**

| Befehl                                             | Beschreibung           |
| -------------------------------------------------- | ---------------------- |
| `net share`                                        | Zeigt lokale Freigaben |
| `net share Freigabename=Pfad /grant:Benutzer,FULL` | Ordner freigeben       |
| `net share Freigabename /delete`                   | Freigabe entfernen     |

### **Domäne, Workgroup & Anmeldung**

| Befehl                             | Beschreibung                                     |
| ---------------------------------- | ------------------------------------------------ |
| `net config workstation`           | Zeigt Konfiguration des Arbeitsplatzes           |
| `net config server`                | Zeigt Serverkonfiguration                        |
| `net computer \\Computer /add`     | Computer zur Domäne hinzufügen                   |
| `net accounts`                     | Richtlinien für Benutzerkonten anzeigen/anpassen |
| `net accounts /maxpwage:unlimited` | Maximale Kennwortgültigkeit setzen               |

### &#x20;**Sitzungen & Anmeldungen**

| Befehl                           | Beschreibung                   |
| -------------------------------- | ------------------------------ |
| `net session`                    | Zeigt aktive Sitzungen         |
| `net session \\Computer /delete` | Sitzung trennen                |
| `net file`                       | Zeigt geöffnete Dateien        |
| `net file [ID] /close`           | Geöffnete Datei schließen      |
| `net logoff [Sitzung]`           | Benutzer abmelden (nur Server) |

### &#x20;**Sonderfunktionen**

| Befehl                          | Beschreibung                                 |
| ------------------------------- | -------------------------------------------- |
| `net help`                      | Übersicht über verfügbare `net`-Unterbefehle |
| `net help [Befehl]`             | Hilfe zu einem bestimmten Unterbefehl        |
| `net statistics workstation`    | Zeigt Netzwerkstatistiken (Client)           |
| `net statistics server`         | Zeigt Netzwerkstatistiken (Server)           |
| `net time \\Computer`           | Uhrzeit eines Netzrechners anzeigen          |
| `net time \\Computer /set /yes` | Uhrzeit mit Netzrechner synchronisieren      |
