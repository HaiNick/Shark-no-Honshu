# /mnt

#### `/mnt` in Linux

Das Verzeichnis `/mnt` in Linux und anderen Unix-basierten Systemen ist ein Standardverzeichnis, das traditionell als allgemeiner **Mount-Punkt** für Dateisysteme verwendet wird, die nicht automatisch von Systemdiensten wie USB-Sticks oder externen Festplatten gemountet werden. Es wird üblicherweise für **temporäre** oder **manuelle** Mounts verwendet.

#### Inhalt von `/mnt`:

* **Manuelle Mounts**: Das Verzeichnis `/mnt` wird oft für das manuelle Einhängen von Dateisystemen verwendet, z.B. beim Mounten einer externen Festplatte, einer Netzwerkfreigabe oder eines temporären Dateisystems, das nicht automatisch eingebunden wird.
* **Temporäre Mounts**: Es kann auch verwendet werden, um temporäre Dateisysteme oder Installations-Mounts für spezielle Aufgaben zu erstellen (z.B. bei einer Systeminstallation oder bei Wartungsarbeiten).

#### Verwendungszweck von `/mnt`:

* **Allgemeiner Mount-Punkt**: In älteren oder traditionellen Systemen war `/mnt` der allgemeine Ort für alle Mount-Punkte, sowohl für manuelle als auch für automatische Einhängepunkte. In modernen Linux-Distributionen wird dieses Verzeichnis jedoch meist für manuell gemountete Dateisysteme und Volumes verwendet, während `/media` für automatisch eingehängte Wechseldatenträger verwendet wird.
* **Temporäre Verwendung**: `/mnt` eignet sich hervorragend für temporäre Mount-Punkte, die nicht dauerhaft sein müssen, z.B. für das temporäre Mounten eines ISO-Images oder das Hinzufügen von externen Laufwerken, die für bestimmte Aufgaben benötigt werden.

#### Inhalt von `/mnt` in modernen Systemen:

* **Benutzerdefinierte Unterverzeichnisse**: Häufig wird `/mnt` von Administratoren verwendet, um benutzerdefinierte Unterverzeichnisse zu erstellen, wie z.B. `/mnt/external` oder `/mnt/backup`, wenn spezielle externe Datenträger oder Netzwerkfreigaben gemountet werden. Diese Verzeichnisse sind dann als Mount-Punkte für externe Quellen gedacht.
* **Keine automatische Verwaltung**: Im Gegensatz zu `/media`, das normalerweise für automatische Mount-Punkte von externen Geräten verwendet wird (wie USB-Sticks oder CDs/DVDs), ist `/mnt` nicht automatisch verwaltet. Administratoren müssen hier die Geräte manuell einhängen.

#### Wichtige Aspekte:

* **Manuelles Einhängen**: Der Hauptunterschied zwischen `/mnt` und `/media` besteht darin, dass `/mnt` in der Regel für manuelle Mounts verwendet wird. Das bedeutet, dass ein Administrator oder Benutzer das Gerät explizit einhängen muss, anstatt dass das System dies automatisch tut, wie es bei `/media` der Fall ist.
* **Nicht für Wechseldatenträger**: Während `/media` speziell für Wechseldatenträger und externe Geräte wie USB-Sticks und CDs/DVDs gedacht ist, wird `/mnt` eher für das Einhängen von Festplatten, Netzwerkfreigaben, temporären Dateisystemen und anderen manuell verwalteten Ressourcen verwendet.

#### Beispiele:

*   **Mounten einer externen Festplatte**:

    ```bash
    mount /dev/sdb1 /mnt/external
    ```
*   **Mounten eines ISO-Images**:

    ```bash
    mount -o loop /path/to/file.iso /mnt/iso
    ```
*   **Mounten einer Netzwerkfreigabe (NFS)**:

    ```bash
    mount -t nfs server:/path/to/share /mnt/nfs_share
    ```

#### Wichtige Hinweise:

* **Berechtigungen**: Da `/mnt` häufig manuell verwendet wird, können die Berechtigungen auf Unterverzeichnisse von `/mnt` variieren. Sie können so konfiguriert werden, dass nur Administratoren oder bestimmte Benutzer Zugriff darauf haben, um sicherzustellen, dass nur autorisierte Benutzer manuell eingehängte Geräte oder Dateisysteme nutzen können.
* **Keine Standardautomount-Funktionalität**: Im Gegensatz zu `/media` gibt es keine automatische Erkennung und das automatische Einhängen von Geräten im Verzeichnis `/mnt`. Alles, was dort gemountet wird, muss manuell geschehen.

