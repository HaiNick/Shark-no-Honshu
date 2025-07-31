# /proc

`/proc` ist ein spezielles Verzeichnis in Linux, das eine virtuelle Dateisystemstruktur enthält, die Informationen über den aktuellen Zustand des Systems und laufende Prozesse liefert. Es wird nicht von der Festplatte gespeichert, sondern existiert im RAM und wird dynamisch vom Kernel erstellt. Der Inhalt von `/proc` ermöglicht den Zugriff auf systemrelevante Daten und stellt ein Interface für die Interaktion mit dem Kernel bereit.

#### Wichtige Verzeichnisse und Dateien in `/proc`:

* **`/proc/[pid]`**: Enthält Informationen über einen spezifischen laufenden Prozess, wobei `[pid]` die Prozess-ID ist. Diese Informationen umfassen Details wie Status, CPU-Auslastung, offene Dateien und Speichernutzung.
* **`/proc/cpuinfo`**: Zeigt Informationen über den Prozessor, wie Modell, Anzahl der Kerne und Architektur.
* **`/proc/meminfo`**: Bietet detaillierte Informationen über die Speichernutzung des Systems.
* **`/proc/uptime`**: Zeigt die Systemlaufzeit sowie die Leerlaufzeit des Systems.
* **`/proc/version`**: Zeigt die Kernel-Version des Systems an.

#### Verwendung von `/proc`:

* Überwachung und Diagnose von Systemressourcen und Prozessen
* Abrufen von Kernel- und Hardwareinformationen
* Systemadministration und Troubleshooting

Da `/proc` eine virtuelle Struktur ist, speichert sie keine echten Dateien und stellt stattdessen Schnittstellen zum Kernel bereit, die bei Bedarf aktualisiert werden.

{% embed url="https://docs.kernel.org/filesystems/proc.html" %}
