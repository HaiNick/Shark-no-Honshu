# Datenschutz und branchenspezifische Sicherheitsstandards

#### General Data Protection Regulation (GDPR)

Die **Datenschutz-Grundverordnung (DSGVO)** ist eine Datenschutzverordnung der Europäischen Union, die am 25. Mai 2018 in Kraft trat. Sie hat das Ziel, personenbezogene Daten von EU-Bürgern zu schützen und regelt, wie Organisationen diese Daten erheben, verarbeiten und speichern dürfen.

**Definition personenbezogener Daten:**\
Darunter versteht man alle Informationen, die sich auf eine identifizierte oder identifizierbare natürliche Person beziehen – also auch Daten, die eine indirekte Identifikation ermöglichen.

**Kernanforderungen der DSGVO:**

* **Einwilligungspflicht:** Die Erhebung personenbezogener Daten darf nur nach vorheriger, ausdrücklicher Zustimmung der betroffenen Person erfolgen.
* **Datensparsamkeit:** Es dürfen nur solche Daten erhoben werden, die für den jeweiligen Zweck zwingend erforderlich sind.
* **Sicherheitsmaßnahmen:** Unternehmen sind verpflichtet, geeignete technische und organisatorische Maßnahmen zum Schutz personenbezogener Daten zu implementieren.

**Reichweite:**\
Die DSGVO gilt für alle Unternehmen, die personenbezogene Daten von EU-Bürgern verarbeiten – unabhängig davon, ob das Unternehmen selbst in der EU ansässig ist oder nicht.

**Sanktionsmechanismus:**\
Die DSGVO sieht strenge Bußgelder vor, gestaffelt nach Schweregrad:

* **Tier 1:** Schwere Verstöße, z. B. unerlaubte Datenerhebung oder Weitergabe ohne Zustimmung → Bußgelder bis zu **4 % des Jahresumsatzes** oder **20 Mio. €** (je nachdem, was höher ist).
* **Tier 2:** Geringfügigere Verstöße, etwa verspätete Datenschutz-Folgenabschätzungen oder unzureichende Sicherheitsrichtlinien → Bußgelder bis zu **2 % des Umsatzes** oder **10 Mio. €**.

**Vorteile der DSGVO:**

* Stärkt das Vertrauen der Kunden durch transparente Datenverarbeitung
* Schafft einheitliche Datenschutzstandards im europäischen Binnenmarkt
* Fördert die Sicherheit durch verbindliche technische Schutzmaßnahmen

***

#### Payment Card Industry Data Security Standard (PCI DSS)

**PCI DSS** ist ein international anerkannter Sicherheitsstandard zur Absicherung von Kreditkartendaten, entwickelt durch die **PCI Security Standards Council** (bestehend u. a. aus Visa, MasterCard, American Express). Er richtet sich insbesondere an Unternehmen, die Kartenzahlungen verarbeiten oder speichern – vor allem im Onlinehandel.

**Ziele von PCI DSS:**

* Verhinderung von Kartenmissbrauch und Betrug
* Sicherstellung der Integrität und Vertraulichkeit von Karteninhaberdaten
* Förderung sicherer Transaktionen im digitalen Zahlungsverkehr

**Schlüsselanforderungen:**

* **Zugriffskontrolle:** Nur autorisierte Personen dürfen Zugang zu Karteninhaberdaten erhalten.
* **Netzwerksicherheit:** Einsatz von Firewalls, verschlüsselten Übertragungen und Sicherheitsmechanismen für Webanwendungen (z. B. WAF – Web Application Firewall).
* **Monitoring:** Überwachung aller Zugriffe auf Karteninhaberdaten sowie Durchführung regelmäßiger Sicherheitsprüfungen und Schwachstellenanalysen.
* **Datenverschlüsselung:** Speicherung und Übertragung sensibler Daten müssen durch starke Verschlüsselungsverfahren geschützt sein.

**Bedeutung:**\
PCI DSS ist kein Gesetz, aber ein vertraglich bindender Standard für Unternehmen, die Kreditkarten akzeptieren. Ein Verstoß kann zum Verlust der Möglichkeit führen, Kartenzahlungen anzunehmen, sowie zu hohen Vertragsstrafen.

Link: [https://docs-prv.pcisecuritystandards.org/PCI%20DSS/Supporting%20Document/PCI\_DSS-QRG-v4\_0.pdf](https://docs-prv.pcisecuritystandards.org/PCI%20DSS/Supporting%20Document/PCI_DSS-QRG-v4_0.pdf)
