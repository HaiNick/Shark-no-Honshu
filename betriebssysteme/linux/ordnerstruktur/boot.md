# /boot

`/boot` ist ein Verzeichnis in Linux, das die für das System benötigten Dateien zum Starten des Betriebssystems enthält. Es speichert alle wichtigen Dateien, die erforderlich sind, damit der Bootloader und der Kernel korrekt geladen und ausgeführt werden können. Diese Dateien sind essentiell für den Systemstart und ermöglichen den Übergang von der Initialisierungsphase des Bootloaders hin zum vollständigen Betriebssystem.

#### Inhalt von `/boot`:

* **`vmlinuz`**: Dies ist die komprimierte Version des Linux-Kernels, der beim Starten des Systems verwendet wird. Der Kernel enthält die grundlegenden Funktionen des Systems, einschließlich Hardware-Treiber, Prozessverwaltung und Dateisystemzugriff.
* **`initrd` oder `initramfs`**: Dies ist das Initial Ramdisk-Image, das während des Systemstarts geladen wird. Es enthält notwendige Treiber und Tools, die vor dem Laden des eigentlichen Systems benötigt werden, insbesondere für das Erkennen von Festplatten und das Entpacken von Kerneldaten.
* **`grub`**: Dies ist der Bootloader, der für das Laden des Kernels und der Initialisierung des Systems verantwortlich ist. Es gibt Konfigurationsdateien, die den Bootprozess steuern und mehrere Betriebssysteme ermöglichen (z.B. beim Dual-Boot).
* **`config-<version>`**: Diese Datei enthält die Konfigurationsparameter für den verwendeten Kernel. Sie ist eine Textdatei, die angibt, welche Funktionen und Treiber beim Erstellen des Kernels aktiviert wurden.
* **`System.map`**: Diese Datei enthält Symbole und Adressen von Funktionen und Variablen im Kernel. Sie wird für Debugging und Kernel-Analysen verwendet.
* **`memtest86+`**: Ein Diagnosetool zum Testen des Arbeitsspeichers (RAM) auf Fehler.

#### Verwendung von `/boot`:

* **Systemstart**: `/boot` enthält alle Dateien, die notwendig sind, damit das System starten kann, wie den Kernel und den Bootloader. Ohne dieses Verzeichnis kann das System nicht erfolgreich gestartet werden.
* **Wartung und Fehlerbehebung**: In `/boot` sind oft Tools und Logs, die beim Troubleshooting von Boot-Problemen oder bei der Wiederherstellung eines fehlerhaften Systems hilfreich sind.
* **Mehrere Betriebssysteme**: Wenn mehrere Betriebssysteme auf einem Computer installiert sind (Dual-Boot), wird der Bootloader in `/boot` verwaltet, um den Benutzer bei der Auswahl des zu startenden Betriebssystems zu unterstützen.

#### Wichtige Aspekte:

* **Nur Lesezugriff**: Das Verzeichnis `/boot` ist oft schreibgeschützt, um versehentliche Änderungen zu verhindern, da es kritisch für das System ist.
* **Größe und Verwaltung**: Die Größe von `/boot` ist in der Regel begrenzt, da es nur die unbedingt erforderlichen Dateien enthält. Es ist wichtig, alte Kernel-Versionen zu entfernen, um den Platz zu verwalten und zu verhindern, dass das Verzeichnis zu groß wird.
