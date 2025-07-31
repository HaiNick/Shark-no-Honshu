# Cyber Kill Chain

Der Begriff "Kill Chain" ist ein militärisches Konzept, das sich auf die Struktur eines Angriffs bezieht. Sie besteht aus der Identifizierung des Ziels, der Entscheidung und dem Befehl, das Ziel anzugreifen, und schließlich der Zerstörung des Ziels.



## Reconnaissance

Bei der "Reconnaissance" handelt es sich um die **Entdeckung** und **Sammlung** von **Informationen** über das **System** und das **Ziel**. Die Aufklärungsphase ist die **Planungsphase** für den Angreifer.

E-Mail-Harvesting bezeichnet das **Sammeln** von **E-Mail-Adressen** aus **öffentlichen**, **kostenpflichtigen** oder **kostenlosen** Diensten. Ein Angreifer kann diese Adressen für **Phishing**-Angriffe nutzen, bei denen sensible Daten wie Anmeldedaten und Kreditkartennummern gestohlen werden.

* OSINT
* theHarvester ( Emails, Domains, Subdomains, IPs, URLs ) [https://github.com/laramies/theHarvester](https://github.com/laramies/theHarvester)
* Hunter.io ( Email, Kontaktinformationen von definierter Domain ) [https://hunter.io/](https://hunter.io/)
* OSINT Framework ( Kollektion an OSINT-Tools ) [https://osintframework.com/](https://osintframework.com/)

## Weaponization

Der Angreifer würde es vorziehen, nicht direkt mit dem Opfer zu interagieren. Stattdessen erstellt er einen " Weaponizer" oder **Waffenträger**, der laut Lockheed Martin **Malware** **und** **Exploit** zu einer **auslieferbaren** **Nutzlast** kombiniert.

Die meisten Angreifer verwenden in der Regel automatisierte Tools zur Generierung der Malware oder beziehen die Malware über das DarkWeb.

Anspruchsvollere Akteure oder von Staaten gesponserte APT-Gruppen (Advanced Persistent Threat Groups) würden ihre eigene Malware schreiben, um das Malware-Muster einzigartig zu machen und die Erkennung auf dem Ziel zu umgehen.

* Ein infiziertes Microsoft Office-Dokument wird erstellt, das ein bösartiges Makro oder VBA-Skripte (Visual Basic for Applications) enthält. Wenn Sie mehr über Makros und VBA erfahren möchten, lesen Sie bitte den Artikel "Intro to Macros and VBA For Script Kiddies" von TrustedSec. \[ [https://www.trustedsec.com/blog/intro-to-macros-and-vba-for-script-kiddies/](https://www.trustedsec.com/blog/intro-to-macros-and-vba-for-script-kiddies/) ]
* Ein Angreifer kann eine bösartige Nutzlast oder einen sehr ausgeklügelten Wurm erstellen, ihn auf die USB-Laufwerke implantieren und sie dann öffentlich verbreiten. Ein Beispiel für einen Virus.
* Ein Angreifer würde Command and Control (C2)-Techniken wählen, um die Befehle auf dem Rechner des Opfers auszuführen oder weitere Nutzdaten zu übermitteln. Mehr über die C2-Techniken erfahren Sie auf MITRE ATT\&CK \[ [https://attack.mitre.org/tactics/TA0011/](https://attack.mitre.org/tactics/TA0011/) ].&#x20;
* Ein Angreifer würde ein Backdoor-Implantat wählen (die Möglichkeit, auf das Computersystem zuzugreifen, einschließlich der Umgehung der Sicherheitsmechanismen).

## Delivery

Der Angreifer entscheidet sich für die Methode zur Übertragung der Payload oder der Malware.

* Phishing Email

Nach der Erkundung und Bestimmung der Angriffsziele würde der böswillige Akteur eine bösartige E-Mail erstellen, die entweder auf eine bestimmte Person (Spearphishing-Angriff) oder auf mehrere Personen im Unternehmen abzielt. Die E-Mail würde eine Nutzlast oder Malware enthalten.

* Verteilung infizierter USB-Sticks

Verteilen infizierter USB-Laufwerke an öffentlichen Orten wie Cafés, Parkplätzen oder auf der Straße. Ein Angreifer könnte eine ausgeklügelte USB-Drop-Attacke durchführen, indem er ein Firmenlogo auf die USB-Laufwerke druckt und sie an das Unternehmen schickt, während er vorgibt, ein Kunde zu sein, der die USB-Geräte als Geschenk verschickt.

* Watering hole Attacke

Bei einem Watering-Hole-Angriff handelt es sich um einen gezielten Angriff, der auf eine bestimmte Personengruppe abzielt, indem er die von ihr normalerweise besuchte Website kompromittiert und sie dann auf eine bösartige Website nach Wahl des Angreifers umleitet. Der Angreifer sucht nach einer bekannten Sicherheitslücke auf der Website und versucht, diese auszunutzen. Der Angreifer ermutigt die Opfer, die Website zu besuchen, indem er "harmlose" E-Mails versendet, in denen er auf die bösartige URL hinweist, damit der Angriff effizienter funktioniert. Nach dem Besuch der Website lädt das Opfer ungewollt Malware oder eine bösartige Anwendung auf seinen Computer herunter. Diese Art von Angriff wird als Drive-by-Download bezeichnet. Ein Beispiel hierfür ist ein bösartiges Popup-Fenster, das zum Herunterladen einer gefälschten Browser-Erweiterung auffordert.

## Exploitation

Um Zugang zum System zu erhalten, muss ein Angreifer die Sicherheitslücke ausnutzen.

Nachdem er sich Zugang zum System verschafft hat, könnte der böswillige Akteur Software-, System- oder Server-basierte Schwachstellen ausnutzen, um seine Privilegien zu erweitern oder sich seitlich im Netzwerk zu bewegen. Laut CrowdStrike bezieht sich "laterale Bewegung" auf die Techniken, die ein böswilliger Akteur nach dem anfänglichen Zugriff auf den Computer des Opfers anwendet, um tiefer in ein Netzwerk einzudringen und sensible Daten zu erhalten.

Der Angreifer könnte in dieser Phase auch einen "Zero-Day-Exploit" anwenden. Laut FireEye "ist ein Zero-Day-Exploit oder eine Zero-Day-Schwachstelle ein unbekannter Exploit in freier Wildbahn, der eine Schwachstelle in Software oder Hardware aufdeckt und komplizierte Probleme verursachen kann, lange bevor jemand merkt, dass etwas nicht stimmt. Ein Zero-Day-Exploit lässt von Anfang an KEINE Möglichkeit zur Entdeckung."

Dies sind Beispiele dafür, wie ein Angreifer einen Angriff durchführt:

* Das Opfer löst den Exploit aus, indem es den E-Mail-Anhang öffnet oder auf einen bösartigen Link klickt.&#x20;
* Verwendung eines Zero-Day-Exploits.&#x20;
* Ausnutzung von Software-, Hardware- oder sogar menschlichen Schwachstellen.&#x20;
* Ein Angreifer löst den Exploit für serverbasierte Schwachstellen aus.

## Installation

Der Angreifer möchte eine konstante Backdoor erstellen um jederzeit Zugriff auf das System erhalten zu können. Das kann durch folgendes erreicht werden:

* Web Shell auf Webserver
* Backdoor auf dem System (z.b. mit Meterpreter)
* Windows Dienste erstellen oder modifizieren ( Technik T1543.003 [https://attack.mitre.org/techniques/T1543/003/](https://attack.mitre.org/techniques/T1543/003/) )&#x20;

Ein Angreifer kann die Windows-Dienste erstellen oder ändern, um die bösartigen Skripte oder Nutzlasten regelmäßig als Teil der Persistenz auszuführen. Ein Angreifer kann Tools wie sc.exe (sc.exe ermöglicht das Erstellen, Starten, Anhalten, Abfragen oder Löschen von Windows-Diensten) und Reg verwenden, um Dienstkonfigurationen zu ändern. Der Angreifer kann auch die bösartige Nutzlast maskieren, indem er einen Dienstnamen verwendet, von dem bekannt ist, dass er mit dem Betriebssystem oder legitimer Software in Verbindung steht.

[https://attack.mitre.org/techniques/T1036/](https://attack.mitre.org/techniques/T1036/)

* Hinzufügen des Eintrags zu den " run keys " für die bösartige Payload in der Registry oder im Startup-Ordner.

Auf diese Weise wird die Nutzlast jedes Mal ausgeführt, wenn sich der Benutzer am Computer anmeldet. Laut MITRE ATT\&CK gibt es einen Startordner für einzelne Benutzerkonten und einen systemweiten Startordner, der unabhängig vom angemeldeten Benutzerkonto überprüft wird.

## Command & Control

Nachdem die Malware auf dem Rechner des Opfers persistiert und ausgeführt wurde, öffnet der Angreifer den C2-Kanal (Command and Control) über die Malware, um das Opfer aus der Ferne zu kontrollieren und zu manipulieren. Dieser Begriff ist auch als C\&C oder C2 Beaconing bekannt und bezeichnet eine Art der bösartigen Kommunikation zwischen einem C\&C-Server und der Malware auf dem infizierten Host. Der infizierte Host kommuniziert ständig mit dem C2-Server; daher stammt auch der Begriff Beaconing.

Der kompromittierte Endpunkt kommuniziert mit einem externen Server, der von einem Angreifer eingerichtet wurde, um einen Befehls- und Kontrollkanal aufzubauen. Nach dem Aufbau der Verbindung hat der Angreifer die volle Kontrolle über den Rechner des Opfers. Bis vor kurzem war IRC (Internet Relay Chat) der traditionelle C2-Kanal, der von Angreifern genutzt wurde. Dies ist heute nicht mehr der Fall, da moderne Sicherheitslösungen bösartigen IRC-Verkehr leicht erkennen können.

Dabei für gewöhnlich benutzte C2 Channels:

* Die Protokolle HTTP auf Port 80 und HTTPS auf Port 443 - diese Art von Beaconing vermischt den bösartigen Verkehr mit dem legitimen Verkehr und kann dem Angreifer helfen, Firewalls zu umgehen.
* DNS (Domain Name Server). Der infizierte Rechner stellt ständig DNS-Anfragen an den DNS-Server, der einem Angreifer gehört. Diese Art der C2-Kommunikation wird auch als DNS-Tunneling bezeichnet.

## Actions on Objectives ( Exfiltration )

Nachdem der Angreifer sechs Phasen des Angriffs durchlaufen hat, kann er schließlich seine Ziele erreichen, d. h. er kann die ursprünglichen Ziele in Angriff nehmen. Mit direktem Tastaturzugriff kann der Angreifer Folgendes erreichen:

* Sammeln der Anmeldedaten von Benutzern.
* Privilegienerweiterung (Erlangung von erweiterten Zugriffsrechten wie Domänenadministrator-Zugriff von einer Workstation durch Ausnutzung der Fehlkonfiguration).
* Interne Erkundung (z. B. kann ein Angreifer mit interner Software interagieren, um deren Schwachstellen zu finden).
* Seitliche Bewegung durch die Umgebung des Unternehmens.
* Sammeln und Exfiltrieren sensibler Daten.
* Löschen der Backups und Schattenkopien. Schattenkopie ist eine Microsoft-Technologie, die Sicherungskopien, Schnappschüsse von Computerdateien oder Volumes erstellen kann.
* Überschreiben oder Beschädigen von Daten.
