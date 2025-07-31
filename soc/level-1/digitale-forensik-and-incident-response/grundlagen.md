# Grundlagen

### Artefakte (Artifacts)

Artefakte werden definiert als Beweisstücke, die auf eine an einem System durchgeführte Aktivität hinweisen. Im Rahmen von DFIR werden Artefakte gesammelt, um eine Hypothese oder Behauptung über Angreiferaktivitäten zu untermauern.

Es sei beispielsweise die Behauptung aufgestellt, dass der Angreifer Windows-Registrierungsschlüssel verwendet hat, um die Persistenz eines Systems aufrechtzuerhalten. In diesem Fall können die entsprechenden Registrierungsschlüssel zur Stützung der Behauptung herangezogen werden. In diesem Fall wird der besagte Registrierungsschlüssel als Artefakt betrachtet.

Das Sammeln von Artefakten stellt einen essenziellen Bestandteil des DFIR-Prozesses dar. Artefakte können aus dem Dateisystem, dem Speicher oder der Netzwerkaktivität des Endpunkts oder Servers gesammelt werden.

***

### Beweissicherung (Evidence Preservation)

Im Rahmen der Durchführung von DFIR ist die Wahrung der Integrität der gesammelten Beweise von essentieller Bedeutung. Aus diesem Grund existieren in der Branche bestimmte bewährte Praktiken.

Es ist zu berücksichtigen, dass jede forensische Analyse die Beweise kontaminiert. Aus diesem Grund werden die Beweise zunächst gesammelt und schreibgeschützt. Im Anschluss wird eine Kopie des schreibgeschützten Beweismaterials für die Analyse verwendet.

Dieses Verfahren gewährleistet, dass die Originalbeweise während des Analyseprozesses nicht verunreinigt werden und ihre Integrität erhalten bleibt. Im Falle einer Beschädigung der zu untersuchenden Kopie besteht jederzeit die Möglichkeit, eine neue Kopie des aufbewahrten Beweismittels zu erstellen.

***

### Gewahrsamskette (Chain of custody)

Ein weiterer essenzieller Aspekt, um die Integrität von Beweismitteln zu gewährleisten, ist die Verwahrkette. Im Rahmen der Beweisammlung ist eine adäquate Verwahrung der gesammelten Beweismittel zu gewährleisten, um deren Integrität und Authentizität zu bewahren.

Personen, die nicht mit den Ermittlungen in Verbindung stehen, ist es untersagt, in den Besitz der Beweismittel zu gelangen, da dies eine Beeinträchtigung der Beweiskette zur Folge hätte. Eine verunreinigte Beweiskette wirft Fragen hinsichtlich der Integrität der Daten auf und schwächt den konstruierten Fall, indem unbekannte Variablen hinzugefügt werden, die nicht gelöst werden können.

Es sei angenommen, dass ein Festplatten-Image während des Transports von der Person, die es erstellt hat, zu der Person, die die Analyse durchführt, in den Besitz einer Person gelangt, die nicht über die erforderlichen Kenntnisse für den Umgang mit solchen Beweismitteln verfügt. In diesem Fall ist es nicht möglich, die Sicherheit zu gewährleisten, dass die Person das Beweismittel adäquat behandelt und es durch ihre Tätigkeit nicht kontaminiert hat.

***

### Reihenfolge der Flüchtigkeit (Order of volatility)

Digitale Beweismittel sind in vielen Fällen flüchtig, was bedeutet, dass sie bei nicht rechtzeitiger Erfassung verloren gehen können. Ein Beispiel ist der Verlust von Daten im RAM-Speicher eines Computers, wenn das Gerät ausgeschaltet wird. Der RAM-Speicher speichert Daten nur, solange das Gerät eingeschaltet ist.

Einige Quellen sind im Vergleich zu anderen als weniger flüchtig zu bewerten. Eine Festplatte fungiert als ein Speichermedium, welches eine hohe Speicherkapazität aufweist und Daten auch im Falle eines Stromausfalls konserviert. Daher ist eine Festplatte weniger flüchtig als ein RAM.

Bei der Durchführung von DFIR ist es von entscheidender Bedeutung, die Reihenfolge der Flüchtigkeit der verschiedenen Beweisquellen zu verstehen, um sie entsprechend zu erfassen und zu sichern. In dem oben angeführten Beispiel ist es essenziell, den Arbeitsspeicher vor der Festplatte zu sichern, um einen Verlust von Daten im Arbeitsspeicher zu verhindern.

***

### Erstellung einer Zeitleiste (Timeline creation)

Nach der Sammlung und Erhaltung der Artefakte ist es essenziell, diese verständlich darzustellen, um die darin enthaltenen Informationen vollständig nutzen zu können. Für eine effiziente und präzise Analyse ist die Erstellung einer Zeitleiste der Ereignisse unerlässlich.

Die Darstellung sämtlicher Aktivitäten erfolgt in chronologischer Reihenfolge. Diese Tätigkeit wird als Erstellung einer Zeitleiste bezeichnet. Die Erstellung eines Zeitstrahls ist ein wesentlicher Aspekt der Untersuchung, da sie der Analyse eine klare Perspektive bietet und die Integration von Informationen aus diversen Quellen ermöglicht.

Dies ist von entscheidender Bedeutung für die Entwicklung einer umfassenden Dokumentation über den Ablauf der Ereignisse.

***

### Prozessschritte für den Incident Response

Je nach Organisation werden die Schritte unterschiedliche definiert.\
Unter SANS werden die Schritte noch einmal ausführlich erklärt.

{% tabs %}
{% tab title="NIST (nist.gov)" %}
1. Vorbereitung
2. Erkennung und Analyse
3. Eindämmung, Ausrottung und Wiederherstellung
4. Aktivitäten nach einem Vorfall
{% endtab %}

{% tab title="SANS (sans.org)" %}
Die folgenden Schritte werden auch oft unter dem Acronym PICERL zusammengefasst/ genannt.

1. **P**reparation (Vorbereitung)\
   Bevor es zu einem Zwischenfall kommt, müssen Vorbereitungen getroffen werden, damit alle für den Fall eines Zwischenfalls gerüstet sind. Zur Vorbereitung gehört, dass die erforderlichen Mitarbeiter, Verfahren und Technologien vorhanden sind, um Vorfälle zu verhindern und darauf zu reagieren.
2. **I**dentification (Kennzeichnung / Identifizierung)\
   In der Identifizierungsphase wird ein Vorfall anhand einiger Indikatoren erkannt. Diese Indikatoren werden dann auf False Positives hin analysiert, dokumentiert und an die zuständigen Akteure weitergeleitet.
3. **C**ontainment (Eindämmung)\
   In dieser Phase wird der Vorfall eingedämmt, und es werden Anstrengungen unternommen, um seine Auswirkungen zu begrenzen. Auf der Grundlage der forensischen Analyse des Vorfalls, die Teil dieser Phase ist, können kurz- und langfristige Maßnahmen zur Eindämmung der Bedrohung ergriffen werden.
4. **E**radication (Ausrottung)\
   Anschließend wird die Bedrohung aus dem Netz entfernt. Es muss sichergestellt werden, dass eine ordnungsgemäße forensische Analyse durchgeführt und die Bedrohung vor der Ausrottung wirksam eingedämmt wird. Wenn beispielsweise der Eintrittspunkt des Bedrohungsakteurs in das Netzwerk nicht entfernt wird, kann die Bedrohung nicht wirksam beseitigt werden, und der Akteur kann wieder Fuß fassen.
5. **R**ecovery (Wiederherstellung)\
   Sobald die Bedrohung aus dem Netz entfernt ist, werden die unterbrochenen Dienste wieder so hergestellt, wie sie vor dem Vorfall waren.
6. **L**essons learned (Aufzeichnung / Diskussion des Gelernten)\
   Abschließend wird der Vorfall überprüft, dokumentiert und auf der Grundlage der Erkenntnisse aus dem Vorfall werden Maßnahmen ergriffen, um sicherzustellen, dass das Team auf den nächsten Vorfall besser vorbereitet ist.
{% endtab %}
{% endtabs %}



### Forensik Cheatsheets

Linux: [linux-forensik.md](../../../cheat-sheets/linux-forensik.md "mention")

Windows: [powershell.md](../../../cheat-sheets/powershell.md "mention")

Handbücher für Incident Handler:&#x20;

* [https://www.sans.org/white-papers/33901/](https://www.sans.org/white-papers/33901/)&#x20;
* [https://sansorg.egnyte.com/dl/SzUc95nE0x](https://sansorg.egnyte.com/dl/SzUc95nE0x)
* [https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

