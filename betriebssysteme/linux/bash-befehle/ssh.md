# ssh



```
# SSH-Schlüssel generieren
ssh-keygen
```

## ssh-authorized keys - Login ohne Passwort



Der Befehl `ssh-copy-id` wird verwendet, um den öffentlichen Teil eines SSH-Schlüsselpaares auf einen entfernten Server zu kopieren und ihn der Datei `~/.ssh/authorized_keys` hinzuzufügen, um eine Passwortlose Authentifizierung zu ermöglichen.

#### `ssh-copy-id -i ~/.ssh/id_rsa.pub username@remote_host`

* `ssh-copy-id`: Dies ist ein Shell-Skript, das häufig auf Unix-ähnlichen Systemen (Linux, macOS usw.) verfügbar ist. Es ist dafür gedacht, den öffentlichen Teil eines SSH-Schlüsselpaares auf einen entfernten Server zu kopieren und ihn der Liste der autorisierten Schlüssel hinzuzufügen.
* `-i ~/.ssh/id_rsa.pub`: Dies ist die Option, die den Pfad zur öffentlichen Schlüsseldatei angibt, die kopiert werden soll. Standardmäßig wird `~/.ssh/id_rsa.pub` verwendet, wenn keine Datei explizit angegeben wird. Hier wird davon ausgegangen, dass sich Ihr öffentlicher Schlüssel in der Datei `~/.ssh/id_rsa.pub` befindet. Wenn Sie einen anderen Schlüssel verwenden möchten, können Sie den Pfad zur entsprechenden `.pub`-Datei angeben.
* `username@remote_host`: Dies sind die Anmeldeinformationen für den entfernten Server, zu dem Sie eine Passwortlose SSH-Verbindung herstellen möchten. `username` ist Ihr Benutzername auf dem entfernten Server und `remote_host` ist die IP-Adresse oder der Hostname des entfernten Servers.

#### Funktionsweise:

1. **Verbindung zum entfernten Server**: `ssh-copy-id` stellt eine SSH-Verbindung zum entfernten Server her, indem es versucht, sich mit den angegebenen Anmeldeinformationen (`username@remote_host`) zu authentifizieren.
2. **Kopieren des öffentlichen Schlüssels**: Wenn die Verbindung erfolgreich ist und der öffentliche Schlüssel (`~/.ssh/id_rsa.pub`) noch nicht in der Datei `~/.ssh/authorized_keys` auf dem entfernten Server vorhanden ist, wird der öffentliche Schlüssel auf den Server kopiert und an das Ende der Datei `~/.ssh/authorized_keys` angehängt.
3. **Berechtigungen**: Nach dem Hinzufügen des Schlüssels überprüft `ssh-copy-id` die Berechtigungen der `.ssh`-Verzeichnisse und der `authorized_keys`-Datei auf dem entfernten Server. Diese sollten so eingerichtet sein, dass nur der Besitzer (der Benutzer) Schreibzugriff hat, um die Sicherheit der SSH-Verbindung zu gewährleisten.

#### Vorteile von `ssh-copy-id`:

* **Einfache Einrichtung**: `ssh-copy-id` bietet eine einfache Möglichkeit, SSH-Schlüssel für Passwortlose Authentifizierung zu konfigurieren, ohne dass manuelle Eingriffe auf dem entfernten Server erforderlich sind.
* **Sicherheit**: Die Verwendung von SSH-Schlüsseln bietet eine höhere Sicherheit im Vergleich zu Passwörtern, da sie schwerer zu erraten oder zu knacken sind.
* **Bequemlichkeit**: Sobald der Schlüssel einmal auf dem Server hinzugefügt wurde, können Sie sich ohne Eingabe eines Passworts anmelden, was den Zugriff auf den Server effizienter macht.

