# /var

#### `/var` in Linux

Das Verzeichnis `/var` ist ein weiteres wichtiges Verzeichnis im Linux-Dateisystem. Es steht für "variable" Daten und enthält Dateien, deren Inhalt im Laufe der Zeit variiert. Das Verzeichnis wird hauptsächlich für Dateien verwendet, die sich im Betrieb häufig ändern, wie Log-Dateien, Caching-Daten, Spool-Dateien und temporäre Dateien.

#### Inhalt von `/var`:

Das Verzeichnis `/var` wird häufig in mehrere Unterverzeichnisse unterteilt, die bestimmte Arten von variablen Daten enthalten. Hier sind die wichtigsten Unterverzeichnisse und deren Inhalte:

* **`/var/log`**: Enthält Log-Dateien, die von verschiedenen Programmen und Systemdiensten erstellt werden. Diese Dateien sind für die Fehlerbehebung und Systemüberwachung unerlässlich. Hier finden sich Logs von Systemdiensten, Anwendungen, Sicherheitsprotokolle und vieles mehr.
* **`/var/spool`**: Beinhaltet Warteschlangen für Dienste wie Druckerwarteschlangen, E-Mail-Postfächer und andere Prozesse, die auf die Bearbeitung von Aufgaben warten. Hier können sich E-Mails, Druckaufträge und andere wartende Aufgaben befinden, die noch verarbeitet werden müssen.
* **`/var/cache`**: Speichert temporäre Cache-Daten, die von Programmen und Anwendungen verwendet werden, um die Leistung zu verbessern. Dies kann Daten beinhalten, die wiederverwendet werden, ohne dass sie erneut heruntergeladen oder berechnet werden müssen.
* **`/var/tmp`**: Enthält temporäre Dateien, die über System-Neustarts hinweg erhalten bleiben. Im Gegensatz zu `/tmp`, wo temporäre Daten nach einem Neustart gelöscht werden, werden Dateien in `/var/tmp` normalerweise nicht entfernt.
* **`/var/lib`**: Beinhaltet variable Daten für bestimmte Programme und Dienste. Diese Daten können von Datenbanken, Paketmanagern und anderen Anwendungen verwendet werden. Hier speichern Programme persistent Daten, die sie über die Zeit hinweg benötigen.
* **`/var/run`**: Früher auch als `/var/lock` bezeichnet, enthält es PID-Dateien (Process ID), Sockets und andere Informationen, die während der Systemlaufzeit verwendet werden. Diese Dateien enthalten temporäre Informationen über laufende Prozesse und Dienste.
* **`/var/mail`**: Hier werden E-Mails für Benutzer gespeichert. Es handelt sich um das Standardverzeichnis für E-Mail-Postfächer, wenn der lokale E-Mail-Dienst (wie `sendmail` oder `postfix`) verwendet wird.

#### Verwendungszweck von `/var`:

* **Protokollierung und Systemüberwachung**: Das Verzeichnis `/var/log` ist ein zentraler Ort für Systemprotokolle. Diese Logs sind entscheidend für die Überwachung des Systems, das Debugging und die Sicherheitsüberprüfung.
* **Warteschlangen und Spool-Daten**: Das Verzeichnis `/var/spool` enthält Dateien, die in Warteschlangen auf ihre Verarbeitung warten. Dies können E-Mails, Druckaufträge oder andere laufende Aufgaben sein.
* **Cache-Daten für verbesserte Leistung**: `/var/cache` wird von Programmen verwendet, um Daten zwischenzuspeichern, die sie wiederverwenden möchten, um die Leistung zu verbessern, ohne sie erneut herunterladen oder berechnen zu müssen.
* **Temporäre Dateien**: `/var/tmp` speichert temporäre Dateien, die zwischen Neustarts des Systems erhalten bleiben sollen. Diese Dateien können von Anwendungen oder Systemprozessen für die Übergangszeit benötigt werden.
* **Persistente Anwendungsdaten**: `/var/lib` enthält Daten, die von Anwendungen persistent gespeichert werden, etwa von Datenbanken oder Paketmanagern.

#### Eigenschaften von `/var`:

* **Dynamische Daten**: Im Gegensatz zu den meisten anderen Verzeichnissen im Linux-Dateisystem enthält `/var` hauptsächlich Daten, die sich im Laufe der Zeit ändern. Daher ist es ein "variables" Verzeichnis.
* **Schreibgeschützte Verzeichnisse für Benutzer**: Viele Dateien und Verzeichnisse in `/var` sind für normale Benutzer nicht schreibbar, insbesondere in Verzeichnissen wie `/var/log` oder `/var/spool`, da sie sicherstellen müssen, dass nur bestimmte Prozesse oder Administratoren auf diese Daten zugreifen und sie ändern können.
* **Wichtige Systeminformationen**: Viele der Daten in `/var`, insbesondere Logs und PID-Dateien, sind entscheidend für die ordnungsgemäße Verwaltung und den Betrieb des Systems. Das Überwachen dieser Dateien kann bei der Fehlerbehebung und der Verfolgung von Sicherheitsereignissen helfen.

#### Wichtige Aspekte:

* **Protokolle und Fehlerbehebung**: Viele Systemprotokolle in `/var/log` werden verwendet, um Fehler und Sicherheitsprobleme zu überwachen. Administratoren greifen oft auf diese Protokolle zu, um Informationen über Systemereignisse oder Sicherheitsverletzungen zu sammeln.
* **Datenbanken und Spool-Systeme**: Das Verzeichnis `/var/lib` wird von Programmen verwendet, um Daten über längere Zeiträume hinweg zu speichern. Diese Daten können von Paketmanagern oder Datenbankdiensten verwendet werden.
* **Sicherheitsbedenken**: Da viele der Daten in `/var` sensibel sind, insbesondere Protokolle und E-Mail-Daten, ist es wichtig, auf die Berechtigungen und den Zugriff auf diese Verzeichnisse zu achten, um unbefugte Änderungen oder Ausspähungen zu verhindern.

