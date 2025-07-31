# /tmp

#### `/tmp` in Linux

Das Verzeichnis `/tmp` ist ein **temporäres Verzeichnis** in Linux, das von Systemen und Anwendungen verwendet wird, um temporäre Dateien zu speichern. Diese Dateien werden häufig während des Betriebs eines Systems oder einer Anwendung benötigt, jedoch sind sie nicht dauerhaft und können gelöscht werden, sobald sie nicht mehr gebraucht werden.

#### Inhalt von `/tmp`:

* **Temporäre Dateien**: Das Verzeichnis wird von Programmen und dem System verwendet, um Daten zwischenzuspeichern, die nur vorübergehend benötigt werden. Beispielsweise speichert ein Texteditor während der Bearbeitung eine temporäre Datei im `/tmp`-Verzeichnis.
* **Daten von Programmen und Prozessen**: Viele Anwendungen schreiben temporäre Daten in `/tmp`, zum Beispiel für Caching-Zwecke oder um Zwischenstände von Berechnungen zu speichern.
* **Systemdienste und Installationsvorgänge**: Auch das System selbst oder bei der Installation von Software werden oft temporäre Dateien im `/tmp`-Verzeichnis abgelegt.

#### Verwendungszweck von `/tmp`:

* **Temporäre Datenspeicherung**: Programme und Benutzer speichern temporäre Daten, die keine langfristige Speicherung erfordern. Diese Daten werden nach Beendigung des Programms oder nach einer bestimmten Zeit gelöscht.
* **Cache und Sitzungsdaten**: Anwendungen können Caches oder Sitzungsdaten in `/tmp` ablegen, die beim Neustart oder bei der Beendigung des Programms entfernt werden.
* **Interprozesskommunikation**: Manche Programme nutzen `/tmp` als Ort für **Interprozesskommunikation** (IPC), um Dateien zwischen Prozessen auszutauschen.

#### Eigenschaften von `/tmp`:

* **Datenverlust nach Neustart**: In vielen Systemen wird das Verzeichnis `/tmp` nach einem Neustart des Systems geleert, um sicherzustellen, dass keine unnötigen temporären Dateien zurückgelassen werden.
* **Dateisicherheitsprobleme**: Da `/tmp` häufig für temporäre, nicht vertrauliche Daten verwendet wird, ist es ein häufiger Angriffsvektor. Zum Beispiel können Angreifer, die Schreibzugriff auf dieses Verzeichnis haben, möglicherweise **unsichere temporäre Dateien** platzieren oder manipulieren.
* **Verschiedene Berechtigungen**: Die Berechtigungen von `/tmp` sind in der Regel für alle Benutzer lesbar und schreibbar, was es einfach macht, dass Programme und Skripte temporäre Dateien speichern können. Dies kann jedoch ein Sicherheitsrisiko darstellen, da schadhafter Code in temporären Dateien platziert werden könnte.

#### Wichtige Aspekte:

* **Automatische Bereinigung**: In vielen modernen Linux-Distributionen wird `/tmp` nach einem Neustart oder durch systemweite Bereinigungsskripte automatisch geleert, um den Platz zu optimieren.
* **Unabhängigkeit von Benutzern**: Alle Benutzer des Systems haben in der Regel Zugriff auf `/tmp`, um temporäre Dateien zu speichern, aber sie haben keine privilegierten Berechtigungen, um auf Dateien anderer Benutzer zuzugreifen.
* **Sicherheitsvorkehrungen**: Es gibt gewisse Sicherheitsmechanismen, um die Sicherheit von `/tmp` zu gewährleisten, zum Beispiel durch das Setzen von **noexec**, **nosuid** oder **nodev**-Optionen für das Dateisystem, um die Ausführung von bösartigen Dateien zu verhindern.
