# Dateiberechtigungen und Attribute

### Grundlegende Dateiberechtigungen in Unix

Unix-basierte Betriebssysteme verwenden ein Berechtigungssystem, um den Zugriff auf Dateien und Verzeichnisse zu kontrollieren. Diese Berechtigungen bestimmen, welcher Benutzer was mit einer Datei oder einem Verzeichnis tun darf. Es gibt drei grundlegende Arten von Rechten sowie drei Kategorien von Benutzern.

#### Benutzerkategorien

Die Berechtigungen werden für drei verschiedene Benutzergruppen definiert:

1. **Benutzer (Owner)**: Der Eigentümer der Datei.
2. **Gruppe (Group)**: Die Benutzergruppe, der die Datei zugeordnet ist.
3. **Andere (Others)**: Alle anderen Benutzer, die nicht Eigentümer oder Gruppenmitglied sind.

#### Berechtigungstypen

Für jede dieser Gruppen können die folgenden Rechte vergeben werden:

* **Lesen (read, `r`)**\
  Erlaubt das Anzeigen des Dateiinhalts bzw. das Auflisten von Verzeichnissen.
* **Schreiben (write, `w`)**\
  Erlaubt das Ändern des Dateiinhalts bzw. das Erstellen, Löschen oder Umbenennen von Dateien innerhalb eines Verzeichnisses.
* **Ausführen (execute, `x`)**\
  Erlaubt das Ausführen von Programmen oder das Betreten eines Verzeichnisses.

#### Darstellung der Berechtigungen

Die Zugriffsrechte werden mit einem zehnstelligen String dargestellt, zum Beispiel:

```
-rwxr-xr--
```

Die Bedeutung der Positionen ist wie folgt:

| Position | Bedeutung                                     |
| -------- | --------------------------------------------- |
| 1        | Dateityp (`-` für Datei, `d` für Verzeichnis) |
| 2–4      | Rechte für den Benutzer                       |
| 5–7      | Rechte für die Gruppe                         |
| 8–10     | Rechte für andere Benutzer                    |

Beispiel: `-rw-r--r--`

* Normale Datei (`-`)
* Besitzer darf lesen und schreiben (`rw-`)
* Gruppe darf lesen (`r--`)
* Andere dürfen ebenfalls nur lesen (`r--`)

#### Oktale Darstellung

Berechtigungen lassen sich auch in oktaler Form ausdrücken. Jede Kombination von `r`, `w`, `x` ergibt eine Ziffer zwischen 0 und 7:

| Rechte | Oktalwert |
| ------ | --------- |
| `---`  | 0         |
| `--x`  | 1         |
| `-w-`  | 2         |
| `-wx`  | 3         |
| `r--`  | 4         |
| `r-x`  | 5         |
| `rw-`  | 6         |
| `rwx`  | 7         |

**Beispiele:**

* `644` = Besitzer: lesen + schreiben, Gruppe & andere: nur lesen (`rw-r--r--`)
* `755` = Besitzer: alle Rechte, Gruppe & andere: lesen + ausführen (`rwxr-xr-x`)
* `600` = Nur Besitzer hat Lese- und Schreibrechte (`rw-------`)

***

### Spezielle Berechtigungsbits (Special Bits) in Unix

Neben den klassischen Dateiberechtigungen **lesen (r)**, **schreiben (w)** und **ausführen (x)** existieren unter Unix drei sogenannte _Special Bits_: **SUID**, **SGID** und **Sticky Bit**. Diese erweitern die Funktionalität der Dateiberechtigungen, insbesondere in sicherheitskritischen oder gemeinsam genutzten Umgebungen.

#### SUID (Set User ID)

Das **SUID-Bit** (Set User ID) ermöglicht es einem Benutzer, ein ausführbares Programm mit den Rechten des Datei-Eigentümers auszuführen, anstatt mit den eigenen Benutzerrechten. Dies wird insbesondere bei Programmen benötigt, die auf Systemressourcen zugreifen, für die der ausführende Benutzer keine direkten Rechte besitzt.

**Beispiel:** Das Kommando `passwd` erlaubt es normalen Benutzern, ihre Passwörter zu ändern. Da Passwortdaten in einer geschützten Systemdatei gespeichert sind, muss `passwd` mit Root-Rechten ausgeführt werden. Dies wird durch das gesetzte SUID-Bit realisiert.

**Darstellung in den Dateiberechtigungen:**\
Statt `rwx` erscheint im Benutzerbereich `rws`, z. B. `-rwsr-xr-x`.

#### SGID (Set Group ID)

Das **SGID-Bit** (Set Group ID) funktioniert analog zum SUID-Bit, bezieht sich jedoch auf die Gruppenzugehörigkeit. Wird eine Datei mit gesetztem SGID-Bit ausgeführt, erfolgt die Ausführung mit den Rechten der Gruppe, der die Datei gehört.

**Beispiel:** Das Kommando `wall`, mit dem Nachrichten an alle eingeloggten Benutzer gesendet werden können, ist typischerweise mit dem SGID-Bit versehen, da es auf Gruppengeräte wie `tty` zugreift.

**Darstellung in den Dateiberechtigungen:**\
Statt `rwx` erscheint im Gruppenbereich `rws`, z. B. `-rwxr-sr-x`.

#### Sticky Bit

Das **Sticky Bit** kommt typischerweise bei gemeinsam genutzten Verzeichnissen zum Einsatz und dient der Zugriffskontrolle auf enthaltene Dateien. Ist das Sticky Bit gesetzt, können Dateien innerhalb des Verzeichnisses nur vom jeweiligen Eigentümer oder vom Root-Benutzer gelöscht oder umbenannt werden – unabhängig von den sonstigen Schreibrechten auf das Verzeichnis.

**Beispiel:** Das Verzeichnis `/tmp` hat das Sticky Bit gesetzt. Dadurch können alle Benutzer dort temporäre Dateien anlegen, aber keine fremden Dateien löschen.

**Darstellung in den Dateiberechtigungen:**\
Am Ende erscheint ein `t`, z. B. `drwxrwxrwt`.

#### Oktale Darstellung

Die Spezialbits lassen sich auch in der oktalen Notation ausdrücken, indem eine vierte Ziffer vor die bekannten dreistelligen Berechtigungswerte gesetzt wird:

* **4** = SUID
* **2** = SGID
* **1** = Sticky Bit

**Beispiele:**

* `1777` = Sticky Bit gesetzt, alle Benutzer haben volle Rechte (z. B. `/tmp`)
* `4644` = SUID gesetzt, Besitzer hat `rw-`, andere nur Leserechte
* `2444` = SGID gesetzt, Datei ist lesbar, aber nicht schreib- oder ausführbar
* `6777` = SUID und SGID gesetzt bei vollen Rechten für alle
* `7444` = SUID, SGID und Sticky Bit gesetzt, nur Leserechte für alle



***

### Dateiattribute unter Linux (`lsattr` / `chattr`)

Neben den klassischen Dateiberechtigungen (rwx) und Special Bits (SUID, SGID, Sticky Bit) kennt Linux weitere **Dateiattribute**, die sich mit den Befehlen `lsattr` (anzeigen) und `chattr` (ändern) verwalten lassen. Diese Attribute wirken auf Dateiebene und bieten zusätzliche Kontrollmechanismen für Schutz, Verhalten oder Sicherheit einer Datei.

siehe [lsattr-chattr.md](bash-befehle/lsattr-chattr.md "mention")

#### Anzeige von Dateiattributen: `lsattr`

Der Befehl `lsattr` zeigt gesetzte Attribute einer Datei oder eines Verzeichnisses an. Beispiel:

```bash
lsattr datei.txt
----i--------e-- datei.txt
```

Jedes Zeichen in der Attributanzeige steht für ein bestimmtes Attribut. Ein Bindestrich bedeutet, dass das entsprechende Attribut nicht gesetzt ist.

#### Setzen von Dateiattributen: `chattr`

Mit `chattr` lassen sich Dateiattribute ändern. Beispiel:

```bash
chattr +i datei.txt
```

Dies setzt das **immutable**-Attribut, welches die Datei schreibschützt, selbst für root.

#### Wichtige Dateiattribute im Überblick

| Attribut | Symbol                     | Beschreibung                                                                                               |
| -------- | -------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `a`      | Append only                | Datei kann nur angehängt, aber nicht überschrieben oder gelöscht werden. Nützlich für Log-Dateien.         |
| `c`      | Compressed                 | Datei wird transparent komprimiert (nicht von allen Dateisystemen unterstützt).                            |
| `d`      | No dump                    | Datei wird von `dump`-Backups ignoriert.                                                                   |
| `e`      | Extents                    | Verwendet Extents für Speicherverwaltung (automatisch gesetzt, nicht direkt veränderbar).                  |
| `i`      | Immutable                  | Datei kann nicht verändert, umbenannt oder gelöscht werden – selbst von root nicht, ohne vorher `-i`.      |
| `j`      | Data journaling            | Daten werden vor dem Schreiben ins Journal geschrieben (nur bei ext3 mit aktiviertem Journaling relevant). |
| `s`      | Secure deletion            | Inhalt wird beim Löschen überschrieben (abhängig vom Dateisystem).                                         |
| `t`      | No tail-merging            | Verhindert Tail-Merging bei ReiserFS.                                                                      |
| `u`      | Undeletable                | Ermöglicht Wiederherstellung gelöschter Dateien (nicht allgemein unterstützt).                             |
| `A`      | No atime updates           | Zugriffszeit (atime) wird nicht aktualisiert, was IO sparen kann.                                          |
| `S`      | Synchronous updates        | Änderungen an Dateiinhalt und Metadaten werden sofort auf Platte geschrieben.                              |
| `T`      | Top of directory hierarchy | Markiert ein Verzeichnis als "Top-Level" für Ordnungszwecke (z. B. bei Orlov-Allocator).                   |
| `Z`      | Compressed dirty file      | Nur relevant für bestimmte komprimierende Dateisysteme.                                                    |
| `E`      | Encrypted                  | Datei ist verschlüsselt (z. B. bei ext4 mit Verschlüsselung).                                              |

> Hinweis: Nicht alle Attribute sind auf jedem Dateisystem verfügbar oder veränderbar. Am weitesten verbreitet ist `i` (immutable), gefolgt von `a` (append only) und `A` (no atime).

#### Nutzung von `chattr`

* **Setzen eines Attributs:** `chattr +i datei.txt`
* **Entfernen eines Attributs:** `chattr -i datei.txt`
* **Kombination mehrerer Attribute:** `chattr +ai datei.txt`

`chattr` benötigt in der Regel Root-Rechte.
