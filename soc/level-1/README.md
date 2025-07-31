# Level 1

Ein Security Operations Center (SOC) Analyst ist die erste Verteidigungslinie in einem Sicherheitsüberwachungsteam. Die Aufgabe von L1 SOC Analysts besteht darin, sicherheitsrelevante Ereignisse zu identifizieren, zu überwachen und erste Bewertungen vorzunehmen. Im Rahmen ihrer Tätigkeit setzen sie verschiedene Sicherheitstools und -plattformen ein, um potenzielle Bedrohungen frühzeitig zu erkennen und diese gemäß festgelegter Prozesse an die zuständigen Eskalationsstufen weiterzuleiten. Ihre Rolle ist von entscheidender Bedeutung für eine schnelle Reaktion auf Sicherheitsvorfälle und das tägliche Monitoring der IT-Infrastruktur eines Unternehmens.

**Typische Aufgaben:**

* Überwachung von Sicherheitsalarmen und -ereignissen (z. B. über SIEM-Systeme)
* Erste Analyse und Kategorisierung von Vorfällen
* Eskalation an L2/L3-Analysten bei komplexeren Bedrohungen
* Dokumentation von Vorfällen und Reaktionen
* Durchführung einfacher Maßnahmen zur Bedrohungsabwehr (z. B. Isolierung infizierter Systeme)

**Verwendete Tools und Plattformen:**

* SIEM-Systeme (z. B. Splunk, IBM QRadar, Azure Sentinel)
* Endpoint Detection and Response (EDR)-Lösungen
* Ticketing- und Incident-Management-Systeme
* Threat Intelligence Plattformen
* Firewalls, IDS/IPS-Systeme

**Erforderliche Fähigkeiten:**

* Grundkenntnisse in Netzwerksicherheit und IT-Systemen
* Vertrautheit mit Log-Analyse
* Fähigkeit zur schnellen Einschätzung von Sicherheitsereignissen
* Teamarbeit und klare Kommunikation
* Bereitschaft zur Schichtarbeit (häufig 24/7-Betrieb)



Zur Identifikation und Erhöhung der Visibilität sollten folgende Systeme/Methoden implementiert sein:

*   **Assets & Identities** (Identity Inventory, Asset Inventory, Netzwerkdiagramme, Work-/Run-/Playbooks zur Definition von Schritten):

    Workbooks – auch als Playbooks, Runbooks oder Workflows bezeichnet – sind strukturierte Dokumente, die klar definieren, welche Schritte zur Untersuchung und Behebung bestimmter Bedrohungen notwendig sind. Da L1 SOC Analysts als Junior-Fachkräfte gelten und nicht jedes Angriffsszenario perfekt einschätzen müssen, werden diese Anleitungen in der Regel von erfahrenen Analysten erstellt. L1-Analysten sollen – und müssen mitunter – Sicherheitsalarme strikt gemäß diesen Vorgaben bearbeiten, um Fehler zu vermeiden und eine effiziente, konsistente Analyse sicherzustellen.
*   Reporting

    Folgende Punkte sollten mindestens enthalten sein:

    * **Who (Wer):** Welcher Benutzer hat sich angemeldet, einen Befehl ausgeführt oder eine Datei heruntergeladen?
    * **What (Was):** Welche genaue Handlung oder Abfolge von Ereignissen wurde durchgeführt?
    * **When (Wann):** Wann genau begann und endete die verdächtige Aktivität?
    * **Where (Wo):** Welches Gerät, welche IP-Adresse oder welche Website war an dem Alarm beteiligt?
    * **Why (Warum):** Der wichtigste Punkt – die Begründung für die abschließende Bewertung des Vorfalls.
