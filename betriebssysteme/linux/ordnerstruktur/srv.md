# /srv

#### `/srv` in Linux

Das Verzeichnis `/srv` ist ein standardmäßiges Verzeichnis auf Linux- und Unix-ähnlichen Systemen, das dazu dient, **Daten** und **Dienste** für verschiedene **Dienste** und **Serveranwendungen** zu speichern, die vom System angeboten werden. Der Name "`srv`" steht für **"service"** und bezeichnet einen Ort, an dem Daten für vom System bereitgestellte Dienste aufbewahrt werden.

#### Inhalt von `/srv`:

* **Daten für Dienste**: In `/srv` werden Daten abgelegt, die von verschiedenen Diensten oder Serveranwendungen benötigt werden. Diese Dienste könnten beispielsweise Webserver, FTP-Server, Datenbanken oder andere Netzwerkdienste sein.
* **Service-bezogene Dateien**: Hier befinden sich Dateien, die für einen spezifischen Dienst oder Server konzipiert sind, wie etwa Web-Inhalte für HTTP-Server, Daten für FTP-Server oder Datenbanken für Anwendungen.
* **Service-spezifische Verzeichnisse**: Verschiedene Subverzeichnisse in `/srv` können die Daten für unterschiedliche Dienste enthalten, zum Beispiel für einen Webserver wie Apache oder Nginx, einen FTP-Server oder für eine Datenbank wie MySQL.

#### Verwendungszweck von `/srv`:

* **Zentrale Ablage von Dienstdaten**: `/srv` dient als zentraler Speicherort für Daten, die spezifisch für die Dienste des Systems sind. Es ermöglicht eine strukturierte Organisation von Dienstdaten und stellt sicher, dass sie an einem konsistenten Ort gespeichert werden.
* **Standardisierung**: Das Verzeichnis `/srv` wurde im Filesystem Hierarchy Standard (FHS) definiert, um eine klare und standardisierte Trennung von Dienstdaten und anderen Dateisystemressourcen wie Systemdaten oder Benutzerdaten zu gewährleisten.
* **Zugriffsorganisation**: Dieses Verzeichnis trägt zur Trennung von Daten für verschiedene Dienste bei, was für die Verwaltung von großen Serverumgebungen oder Multidienstsystemen wichtig ist.

#### Beispiele für Inhalte in `/srv`:

* **Webserver-Daten**: Daten für einen Webserver wie HTML-Dateien, Skripte oder andere Ressourcen, die über HTTP oder HTTPS bereitgestellt werden.
  * Beispiel: `/srv/www/` für einen Webserver
* **FTP-Server-Daten**: Dateien, die über einen FTP-Server bereitgestellt werden.
  * Beispiel: `/srv/ftp/`
* **Datenbanken**: Datenbankdateien für Serveranwendungen wie MySQL oder PostgreSQL.
  * Beispiel: `/srv/mysql/`
* **Samba oder NFS**: Verzeichnisse für freigegebene Dateien, die über Samba (Windows-kompatibler Dateifreigabedienst) oder NFS (Network File System) verfügbar gemacht werden.
  * Beispiel: `/srv/samba/` oder `/srv/nfs/`

#### Beispiel:

Ein Webserver speichert seine Daten in einem speziellen Verzeichnis innerhalb von `/srv`:

```bash
ls /srv/www/
```

#### Wichtige Aspekte:

* **Normen und Konventionen**: Während das Verzeichnis `/srv` im FHS-Standard festgelegt ist, wird es nicht immer auf allen Linux-Distributionen verwendet. Einige Distributionen setzen stattdessen andere Verzeichnisse wie `/var/www` für Webserverdaten oder `/var/lib` für Dienstdaten ein.
* **Trennung von Daten**: Das Verzeichnis hilft dabei, Daten und Dienste klar zu trennen, was besonders bei der Verwaltung von Servern mit mehreren Diensten von Vorteil ist. Dies erleichtert die Wartung und Organisation der Serverumgebung.
* **Nicht-persistente Daten**: Daten, die in `/srv` gespeichert sind, sind in der Regel persistent und bleiben auch nach einem Neustart des Systems erhalten. Im Gegensatz zu `/run`, das flüchtige Daten speichert, sind Daten in `/srv` in der Regel für den langfristigen Einsatz gedacht.
