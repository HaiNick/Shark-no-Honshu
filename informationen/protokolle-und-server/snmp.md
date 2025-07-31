---
description: Simple Network Management Protocol
---

# SNMP

SNMP ist ein Protokoll zur Überwachung und Verwaltung von Netzwerkgeräten wie Routern, Switches, Servern und Druckern. Es verwendet standardmäßig UDP-Port 161 für Anfragen und UDP-Port 162 für Traps (Benachrichtigungen). SNMP ermöglicht das Abfragen von Gerätestatus, Konfigurationen und Leistungsdaten über sogenannte Management Information Bases (MIBs). Für Pentester ist SNMP interessant, weil schlecht gesicherte Community-Strings (wie "public") oft Informationen über das Netzwerk preisgeben oder sogar Konfigurationsänderungen erlauben können. Blue Teams sollten SNMP überwachen und absichern, um potenzielle Angriffsflächen zu minimieren.
