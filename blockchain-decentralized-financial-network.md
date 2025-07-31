---
description: peer-to-peer software
---

# Blockchain (decentralized financial network)

tells Internet how to transfer secure information/ money /assets

Blockchain Layer, Bitcoin = Application

Blockchain wird Layer wie SMTP

Blockchain löst das Problem "mehrfach ausgegebenes Geld" (Double-Spend Problem), indem im Hintergrund eine Instanz (Blockchain) überwacht, dass es wirklich nur einmal ausgegeben wird.

dauert noch lange bis es komplett ausgerollt ist -> auslieferung applikationen dauert noch

keine Banken werden mehr als Zwischenstation benötigt

## Smartnetwork

Größere Komplexität im internetkabel durch große Checksums

Internet wird wie VPN



## Technischer Teil

Software-Wallets beinhalten Keys.&#x20;

Public+Private -Key zum Zugriff auf das Geld im Hauptbuchhaltung/Buchführungssystem (ledger system).

PKI-Infrastructure Publickey I(E)?ncryption Infrastructure

Public Key (32Bit-AlphaNumeric-Address) - Eigener Bitcoin-Adresse?

Private Key (Transaction)

Kauf Ablauf : Andere Adresse scannen/eingeben + Menge (Transaktion) -> Internet Software prüft ob Menge an Geld vorhanden (Ledger Balance) -> überweist Menge (Transaction Log) -> abgeschlossene Transaktion wird im Log aufgezeichnet.

Transaktion erst wenn Block gemined wurde

Alle 10min werden Blocks zusammengestellt -> alle 10 Minuten werden die Transaktions bestätigt

Backup?? Codes müssen dringend dupliziert gespeichert werden, da wenn z.b. das Handy ins Wasser fällt, kein Zugriff mehr auf das Konto möglich ist. Es gibt keine Wiederherstellungsmöglichkeiten!!

Bitcoin Demon; Nodes laufen verteilt

## Auditing mit Blockchain

Buchhalter können eine Blockchain als Audit Trail verwenden, da der Audit Trail eine geschützte chronologische Aufzeichnung ist. Und die Blockchain ist genau das.

Die Blockchain hat mit ihrer Peer-to-Peer-Systeminfrastruktur und ihren kryptografischen Fähigkeiten eine Menge zu bieten.

Beim [Auditing in der Informationstechnik](https://de.wikipedia.org/wiki/Auditing_\(Informationstechnik\)) geht es darum, sicherheitskritische Operationen von Softwareprozessen aufzuzeichnen. Dies betrifft insbesondere den Zugriff auf und die Veränderung von vertraulichen oder kritischen Informationen. Das Auditing eignet sich hierbei deshalb für eine Blockchain, weil es relativ geringe Datenmengen produziert und gleichzeitig hohe Sicherheitsanforderungen aufweist.

Eine Blockchain kann hierbei das _Audit-Log_ (auch als _Audit-Trail_ bezeichnet) vor Veränderung schützen. Zudem sollten die einzelnen Einträge mit einer [digitalen Signatur](https://de.wikipedia.org/wiki/Digitale_Signatur) versehen werden, um die Echtheit zu gewährleisten. Ein dezentraler Konsensmechanismus, wie bei Bitcoin, wird nicht zwingend benötigt.

Da einerseits vertrauliche Informationen gespeichert werden und andererseits kein Element der Blockchain gelöscht werden kann, ohne diese ungültig zu machen, kann zudem eine [Verschlüsselung](https://de.wikipedia.org/wiki/Verschl%C3%BCsselung) der einzelnen Einträge erfolgen. Da die Implementierung von Blockchains derzeit (Stand Mai 2017) mangels einfach zu verwendender Implementierungen sehr aufwändig ist, empfiehlt sich der Einsatz nur für besonders schützenswerte Informationen.

Einsatzbeispiele sind das Auditing bei Systemen für medizinische Informationen (z. B. [Elektronische Gesundheitsakte](https://de.wikipedia.org/wiki/Elektronische_Gesundheitsakte)), Verträgen und Geldtransaktionen mit hohem finanziellen Wert, militärischen Geheimnissen, der Gesetzgebung und der [elektronischen Stimmabgabe](https://de.wikipedia.org/wiki/Elektronische_Stimmabgabe), dem [Sicherheitsmanagement](https://de.wikipedia.org/wiki/Sicherheitsmanagement) kritischer Anlagen oder Daten von Großunternehmen, die unter den [Sarbanes-Oxley Act](https://de.wikipedia.org/wiki/Sarbanes-Oxley_Act) oder ähnliche Richtlinien fallen.

Wie im Juli 2018 bekannt wurde, testen die vier Wirtschaftsprüfungsgesellschaften [Deloitte](https://de.wikipedia.org/wiki/Deloitte), [KPMG](https://de.wikipedia.org/wiki/KPMG), [PricewaterhouseCoopers International](https://de.wikipedia.org/wiki/PricewaterhouseCoopers_International) und [Ernst & Young](https://de.wikipedia.org/wiki/Ernst_%26_Young) einen Blockchain-Dienst zur Prüfung der Zwischenberichte von Aktiengesellschaften. Ziel ist es, den Wirtschaftsprüfungsunternehmen die Möglichkeit zu geben, die Geschäftsvorgänge durch eine nachvollziehbare und manipulationssichere Datenkette auf dezentrale Weise zu verfolgen, wodurch der Bestätigungsprozess optimiert und automatisiert wird.

Dazu entwickelt und betreibt [ChainSecurity](https://de.wikipedia.org/w/index.php?title=ChainSecurity\&action=edit\&redlink=1) automatisierte Scan-Programme für [smarte Verträge](https://de.wikipedia.org/wiki/Smart_Contract). Anbieter solcher smarten Verträge können sich von der Firma auditieren und somit die Sicherheit ihrer Verträge garantieren lassen. Chainsecurity ist somit vergleichbar einem [TÜV](https://de.wikipedia.org/wiki/T%C3%9CV) für smarte Verträge und wurde im Jahr 2017 von dem [ETH](https://de.wikipedia.org/wiki/ETH_Z%C3%BCrich)-[Professor](https://de.wikipedia.org/wiki/Professor) Martin Vechev und den ehemaligen ETH-Doktoranden Hubert Ritzdorf und Petar Tsankov gegründet. Es verfolgt das Ziel die Blockchain-Technologien sicherer zu machen.

### --

Für einen **Informatiker** produziert die Blockchain-Technologie eine einfache Datenstruktur, die Blockchain, die Daten als Transaktionen in einzelnen Blöcken verkettet und in einem verteilten Peer-to-Peer-Netz redundant verwaltet. Die Alternative wäre eine konventionelle Datenbank, die kontinuierlich von allen Teilnehmern repliziert wird.

Für die **Cyber-Sicherheitsexperten** hat die Blockchain-Technologie den Vorteil, dass die Daten als Transaktionen in den einzelnen Blöcken manipulationssicher gespeichert werden können, das heißt, die Teilnehmer der Blockchain sind in der Lage, die Echtheit, den Ursprung und die Unversehrtheit der gespeicherten Daten (Transaktionen) zu überprüfen. Die Alternative wäre hier zum Beispiel ein [PKI](https://de.wikipedia.org/wiki/Public-Key-Infrastruktur)-System als zentraler Vertrauensdienstanbieter.

## Hash-Baum ( Merkle-Tree )

<figure><img src="https://www.researchgate.net/publication/334291891/figure/fig4/AS:778457525022720@1562610145642/The-architecture-of-Merkle-tree-in-the-blockchain.ppm" alt=""><figcaption><p>Merkle Tree in der Blockchain</p></figcaption></figure>

### SHA-1 ( [https://de.wikipedia.org/wiki/Secure\_Hash\_Algorithm](https://de.wikipedia.org/wiki/Secure_Hash_Algorithm) )

\--

### Whirlpool ( [https://de.wikipedia.org/wiki/Whirlpool\_(Algorithmus)](https://de.wikipedia.org/wiki/Whirlpool_\(Algorithmus\)) )

\--

### Tiger-Algorithmus ( [https://de.wikipedia.org/wiki/Tiger\_(Hashfunktion)](https://de.wikipedia.org/wiki/Tiger_\(Hashfunktion\)) )

Der erzeugte Hashwert hat eine Länge von 128, 160 oder 192 Bit

### Hashfunktionen

Eine der Hashfunktionen wird zum Hashen der Transaktionen innerhalb der Blöcke verwendet
