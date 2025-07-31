# SMTP

```bash
# Verbindung zum POP3-Server auf Port 110 (unverschlüsselt)
nc mail.example.com 110

# Server begrüßt den Client
+OK POP3 server ready

# Benutzername senden
USER max.mustermann
+OK User accepted

# Passwort senden
PASS geheim123
+OK User successfully logged on

# Anzahl und Gesamtdatenmenge der E-Mails abfragen
STAT
+OK 2 3456

# Nachrichtenübersicht anzeigen
LIST
+OK 2 messages (3456 octets)
1 1234
2 2222

# Nachricht 1 abrufen
RETR 1
+OK 1234 octets
[Nachrichtentext folgt...]

# Nachricht 2 löschen (markiert zur Löschung nach QUIT)
DELE 2
+OK message 2 deleted

# Verbindung ordnungsgemäß beenden und markierte Nachrichten löschen
QUIT
+OK Goodbye
```
