# Blue Team

* [ ] **Netzwerk-Monitoring & Logging**\
  – IDS/IPS (z. B. Snort, Suricata), NetFlow, Syslog
* [ ] **Firewall & ACL-Konfiguration**
* [ ] **Segmentierung & VLANs**
* [ ] **Network Access Control (NAC)**
* [ ] **Zero Trust Networking**
* [ ] **Threat Hunting in Netzwerkdaten**
* [ ] **Detection von Scans / Anomalien**
* [ ] **SIEM-Integration von Netzwerkdaten**

## Einzuordnen

Zusammenfassend lässt sich sagen, dass Angriffe auf Anmeldesysteme mit einem Tool wie THC Hydra in Kombination mit einer geeigneten Wortliste effizient durchgeführt werden können. Der Schutz vor solchen Angriffen kann sehr komplex sein und hängt vom Zielsystem ab. Einige der Ansätze sind:

* Passwort-Richtlinie: Erzwingt Mindestkomplexitätsbeschränkungen für die vom Benutzer festgelegten Passwörter.&#x20;
* Kontosperrung: Sperrt das Konto nach einer bestimmten Anzahl von Fehlversuchen.&#x20;
* Authentifizierungsversuche drosseln: Verzögert die Antwort auf einen Anmeldeversuch. Eine Verzögerung von ein paar Sekunden ist für jemanden, der das Passwort kennt, tolerierbar, kann aber automatisierte Tools stark behindern.&#x20;
* Verwendung von CAPTCHA: Erfordert die Lösung einer für Maschinen schwierigen Frage. Es funktioniert gut, wenn die Anmeldeseite über eine grafische Benutzeroberfläche (GUI) erfolgt. (Hinweis: CAPTCHA steht für Completely Automated Public Turing test to tell Computers and Humans Apart).&#x20;
* Die Verwendung eines öffentlichen Zertifikats für die Authentifizierung verlangen. Dieser Ansatz funktioniert z. B. gut mit SSH.&#x20;
* Zwei-Faktoren-Authentifizierung: Aufforderung an den Benutzer, einen Code einzugeben, der über andere Wege verfügbar ist, z. B. per E-Mail, Smartphone-App oder SMS.&#x20;
* Es gibt viele andere Ansätze, die anspruchsvoller sind oder ein gewisses Maß an Wissen über den Nutzer erfordern, wie z. B. die IP-basierte Geolokalisierung.
