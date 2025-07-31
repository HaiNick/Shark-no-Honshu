# Threat Modelling & Incident Response

#### Threat Modelling

Threat Modelling ist ein strukturierter Prozess zur Analyse und Bewertung von Sicherheitsrisiken innerhalb einer IT-Infrastruktur oder Anwendung. Ziel ist es, potenzielle Bedrohungen frühzeitig zu identifizieren, Schwachstellen aufzudecken und geeignete Gegenmaßnahmen zu entwickeln. Das Verfahren ähnelt der Risikobewertung im Arbeitsschutz, ist jedoch auf IT-Systeme und digitale Prozesse ausgerichtet.

**Phasen des Threat Modelling:**

1. **Vorbereitung** – Erhebung relevanter Informationen über Systeme, Prozesse und Schutzobjekte
2. **Identifikation** – Analyse möglicher Bedrohungen und Schwachstellen
3. **Mitigation** – Ableitung und Umsetzung von Schutzmaßnahmen
4. **Review** – Überprüfung und kontinuierliche Verbesserung des Sicherheitsmodells

Ein umfassendes Threat Modelling sollte folgende Komponenten berücksichtigen:

* Threat Intelligence (Angreiferverhalten, aktuelle Bedrohungslage)
* Identifikation schützenswerter Assets
* Analyse bestehender Sicherheitsmechanismen
* Risikobewertung und Priorisierung

#### STRIDE-Modell

Ein häufig genutztes Framework zur Bedrohungsanalyse ist STRIDE, entwickelt von Microsoft. Es kategorisiert Sicherheitsbedrohungen in sechs Gruppen:

| Prinzip                    | Beschreibung                                                                                                                          |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| **Spoofing**               | Vortäuschung einer falschen Identität. Schutz durch Authentifizierungsmechanismen und Zugangskontrollen.                              |
| **Tampering**              | Unbefugte Veränderung von Daten oder Systemkonfigurationen. Schutz durch Integritätsprüfungen und kryptographische Signaturen.        |
| **Repudiation**            | Abstreitbarkeit von Aktionen ohne Nachweisbarkeit. Schutz durch lückenlose Protokollierung und Auditing.                              |
| **Information Disclosure** | Unbefugter Zugriff auf vertrauliche Informationen. Schutz durch Verschlüsselung und Zugriffsbeschränkungen.                           |
| **Denial of Service**      | Überlastung oder Ausfall von Systemen. Schutz durch Lastverteilung, Ratenbegrenzung und Redundanzen.                                  |
| **Elevation of Privilege** | Erlangung höherer Berechtigungen durch Ausnutzung von Schwachstellen. Schutz durch Härtung und Rechtevergabe nach dem Minimalprinzip. |

Alternative Ansätze wie das PASTA-Framework (Process for Attack Simulation and Threat Analysis) ergänzen STRIDE durch simulationsbasierte Methoden.

***

### Incident Response

Sicherheitsvorfälle können trotz umfassender Vorsorgemaßnahmen eintreten. Die strukturierte Reaktion auf solche Vorfälle wird als Incident Response bezeichnet. Ziel ist es, Schäden zu minimieren, die Ursache zu identifizieren und den Normalbetrieb wiederherzustellen.

Die Einschätzung eines Vorfalls erfolgt anhand:

* der **Dringlichkeit** (z. B. Art und Geschwindigkeit des Angriffs)
* der **Auswirkung** (z. B. betroffene Systeme, Einfluss auf Geschäftsprozesse)

Die Bearbeitung erfolgt durch ein speziell geschultes **Computer Security Incident Response Team (CSIRT)**.

#### Phasen der Incident Response:

| Phase               | Beschreibung                                                                        |
| ------------------- | ----------------------------------------------------------------------------------- |
| **Preparation**     | Aufbau von Prozessen, Ressourcen und Zuständigkeiten zur Reaktion auf Vorfälle      |
| **Identification**  | Erkennung und Bewertung eines Vorfalls sowie Identifikation des Angreifers          |
| **Containment**     | Begrenzung der Ausbreitung zur Vermeidung weiterer Schäden                          |
| **Eradication**     | Beseitigung der Ursache des Vorfalls                                                |
| **Recovery**        | Wiederherstellung des regulären Betriebs und Prüfung der Systemintegrität           |
| **Lessons Learned** | Analyse des Vorfalls zur Ableitung organisatorischer und technischer Verbesserungen |
