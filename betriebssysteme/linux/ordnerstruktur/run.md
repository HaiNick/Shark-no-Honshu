# /run

#### `/run` in Linux

Das Verzeichnis `/run` ist ein **systemweites Verzeichnis**, das in modernen Linux-Distributionen dazu dient, **temporäre Laufzeitdaten** zu speichern. Es enthält Informationen und Daten, die zur Ausführung des Systems während seiner Laufzeit benötigt werden. Diese Daten sind vor allem für **Systemprozesse** und **Dienste** erforderlich, die im laufenden Betrieb erzeugt werden, aber nicht dauerhaft gespeichert werden müssen.

#### Inhalt von `/run`:

* **Laufzeitdaten**: `/run` speichert temporäre Dateien, die nur während der Systemlaufzeit benötigt werden, wie z.B. PID-Dateien (Process ID), Lock-Dateien, Sockets und temporäre Systeminformationen.
* **Prozessinformationen**: In diesem Verzeichnis werden Dateien wie PID-Dateien abgelegt, die die Prozess-IDs von laufenden Systemdiensten oder Anwendungen enthalten.
* **Sockets und FIFO-Dateien**: Hier können auch Unix-Socket-Dateien und FIFOs (First In, First Out) abgelegt werden, die für die Interprozesskommunikation (IPC) genutzt werden.
* **Temporäre Logs und Systemstatusinformationen**: Hier können auch bestimmte Logs oder Statusinformationen abgelegt werden, die nur während der aktuellen Sitzung von Bedeutung sind.

#### Verwendungszweck von `/run`:

* **Laufzeitdaten**: `/run` ist dafür zuständig, Informationen zu speichern, die während der Systemlaufzeit benötigt werden, aber nicht persistent sind. Es wird hauptsächlich von Systemprozessen, Daemons und laufenden Anwendungen verwendet.
* **Übergang von `/var/run` zu `/run`**: Früher wurden Laufzeitdaten im Verzeichnis `/var/run` gespeichert. In modernen Systemen wurde dieses Verzeichnis durch `/run` ersetzt. `/run` ist ein temporäres Verzeichnis, das während des Systemstarts erstellt wird und in den meisten Fällen im RAM abgelegt wird. Daher werden die darin enthaltenen Daten bei einem Neustart des Systems gelöscht.
* **Daten für laufende Dienste**: Verschiedene Dienste (z.B. Systemd, Netzwerkdienste) verwenden `/run`, um ihre temporären Dateien zu speichern, darunter auch den Status des Dienstes und Konfigurationsinformationen.

#### Wichtige Dateien und Unterverzeichnisse in `/run`:

* **`/run/lock`**: Enthält Dateien, die als Lock-Dateien verwendet werden, um sicherzustellen, dass bestimmte Prozesse oder Dienste nicht gleichzeitig ausgeführt werden.
* **`/run/user/<UID>`**: Dieses Verzeichnis enthält temporäre Dateien für Benutzer, die zur Laufzeit des Systems angemeldet sind. Jedes Benutzerkonto hat ein eigenes Verzeichnis unter `/run/user`, in dem temporäre Dateien wie Sockets und Cache-Dateien abgelegt werden.
* **`/run/systemd/`**: Dieses Verzeichnis enthält temporäre Dateien, die von Systemd verwaltet werden, wie z.B. Sockets und Statusinformationen für laufende Systemd-Dienste.

#### Beispiel:

*   **PID-Datei eines laufenden Dienstes**:

    ```bash
    cat /run/nginx.pid
    ```
*   **Datei für einen Benutzerprozess (z.B. Socket)**:

    ```bash
    ls /run/user/1000/
    ```

#### Wichtige Aspekte:

* **Volatilität**: Der Inhalt von `/run` ist flüchtig, da es normalerweise im RAM gespeichert wird. Das bedeutet, dass alle Dateien und Daten in `/run` nach einem Neustart des Systems verloren gehen. Es handelt sich also um temporäre und nicht-persistente Daten.
* **Unterschied zu `/var`**: Während `/var` für persistente, variable Daten wie Logs, Datenbanken und Cache-Dateien verwendet wird, ist `/run` ausschließlich für temporäre Laufzeitdaten vorgesehen, die bei einem Neustart nicht erhalten bleiben müssen.
* **Systemstart**: Das Verzeichnis `/run` wird in der Regel sehr früh im Bootprozess vom System selbst erzeugt, während `/var/run` weiterhin für Kompatibilitätsszenarien existieren kann.
