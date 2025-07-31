# /sys

#### `/sys` in Linux

Das Verzeichnis `/sys` ist ein **virtuelles Dateisystem** in Linux, das von der Kernel- und Systemumgebung verwendet wird, um Informationen über den aktuellen Zustand des Systems bereitzustellen und Änderungen daran vorzunehmen. Es stellt **Systeminformationen** und **Systemkonfigurationsdaten** zur Verfügung, die dynamisch von den laufenden Prozessen und dem Kernel erzeugt werden. In der Regel wird es für die **Überwachung und Verwaltung** des Kernels und der Hardware verwendet.

#### Inhalt von `/sys`:

* **System- und Hardwareinformationen**: `/sys` enthält Informationen zu den verschiedenen **Hardwarekomponenten**, die vom System erkannt wurden, einschließlich Prozessoren, Speicher, Geräte und Kernel-Module.
* **Kernel- und Gerätekonfiguration**: Es ermöglicht das Ändern und Steuern von **Kernelfunktionen**, wie zum Beispiel das Anpassen von **Geräteeigenschaften** oder das Aktivieren und Deaktivieren von Kernel-Modulen.
* **Virtuelle Schnittstellen**: Die Dateien im `/sys`-Verzeichnis sind oft **virtuell**, das heißt, sie sind nicht echte Dateien auf der Festplatte, sondern bieten eine Schnittstelle zur Kommunikation mit dem Kernel. Änderungen an diesen "Dateien" wirken sich direkt auf den Kernel und das System aus.

#### Verwendungszweck von `/sys`:

* **Dynamische Systemkonfiguration**: Man kann in `/sys` nach Geräten und Systemkomponenten suchen, die aktiv sind, und dynamisch Einstellungen anpassen, ohne das System neu starten zu müssen.
* **Überwachung von Hardware und Kernel**: Das Verzeichnis dient als Schnittstelle, um detaillierte Informationen über **Hardwarekomponenten** wie CPUs, RAM, Netzwerkgeräte und Festplatten zu erhalten.
* **Geräteverwaltung**: Es stellt eine Schnittstelle zur Verfügung, über die Benutzer und Administratoren mit der Systemhardware interagieren und sie steuern können. Beispielsweise lässt sich hier die Konfiguration von Geräten ändern oder Treiber laden.

#### Beispiele für Inhalte in `/sys`:

* **`/sys/class/`**: Hier werden Geräte des Systems nach Kategorien (wie z.B. `net` für Netzwerkgeräte, `block` für Blockgeräte, `cpu` für Prozessoren) organisiert.
  * Beispiel: `/sys/class/net/` enthält Informationen zu den Netzwerkschnittstellen.
* **`/sys/devices/`**: Enthält Informationen zu den physischen Geräten im System. Dies umfasst sowohl die Hardware als auch virtuelle Geräte.
  * Beispiel: `/sys/devices/system/cpu/` enthält Informationen zu den CPU-Kernen des Systems.
* **`/sys/kernel/`**: Hier befinden sich Dateien, die mit dem Kernel selbst verbunden sind, zum Beispiel zur Konfiguration von Kernel-Parametern.
  * Beispiel: `/sys/kernel/debug/` für Debugging-Informationen.
* **`/sys/block/`**: Informationen zu Blockgeräten wie Festplatten und Partitionen.
  * Beispiel: `/sys/block/sda/` enthält Details zur Festplatte `sda`.

#### Beispiel:

Informationen zu den Prozessoren des Systems erhalten:

```bash
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
```

#### Wichtige Aspekte:

* **Virtuelles Dateisystem**: Das `/sys`-Verzeichnis ist virtuell, was bedeutet, dass es keine echten Dateien auf der Festplatte enthält. Stattdessen handelt es sich um ein **dynamisches** Interface zwischen Benutzerlandprozessen und dem Kernel.
* **Systemkonfiguration und -steuerung**: Mit `/sys` können Administratoren und Programme tiefgreifende Änderungen an der Hardwarekonfiguration und an Kernel-Parametern vornehmen, oft ohne Neustart des Systems.
* **Nur für erfahrene Benutzer**: Wegen des Einflusses auf das System ist `/sys` für normale Benutzer in der Regel nicht zugänglich. Änderungen an Dateien in `/sys` können das Verhalten des Systems erheblich beeinflussen und sollten mit Vorsicht vorgenommen werden.
* **Änderungen wirken sofort**: Da es sich um ein virtuelles Dateisystem handelt, können Änderungen sofort wirksam werden und erfordern nicht den Neustart von Diensten oder des Systems.
