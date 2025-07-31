# lost+found

`/lost+found` ist ein spezielles Verzeichnis, das in Linux- und Unix-Dateisystemen zu finden ist, insbesondere auf Dateisystemen, die das **ext3** oder **ext4** Format verwenden. Es ist ein Systemverzeichnis, das während der Dateisystem-Überprüfung (`fsck`) erstellt wird, um verlorene oder beschädigte Dateien und Verzeichnisse wiederherzustellen, die durch Fehler im Dateisystem oder Abstürze des Systems verloren gegangen sind.

#### Inhalt von `/lost+found`:

* **Wiederhergestellte Dateien**: Wenn das Dateisystem beim Start des Systems oder während der Überprüfung (`fsck`) beschädigte oder nicht zugeordnete Dateien findet, werden diese in das Verzeichnis `/lost+found` verschoben. Es enthält in der Regel Dateien, die aufgrund von Dateisystemfehlern verloren gegangen oder unzugeordnet wurden. Diese Dateien sind in der Regel nicht benannt und erhalten zufällige Dateinamen, die von der Wiederherstellungssoftware oder `fsck` generiert wurden.
  * **Dateien ohne Verweis**: Manchmal kann es passieren, dass Dateien und Ordner ihre Verknüpfung zu den Verzeichnissen verlieren, was dazu führt, dass sie unzugänglich werden. Diese Dateien werden dann von `fsck` in das Verzeichnis `/lost+found` verschoben, um sie wiederherzustellen, sodass sie später manuell überprüft und möglicherweise zurückgeführt werden können.
  * **Keine strukturierte Organisation**: Das Verzeichnis `/lost+found` kann eine Vielzahl von Dateien enthalten, die unterschiedliche Formate und Namen haben. Diese Dateien sind nicht immer leicht zu identifizieren, und es kann vorkommen, dass sie manuell untersucht werden müssen, um festzustellen, ob sie noch nützlich sind.

#### Verwendungszweck von `/lost+found`:

* **Wiederherstellung von verlorenen Daten**: Das Verzeichnis dient der Wiederherstellung und Rettung von Daten, die durch Probleme im Dateisystem beschädigt oder verloren gegangen sind. Wenn eine Datei während eines Systemabsturzes oder durch einen Fehler im Dateisystem unzugänglich wird, könnte sie nach der Reparatur wieder in `/lost+found` auftauchen.
* **Dateisystemüberprüfung**: Bei der Überprüfung des Dateisystems (normalerweise beim Booten oder während der Wartung) führt das Programm `fsck` eine Reihe von Tests durch, um festzustellen, ob das Dateisystem Fehler aufweist. Wenn Fehler gefunden werden, kann `fsck` dazu führen, dass die fehlerhaften Dateien in `/lost+found` verschoben werden, um sie später zu überprüfen und wiederherzustellen.
* **Datenrettung bei Fehlern**: `/lost+found` ist also eine Art von "Notfalllager", das als Sicherheitsnetz fungiert, um sicherzustellen, dass verlorene oder beschädigte Daten nicht einfach verloren gehen, sondern zumindest zur späteren Untersuchung aufbewahrt werden.

#### Wichtige Aspekte:

* **Erforderlich für `fsck`**: Das Verzeichnis `/lost+found` wird von `fsck` benötigt, um verlorene Dateien während der Dateisystemprüfung zu speichern. Ohne dieses Verzeichnis könnten nicht alle beschädigten oder verlorenen Dateien von `fsck` wiederhergestellt werden.
* **Zugriffsrechte**: Das Verzeichnis ist normalerweise nur für den Benutzer `root` zugänglich, da es sich um systemkritische Daten handelt. Normale Benutzer sollten nicht in der Lage sein, auf dieses Verzeichnis zuzugreifen, um zu verhindern, dass verlorene Dateien unbeabsichtigt manipuliert oder gelöscht werden.
* **Verzeichnisspezifisch für Dateisysteme**: Das Verzeichnis `/lost+found` existiert nur auf Dateisystemen, die diese Funktion unterstützen, wie etwa ext3 und ext4. Andere Dateisysteme wie `NTFS` oder `FAT` haben keine Entsprechung für das Verzeichnis `/lost+found`.
* **Nicht für tägliche Nutzung**: Normalerweise wird das Verzeichnis `/lost+found` selten genutzt und ist für den normalen Betrieb des Systems nicht relevant, es sei denn, es gibt eine Beschädigung des Dateisystems, bei der Daten wiederhergestellt werden müssen.
