
### Trees in Active Directory

Stellen Sie sich vor, Ihr Unternehmen expandiert in ein neues Land mit eigenen Gesetzen und Vorschriften. Diese Änderungen erfordern eine Anpassung der GPOs, um rechtskonform zu bleiben. Zusätzlich haben Sie jetzt IT-Teams in beiden Ländern, die jeweils nur die Ressourcen ihres Landes verwalten sollen, ohne dass das andere Team eingreift. Zwar könnte eine komplexe OU-Struktur mit Delegationen helfen, dies zu steuern, aber eine große AD-Struktur ist schwer zu verwalten und anfällig für Fehler.

Zum Glück unterstützt Active Directory die Integration mehrerer Domänen, sodass das Netzwerk in eigenständig verwaltbare Einheiten unterteilt werden kann. Wenn zwei Domänen denselben Namespace teilen (z. B. **thm.local**), können diese Domänen zu einem **Tree** verbunden werden.

Wenn die Domäne **thm.local** in zwei Unterdomänen für die Standorte in UK und USA aufgeteilt wird, können Sie einen Tree mit einer Root-Domäne **thm.local** und zwei Subdomains **uk.thm.local** und **us.thm.local** erstellen. Jede dieser Subdomänen hat ihr eigenes AD, ihre eigenen Computer und Benutzer, die unabhängig voneinander verwaltet werden.


![[Pasted image 20241103211355.png]]

Durch die Aufteilung des Netzwerks in mehrere Domänen erhalten Sie eine bessere Kontrolle darüber, wer auf welche Ressourcen zugreifen kann. Das IT-Team in Großbritannien verwaltet beispielsweise ausschließlich UK-Ressourcen über einen eigenen Domain Controller (DC). Ein Benutzer aus der UK-Domäne kann somit keine Benutzer in der US-Domäne verwalten. Die **Domain-Administratoren** jeder Niederlassung haben vollständige Kontrolle über ihre jeweiligen DCs, aber nicht über die DCs anderer Standorte. Auf diese Weise können Richtlinien und Zugriffsrechte für jede Domäne im Tree unabhängig konfiguriert werden.

### Sicherheitsgruppen für Trees und Forests

Mit der Einführung von Trees und Forests kommt eine neue Sicherheitsgruppe ins Spiel: **Enterprise Admins**. Mitglieder dieser Gruppe erhalten administrative Rechte über alle Domänen eines Unternehmens. Jede Domäne behält weiterhin ihre **Domain Admins**, die Administratorrechte nur innerhalb ihrer jeweiligen Domäne besitzen, während die **Enterprise Admins** zentralen Zugriff auf alle Domänen im Unternehmen haben.

### Forests: Integration mehrerer Trees

Domänen können auch in unterschiedlichen **Namespaces** konfiguriert werden. Angenommen, Ihr Unternehmen wächst weiter und übernimmt ein anderes Unternehmen, z. B. **MHT Inc.**. Nach der Fusion existieren vermutlich separate Domänen-Trees für jede Firma, die jeweils von ihren eigenen IT-Teams verwaltet werden. Die Verbindung mehrerer Trees mit verschiedenen Namespaces innerhalb desselben Netzwerks wird als **Forest** bezeichnet. Ein Forest ermöglicht es, mehrere Trees unter einer gemeinsamen Verwaltung zu vereinen, auch wenn sie unterschiedliche Namespaces haben.

![[Pasted image 20241103211405.png]]

### Vertrauensstellungen in Active Directory (Trust Relationship)

Das Organisieren mehrerer Domänen in Trees und Forests ermöglicht eine gut strukturierte Verwaltung und Ressourcenzuteilung. Es kann jedoch vorkommen, dass ein Benutzer aus der Domäne **THM UK** auf eine gemeinsame Datei auf einem Server der **MHT ASIA**-Domäne zugreifen muss. Damit dies möglich ist, werden die in Trees und Forests angeordneten Domänen durch **Vertrauensstellungen** miteinander verbunden.

#### Vertrauensstellungen einfach erklärt

Eine Vertrauensstellung zwischen Domänen ermöglicht es, einen Benutzer aus der Domäne **THM UK** zu autorisieren, auf Ressourcen in der Domäne **MHT EU** zuzugreifen.

Die einfachste Form einer Vertrauensstellung ist die **einseitige Vertrauensstellung**. In einer einseitigen Vertrauensstellung bedeutet es, dass, wenn Domäne **AAA** Domäne **BBB** vertraut, Benutzer von Domäne **BBB** autorisiert werden können, auf Ressourcen in Domäne **AAA** zuzugreifen. Das heißt, während die Domäne **AAA** den Benutzern von **BBB** den Zugriff ermöglicht, gilt das nicht umgekehrt; Benutzer aus Domäne **AAA** können nicht auf Ressourcen in Domäne **BBB** zugreifen, es sei denn, es besteht eine separate Vertrauensstellung.

![[Pasted image 20241103211416.png]]

Die Richtung einer einseitigen Vertrauensstellung steht im Gegensatz zur Richtung des Zugriffs. Das bedeutet, dass in einer einseitigen Vertrauensstellung nur Benutzer von der vertrauenswürdigen Domäne auf Ressourcen der vertrauenden Domäne zugreifen können, nicht umgekehrt.

#### Zweiwegverhältnisse

**Zweiwegverhältnisse** können ebenfalls eingerichtet werden, um beiden Domänen die gegenseitige Autorisierung von Benutzern zu ermöglichen. Standardmäßig führt das Zusammenführen mehrerer Domänen unter einem Tree oder Forest zur Bildung einer zweiwegigen Vertrauensstellung. In diesem Fall können Benutzer von jeder Domäne auf die Ressourcen der jeweils anderen Domäne zugreifen, was die Zusammenarbeit und den Zugriff auf gemeinsame Ressourcen erheblich erleichtert.

### Autorisierungssteuerung

Es ist wichtig zu beachten, dass eine Vertrauensstellung zwischen Domänen nicht automatisch den Zugriff auf alle Ressourcen der anderen Domäne gewährt. Nachdem eine Vertrauensstellung eingerichtet wurde, haben Sie die Möglichkeit, Benutzer über verschiedene Domänen hinweg zu autorisieren. Es liegt jedoch an Ihnen, zu entscheiden, welche spezifischen Berechtigungen tatsächlich gewährt werden und welche Ressourcen geschützt bleiben.