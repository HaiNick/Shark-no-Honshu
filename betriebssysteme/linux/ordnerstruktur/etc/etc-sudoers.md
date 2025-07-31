# /etc/sudoers

Wichtige Konfigurationsdatei in Linux und Unix-ähnlichen Betriebssystemen, die Benutzerrechte steuert. Sie definiert, wer als Superuser Befehle ausführen darf und bietet so eine sichere Möglichkeit zur Verwaltung von Administratorrechten

{% hint style="danger" %}
Die Datei **/etc/sudoers** sollte immer mit dem Befehl `visudo` bearbeitet werden, da so eine Syntaxprüfung gewährleistet ist. Die letzte Zeile der Sudoers-Datei muss zudem immer leer sein! Bei der direkten Bearbeitung ohne Prüfung kann der kleinste Tippfehler dazu führen, dass man sich aus dem System aussperrt und nur über den [Recovery Modus](https://wiki.ubuntuusers.de/Recovery_Modus/) wieder Zugang erhält. Es ist dabei korrekt, die Änderung in **sudoers.tmp** zu speichern, denn `visudo` überprüft nun die Syntax und man hat die Möglichkeit, Fehler zu korrigieren.

Alternativ kann man vor der Änderung eine zweite Konsole mit Root-Rechten (`sudo -i`) öffnen. Stellt man dann fest, dass man sich aus dem System ausgesperrt hat, kann man über diese Konsole die Änderungen wieder rückgängig machen!
{% endhint %}

