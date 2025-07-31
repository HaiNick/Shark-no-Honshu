# Pyramid of Pain

Ein bekanntes Konzept zur Verbesserung der Effektivität von CTI, Threat Hunting und Incident Response. -> Bestimmung des Schwierigkeitsgrades für einen Angreifer

<figure><img src="https://www.attackiq.com/wp-content/uploads/2019/06/blog-pyramid-pain-01-768x432.jpg" alt=""><figcaption></figcaption></figure>

Im Wesentlichen zeigt die "Pyramid of Pain", dass einige Indikatoren einer Kompromittierung für Angreifer beunruhigender sind als andere. Denn wenn diese Indikatoren einem Angreifer vorenthalten werden, ist der Verlust einiger Indikatoren für ihn schmerzhafter als der Verlust anderer

## Schwierigkeitsgrade

`Trivial [0]` -> `Leicht [1]` -> `Einfach [2]` -> `Lästig [3]` -> `Herausfordernd [4]` -> `Schwierig [5]`

## &#x20;Hash-Werte \[ 0 ]

SHA1, MD5 oder andere ähnliche Hash-Werte, die bestimmten verdächtigen oder bösartigen Dateien entsprechen. Hash-Werte werden häufig verwendet, um eindeutige Referenzen zu bestimmten Malware-Samples oder zu Dateien zu liefern, die an einem Eindringen beteiligt sind.

## IP-Adressen \[ 1 ]

Wie der Name schon sagt, aber auch Netzblöcke

* **Fast Flux** : Pool an IPs die bei der Kommunikation rotieren -> [https://unit42.paloaltonetworks.com/fast-flux-101/](https://unit42.paloaltonetworks.com/fast-flux-101/)

## Domain-Namen \[ 2 ]

Ein Domänenname selbst oder Unterdomänen

* **Punycode Attacke** : Genutzt zum Weiterleiten von Nutzern auf bösartige Domains. Bei Punycode werden Wörter, die nicht in ASCII geschrieben werden könnnen, in Unicode ASCII konvertiert.
* Verstecken von bösartigen Domains durch URL-Shortener (`bit.ly, goo.gl, ow.ly, s.id, smarturl.it, tiny.pl, tinyurl.com, x.co`). Wenn ein Link

## Netzwerk-Artefakte \[ 3 ]

Beobachtbare Netzwerkaktivitäten der Angreifer. Typische Beispiele sind URI-Muster, in Netzwerkprotokollen eingebettete C2-Informationen, auffällige HTTP User-Agent- oder SMTP Mailer-Werte usw. (tshark)

## Host-Artefakte \[ 3 ]

Beobachtungen, die durch Aktivitäten des Angreifers auf einem oder mehreren Ihrer Hosts verursacht werden, z. B. Registrierungsschlüssel oder Werte, die bekanntermaßen von bestimmten Malware-Teilen, Dateien oder Verzeichnissen erstellt werden.

Host-Artefakte sind die Spuren oder Beobachtungen, die Angreifer auf dem System hinterlassen, z. B. Registrierungswerte, verdächtige Prozessausführungen, Angriffsmuster oder IOCs (Indicators of Compromise), Dateien, die von bösartigen Anwendungen abgelegt wurden, oder alles, was ausschließlich die aktuelle Bedrohung betrifft.

## Tools \[ 4 ]

Software, die von Angreifern verwendet wird, um ihre Mission zu erfüllen. Dazu gehören Dienstprogramme zum Erstellen bösartiger Dokumente für Spear-Phishing, Backdoors zum Einrichten von C2 oder Passwort-Crackern oder andere Host-basierte Dienstprogramme.

## TTPs \[ 5 ]

Taktiken, Techniken und Verfahren (TTPs): Wie der Angreifer vorgeht, um seine Mission zu erfüllen, von der Aufklärung bis zur Datenexfiltration und bei jedem Schritt dazwischen

