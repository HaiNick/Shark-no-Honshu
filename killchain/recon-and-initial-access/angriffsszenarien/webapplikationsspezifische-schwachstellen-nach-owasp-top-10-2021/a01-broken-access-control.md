---
description: Fehlende oder fehlerhafte Autorisierungsmechanismen
---

# A01 – Broken Access Control

<details>

<summary>Links:</summary>

[https://owasp.org/www-project-top-ten/2017/A5\_2017-Broken\_Access\_Control.html](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control.html)

</details>

<details>

<summary>Siehe:</summary>

* Directory Traversal

- IDOR

</details>

***

## **Horizontal Privilege Escalation**

**Beschreibung:**\
Bei einer **horizontalen Rechteausweitung** (Horizontal Privilege Escalation) gelingt es einem authentifizierten Benutzer, auf Funktionen, Daten oder Ressourcen zuzugreifen, die einem anderen Benutzer mit demselben Berechtigungsniveau zugeordnet sind. Dies stellt eine Verletzung der Mandantentrennung oder Zugriffskontrolle dar und ermöglicht es einem Angreifer beispielsweise, fremde Kontodaten, Dokumente oder Sitzungen zu manipulieren, obwohl keine administrativen Rechte vorliegen.

**Beispiel:**\
Ein Benutzer ändert die Benutzer-ID in der URL von `/user/profile?id=1001` zu `/user/profile?id=1002` und erhält so Zugriff auf das Profil eines anderen Benutzers.

**Risikoeinstufung:**\
Diese Schwachstelle wird unter **Broken Access Control** (OWASP Top 10 – A01:2021) eingeordnet.

***

## **Vertical Privilege Escalation**

**Beschreibung:**\
Bei einer **vertikalen Rechteausweitung** (Vertical Privilege Escalation) verschafft sich ein Benutzer durch Ausnutzung einer Schwachstelle Zugriff auf Funktionen oder Daten, die höheren Berechtigungsstufen vorbehalten sind – etwa administrative Schnittstellen oder systemkritische Operationen. Dies führt zu einer ernsthaften Sicherheitsverletzung, da ein einfacher Benutzer privilegierte Aufgaben ausführen kann.

**Beispiel:**\
Ein nicht privilegierter Benutzer ruft `/admin/settings` direkt auf, ohne dass das System prüft, ob der Benutzer über administrative Berechtigungen verfügt.

**Risikoeinstufung:**\
Ebenfalls klassifiziert unter **Broken Access Control** (OWASP Top 10 – A01:2021), jedoch mit meist höherem Risiko aufgrund der möglichen Systemkompromittierung.
