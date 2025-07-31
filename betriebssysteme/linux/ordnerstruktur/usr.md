# /usr

#### `/usr` in Linux

Das Verzeichnis `/usr` ist eines der wichtigsten Verzeichnisse im Linux-Dateisystem und enthält Systemdateien, die für den Betrieb und die Ausführung von Anwendungen auf dem System erforderlich sind. Es handelt sich um das Hauptverzeichnis für **Anwendungsprogramme**, **Bibliotheken** und **Systemressourcen**, die nicht nur für das Betriebssystem selbst, sondern auch für Software und Dienstprogramme von Drittanbietern benötigt werden.

#### Inhalt von `/usr`:

Das Verzeichnis `/usr` wird häufig in mehrere Unterverzeichnisse unterteilt, die spezifische Arten von Dateien enthalten. Hier sind die wichtigsten Unterverzeichnisse und deren Inhalte:

* **`/usr/bin`**: Enthält ausführbare Programme und Dienstprogramme, die für den normalen Betrieb des Systems erforderlich sind. Dazu gehören sowohl Benutzeranwendungen als auch grundlegende Systemtools. Dies ist das größte Verzeichnis in `/usr`.
* **`/usr/sbin`**: Beinhaltet Systemverwaltungs- und Verwaltungswerkzeuge, die in der Regel von Administratoren verwendet werden. Die Programme hier sind meist nicht für den normalen Benutzer zugänglich oder benötigt.
* **`/usr/lib`**: Enthält gemeinsam genutzte Bibliotheken, die von Programmen in `/usr/bin` oder `/usr/sbin` benötigt werden. Dies umfasst sowohl 32-Bit- als auch 64-Bit-Bibliotheken, die von verschiedenen Anwendungen verwendet werden.
* **`/usr/local`**: Wird für lokal installierte Software verwendet. Hier finden sich Programme und Daten, die nicht Teil der Standarddistribution sind, sondern lokal auf dem System hinzugefügt wurden. Es umfasst auch ein eigenes Set an Verzeichnissen wie `/usr/local/bin`, `/usr/local/lib`, etc.
* **`/usr/share`**: Beinhaltet gemeinsam genutzte Daten wie Dokumentation, Grafiken, Konfigurationsdateien und Übersetzungen, die von Programmen benötigt werden, aber keine ausführbaren Dateien sind.

#### Verwendungszweck von `/usr`:

* **Betriebssystemdateien und Anwendungen**: `/usr` enthält viele der für das System notwendigen Dateien und Anwendungen. Dabei handelt es sich um Software, die nicht für den Systemstart oder die Reparatur benötigt wird, aber wichtig für den normalen Betrieb ist.
* **Gemeinsam genutzte Systemressourcen**: Bibliotheken, die von mehreren Programmen gleichzeitig verwendet werden, werden in `/usr/lib` gespeichert, wodurch die Wiederverwendung und zentrale Verwaltung von Ressourcen ermöglicht wird.
* **Software von Drittanbietern**: Viele Anwendungen von Drittanbietern werden in `/usr` installiert, insbesondere in den Verzeichnissen `/usr/bin` und `/usr/share`, wo Programme ihre ausführbaren Dateien und Konfigurationsdaten ablegen.

#### Eigenschaften von `/usr`:

* **Schreibgeschützt für Benutzer**: In der Regel ist das Verzeichnis `/usr` schreibgeschützt für normale Benutzer, da es systemrelevante Dateien enthält. Nur Administratoren (Root-Benutzer) haben die Berechtigung, Änderungen in diesem Verzeichnis vorzunehmen.
* **Standardverzeichnis für Softwareinstallation**: Viele Linux-Distributionen installieren Programme standardmäßig in das Verzeichnis `/usr`, insbesondere im Unterverzeichnis `/usr/bin`, da dieses Verzeichnis in der Standard-PATH-Umgebungsvariablen der meisten Systeme enthalten ist.
* **Teile des Basissystems**: Das Verzeichnis `/usr` ist ein essenzieller Bestandteil des Basissystems, wobei einige Distributionen `/usr` auf separaten Partitionen oder Dateisystemen installieren, um die Trennung von System- und Anwendungsdateien zu erleichtern.

#### Beispiel:

Um alle Dateien in `/usr/bin` aufzulisten:

```bash
ls /usr/bin
```

#### Wichtige Aspekte:

* **Trennung von `/usr` und `/bin`**: Im Gegensatz zu `/bin`, das essentielle Binärdateien für den Betrieb des Systems enthält, enthält `/usr/bin` Software, die für die normale Nutzung des Systems erforderlich ist, jedoch nicht kritisch für das Hochfahren oder die Reparatur des Systems.
* **Keine direkte Systemwiederherstellung**: Während Verzeichnisse wie `/bin` und `/sbin` für den Betrieb des Systems unerlässlich sind, enthält `/usr` Programme und Bibliotheken, die das System in einem funktionalen Zustand halten, aber nicht für eine Notfallwiederherstellung erforderlich sind.
* **Verwaltung von Software**: Viele Distributionen bieten Tools zur Verwaltung von Softwarepaketen, die Programmbinärdateien und -bibliotheken in den entsprechenden Unterverzeichnissen von `/usr` installieren.
