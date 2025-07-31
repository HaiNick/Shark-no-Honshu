# Group Policy Objects (GPO)

Windows verwaltet Richtlinien mithilfe von **Group Policy Objects (GPOs)**. GPOs sind Sammlungen von Einstellungen, die auf Organisationseinheiten (OUs) angewendet werden können. Sie enthalten Richtlinien für Benutzer oder Computer und ermöglichen es, spezifische Vorgaben für bestimmte Geräte und Benutzer festzulegen.

Zur Konfiguration von GPOs kann das **Group Policy Management Tool** verwendet werden, das im Startmenü verfügbar ist. 

<img src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/06d5e70fbfa648f73e4598e18c8e9527.png" alt="OU scope">

- **Scope** zeigt für welche OU die Policy gültig ist
- **Security Filtering** erlaubt die spezifische Zuordnung auf User oder Computer. Standardmäßig ist hier *Authenticated Users* eingetragen, was alle User/PCs beinhaltet
- **Settings** enthält die tatsächlichen Inhalte des GPO und zeigt, welche spezifischen Konfigurationen es anwendet. Dabei hat jedes GPO Einstellungen, die entweder nur für Computer oder nur für Benutzer gelten

**Beispiel:** Die Passwortpolicy um die minimale Passwortlänge festzulegen befindet sich unter `Computer Configurations -> Policies -> Windows Setting -> Security Settings -> Account Policies -> Password Policy`

**Audit-Richtlinie**
- Pfad: `Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration`

Jede Policy besitzt eine Erklärungsseite welche durch **Doppelklick** auf die jeweilige Policy und Auswahl von **Explain** aufrufbar ist.
Der Eintrag unter **Group Policy Objects** kann auf den rootpfad, hier thm.local oder eines der OUs darunter gezogen werden um die Policy darauf zuzuweisen.

## Verteilung von GPOs

GPOs werden über ein Netzwerkfreigabe-Verzeichnis namens **SYSVOL** im Netzwerk verteilt, das auf den Domain Controllern (DCs) gespeichert ist. Alle Benutzer einer Domäne haben normalerweise Zugriff auf diese Freigabe, um ihre GPOs regelmäßig zu synchronisieren. Standardmäßig verweist die SYSVOL-Freigabe auf den Ordner `C:\Windows\SYSVOL\sysvol\` auf jedem DC im Netzwerk.

Nach einer Änderung an den GPOs kann es bis zu **2 Stunden** dauern, bis alle Computer die Aktualisierungen erhalten. Um die Synchronisation eines bestimmten Computers sofort zu erzwingen, kann folgender Befehl auf diesem Computer ausgeführt werden:

```powershell
gpupdate /force
```