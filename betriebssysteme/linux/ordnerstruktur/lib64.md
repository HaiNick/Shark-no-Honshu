# /lib64

`/lib64` ist ein Verzeichnis im Linux-Dateisystem, das speziell für 64-Bit-Bibliotheken auf 64-Bit-Architekturen vorgesehen ist. Es enthält die 64-Bit-Versionen der Systembibliotheken, die für die Ausführung von 64-Bit-Anwendungen notwendig sind.

#### Inhalt von `/lib64`:

* **64-Bit-Versionen von Bibliotheken**: Auf 64-Bit-Systemen enthält `/lib64` die Systembibliotheken, die für die Ausführung von 64-Bit-Anwendungen erforderlich sind. Diese Bibliotheken bieten grundlegende Funktionen, wie etwa Systemaufrufe, Speicherverwaltung und Netzwerkdienste, für 64-Bit-Prozesse.
  * **64-Bit C-Bibliothek (`libc.so`)**: Diese Bibliothek stellt grundlegende Funktionen für 64-Bit-Anwendungen zur Verfügung, wie etwa Funktionen zur Speicherverwaltung, Dateioperationen und Systemaufrufe.
  * **Dynamische Bibliotheken**: Diese Bibliotheken im `.so`-Format werden zur Laufzeit von Programmen geladen und bieten eine Vielzahl an Funktionalitäten, die nicht im Programmkode selbst enthalten sein müssen.
* **`/lib64` und die Trennung von 32-Bit und 64-Bit**: In Systemen, die sowohl 32-Bit- als auch 64-Bit-Bibliotheken unterstützen, sorgt das Verzeichnis `/lib64` dafür, dass 64-Bit-Anwendungen nur mit den richtigen, für ihre Architektur vorgesehenen Bibliotheken arbeiten. Auf 64-Bit-Systemen gibt es meist auch das Verzeichnis `/lib32`, das 32-Bit-Bibliotheken enthält, um Kompatibilität mit 32-Bit-Anwendungen zu gewährleisten.

#### Verwendung von `/lib64`:

* **Für 64-Bit-Programme**: `/lib64` enthält die Bibliotheken, die 64-Bit-Anwendungen für grundlegende Funktionen benötigen, wie etwa Dateisystemzugriff, Netzwerkkommunikation und Speicherverwaltung.
* **Kompatibilität und Multiplattform**: In einem Linux-System, das sowohl 64-Bit- als auch 32-Bit-Anwendungen unterstützt (Multilib-Systeme), werden die 64-Bit-Bibliotheken in `/lib64` und die 32-Bit-Bibliotheken in `/lib32` abgelegt. Dadurch wird sichergestellt, dass jede Art von Anwendung die richtigen Versionen der Bibliotheken verwendet.

#### Wichtige Aspekte:

* **Berechtigungen und Sicherheit**: Das Verzeichnis `/lib64` enthält wichtige systemweite Bibliotheken, und daher haben nur privilegierte Benutzer (wie `root`) Schreibzugriff auf dieses Verzeichnis. Der Zugriff und die Änderungen an den Dateien in `/lib64` sollten kontrolliert und sicher durchgeführt werden, da fehlerhafte Änderungen die Funktionsweise des gesamten Systems beeinträchtigen könnten.
* **Updates**: Wie auch bei anderen systemkritischen Bibliotheken werden Updates für die in `/lib64` enthaltenen Bibliotheken über das Paketverwaltungssystem (wie `apt`, `yum` oder `pacman`) durchgeführt. Dies sorgt dafür, dass die richtigen Versionen der Bibliotheken immer aktuell und sicher sind.
* **Stabilität des Systems**: Da Bibliotheken in `/lib64` für den Betrieb des Systems und für Anwendungen erforderlich sind, hat jede Änderung oder jeder Fehler in diesen Dateien das Potenzial, das gesamte System zu destabilisieren oder die Funktionsfähigkeit von Anwendungen zu beeinträchtigen.
