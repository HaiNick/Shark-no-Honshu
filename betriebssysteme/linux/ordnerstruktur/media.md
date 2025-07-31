# /media

`/media` ist ein Verzeichnis in Linux- und Unix-Dateisystemen, das für **Wechselmedien** und **externe Speichermedien** wie USB-Sticks, externe Festplatten, CD/DVDs und andere Speichermedien verwendet wird, die vom System eingehängt (gemountet) werden.

#### Inhalt von `/media`:

* **Wechselmedien**: Das Verzeichnis `/media` dient hauptsächlich als Mount-Punkt für Wechseldatenträger. Wenn ein USB-Laufwerk, eine CD/DVD oder eine andere externe Speichereinheit in das System eingehängt wird, erscheint es in der Regel als Unterverzeichnis in `/media`. Zum Beispiel könnte ein USB-Stick, der eingehängt wurde, als `/media/usb` oder `/media/NAME_DES_LAUFWERKS` erscheinen.
* **Automatisch erzeugte Unterverzeichnisse**: In modernen Linux-Distributionen wird das Verzeichnis `/media` typischerweise dynamisch verwaltet. Wenn eine neue Datei oder ein neues Medium angeschlossen wird, erstellt das System automatisch ein Verzeichnis unter `/media`, das den Namen des Geräts oder einen durch den Benutzer angegebenen Namen trägt. Zum Beispiel könnte beim Einhängen einer Festplatte der Ordner `/media/username/label` erstellt werden.

#### Verwendungszweck von `/media`:

* **Mount-Punkte für externe Geräte**: Der primäre Zweck des Verzeichnisses `/media` ist es, als zentrale Stelle für alle externen Geräte zu dienen, die an das System angeschlossen werden. Dies umfasst Wechseldatenträger wie USB-Sticks, externe Festplatten, optische Medien (CD/DVDs) und Netzwerkfreigaben.
* **Automatisches Einhängen**: Viele Linux-Distributionen verwenden heute **automatische Erkennung** und **automatisches Einhängen** von Geräten, was bedeutet, dass beim Anschließen eines Geräts automatisch ein Mount-Punkt im Verzeichnis `/media` erstellt wird. Einhängepunkte sind hier der Ort, an dem die Inhalte des externen Mediums zugänglich gemacht werden.
* **Benutzerfreundlichkeit**: Im Gegensatz zu `/mnt`, das in der Vergangenheit als allgemeiner Mount-Punkt verwendet wurde, wird `/media` speziell für externe Medien genutzt. Dies trägt zur Benutzerfreundlichkeit bei, indem es für den Benutzer klar macht, dass sich dort externe Geräte befinden.

#### Wichtige Aspekte:

* **Automatische Erkennung und Einhängen**: Viele Desktop-Umgebungen und Linux-Distros bieten die Funktion der automatischen Erkennung und des automatischen Einhängens von Medien, die beim Anstecken eines USB-Sticks oder einer CD/DVD das zugehörige Verzeichnis im `/media`-Verzeichnis erstellen. Der Benutzer kann dann direkt auf das Gerät zugreifen.
* **Zugriffsrechte**: Die Berechtigungen auf Verzeichnisse innerhalb von `/media` können variieren, je nachdem, wie das Gerät eingebunden wurde und welche Benutzer darauf zugreifen sollen. Im Allgemeinen hat der Benutzer, der das Gerät eingehängt hat, Lese- und Schreibzugriff auf das Gerät. Auch andere Benutzer oder Gruppen können Zugriffsrechte auf diese Medien haben.
* **Verwendet für unterschiedliche Dateisysteme**: Das `/media`-Verzeichnis ist flexibel und unterstützt eine Vielzahl von Dateisystemen, die auf externen Geräten verwendet werden, wie `FAT32`, `NTFS`, `ext4` oder `exFAT`. Das System kümmert sich darum, die entsprechenden Dateisystemtreiber zu laden, um das angeschlossene Gerät zugänglich zu machen.
* **Unterschied zu `/mnt`**: Während `/mnt` ebenfalls als Mount-Punkt genutzt werden kann, wird es traditionell für **temporäre** oder **manuelle** Einhängepunkte verwendet, während `/media` speziell für **automatisch eingehängte externe Medien** gedacht ist. In vielen modernen Systemen ist es jedoch so, dass Geräte sowohl manuell als auch automatisch in `/mnt` oder `/media` eingehängt werden können.

#### Beispiele:

* Beim Einstecken eines USB-Sticks kann das Gerät in das Verzeichnis `/media/username/usb_drive` gemountet werden.
* Wenn eine CD/DVD in das Laufwerk eingelegt wird, kann sie unter `/media/username/label` zugänglich gemacht werden.
